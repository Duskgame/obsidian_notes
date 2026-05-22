# Cloud Functions Gen2

[GCP Docs — Cloud Functions Gen2](https://cloud.google.com/functions/docs/concepts/version-comparison) | [Gen2 under the hood](https://cloud.google.com/functions/docs/concepts/execution-environment)

Cloud Functions Gen2 is GCP's second-generation serverless function platform. Unlike Gen1 (which had its own isolated runtime), Gen2 functions run on top of **Cloud Run** — every function is internally a Cloud Run service. This gives Gen2 higher concurrency limits, longer timeouts, larger instances, and direct access to Artifact Registry images.

> **Relevance:** All Cloud Functions in the SAKE project (`wip-gcp-project-key-rotation`) — `smtpsender`, `jiracommenter`, `adgroups` — are deployed as Gen2.

---

## Gen1 vs Gen2

| Feature | Gen1 | Gen2 |
|---|---|---|
| Underlying runtime | Custom sandbox | Cloud Run |
| Max timeout | 9 min (HTTP), 10 min (background) | 60 min |
| Max concurrency per instance | 1 | 1000 |
| Max memory | 8 GB | 32 GB |
| Max vCPUs | 4 | 8 |
| Traffic splitting | No | Yes (via Cloud Run) |
| Min instances | Yes | Yes |
| Source deployment | GCS zip | GCS zip → Artifact Registry image |
| Direct Cloud Run access | No | Yes (same service) |

---

## Deployment pipeline (Terraform)

Gen2 functions are deployed from a zipped Python/Node/etc. source stored in GCS. GCP builds a container image automatically and pushes it to Artifact Registry.

```hcl
# 1. Zip the source directory
data "archive_file" "smtpsender_zip" {
  type        = "zip"
  source_dir  = "${path.module}/cloudfunctions/smtpsender"
  output_path = "/tmp/smtpsender.zip"
}

# 2. Upload zip to GCS
resource "google_storage_bucket_object" "smtpsender_source" {
  name   = "smtpsender-${data.archive_file.smtpsender_zip.output_md5}.zip"
  bucket = google_storage_bucket.cloudfunction_sources.name
  source = data.archive_file.smtpsender_zip.output_path
}

# 3. Artifact Registry repo (Gen2 needs this)
resource "google_artifact_registry_repository" "cf_images" {
  repository_id = "cf-images"
  format        = "DOCKER"
  location      = "europe-west1"
}

# 4. Deploy Gen2 function
resource "google_cloudfunctions2_function" "smtpsender" {
  name     = "smtpsender"
  location = "europe-west1"

  build_config {
    runtime     = "python311"
    entry_point = "hello_http"
    source {
      storage_source {
        bucket = google_storage_bucket.cloudfunction_sources.name
        object = google_storage_bucket_object.smtpsender_source.name
      }
    }
    service_account = google_service_account.cf_builder.email
    docker_repository = google_artifact_registry_repository.cf_images.id
  }

  service_config {
    available_memory      = "256M"
    timeout_seconds       = 60
    max_instance_count    = 10
    service_account_email = google_service_account.smtp_sender.email

    vpc_connector                  = "projects/${var.project_id}/locations/europe-west1/connectors/con-dev-euw1-srvless01"
    vpc_connector_egress_settings  = "ALL_TRAFFIC"
    ingress_settings               = "ALLOW_INTERNAL_AND_GCLB"

    environment_variables = {
      PROJECT_ID = var.project_id
    }
  }
}
```

---

## Entry point

The function's entry point is a plain Python HTTP handler — no framework required:

```python
# cloudfunctions/smtpsender/main.py
import functions_framework

@functions_framework.http
def hello_http(request):
    # request is a flask.Request object
    subject = request.form.get("subject")
    # ... handle request
    return "OK", 200
```

The `functions_framework` package wraps the function in a local Flask server for both local dev and production execution.

---

## Networking in Gen2

Gen2 functions run in GCP's serverless infrastructure (no VPC by default). A **VPC connector** tunnels traffic into a private VPC:

```
Cloud Function (Gen2)
  │
  │ egress: ALL_TRAFFIC (all outbound via connector)
  ▼
VPC Access Connector (con-dev-euw1-srvless01)
  │
  ▼
Internal VPC Network
  │
  ├─► Internal services (databases, other CFs via LB)
  └─► Internet (via Cloud NAT or egress gateway)
```

`ingress: ALLOW_INTERNAL_AND_GCLB` means only traffic from within the VPC or via the GCP load balancer reaches the function — direct internet calls to the function URL are blocked.

---

## Secret mounting

Gen2 supports mounting Secret Manager secrets as environment variables or volume files:

```hcl
service_config {
  secret_environment_variables {
    key        = "SMTP_PASSWORD"
    project_id = var.project_id
    secret     = "smtp_password"
    version    = "latest"
  }
}
```

The function reads it as a normal env var: `os.environ["SMTP_PASSWORD"]`.

---

## Observability

Gen2 functions write logs to **Cloud Logging** automatically. Structured logs (JSON) are parsed and queryable:

```python
import google.cloud.logging
client = google.cloud.logging.Client()
client.setup_logging()

import logging
logging.info({"message": "key created", "sa": sa_email, "key_id": key_id})
```

Metrics (invocation count, latency, error rate) appear automatically in Cloud Monitoring.

---

## Summary

| Aspect | Detail |
|---|---|
| Runtime | python311, nodejs20, go122, java21, etc. |
| Trigger | HTTP (Gen2 default), Pub/Sub, Eventarc |
| Backed by | Cloud Run (same service, same console) |
| Source format | ZIP in GCS → auto-built container image |
| Image storage | Artifact Registry (required for Gen2) |
| Max timeout | 60 min |
| Networking | VPC connector for private network access |

---

## Related Topics

- [[Serverless]] — general serverless computing concepts
- [[Service Account Key Rotation]] — SAKE project deploying three Gen2 functions
- [[Workload Identity Federation]] — keyless auth pattern used alongside Gen2 in CI/CD
- [[GoogleCloud]] — GCP platform overview
- [[VPC]] — VPC connector required for private network access
