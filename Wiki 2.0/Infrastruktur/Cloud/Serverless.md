# Serverless Computing

[CNCF Serverless Whitepaper](https://github.com/cncf/wg-serverless/tree/master/whitepapers/serverless-overview) | [AWS Serverless](https://aws.amazon.com/serverless/)

Serverless is a cloud execution model where the provider dynamically manages infrastructure allocation — you deploy code (functions), not servers. You pay only for actual execution time, not idle capacity.

---

## How it differs from traditional hosting

| Traditional Server / VM | Serverless |
|---|---|
| Always running, always billed | Only runs (and bills) on invocation |
| You manage OS, runtime, scaling | Provider handles everything below your code |
| Predictable latency | Potential cold start delay |
| Long-running processes supported | Max execution time (e.g. 15 min on AWS Lambda) |
| Fixed capacity | Scales automatically to zero or to millions |

> In a traditional setup you rent a kitchen that runs 24/7. In serverless, the kitchen only fires up when an order comes in.

---

## Execution model

```
Trigger (HTTP, event, schedule)
        ↓
Provider spins up function instance
        ↓
Function executes
        ↓
Instance idles or is torn down
```

Functions are **stateless** — no shared memory between invocations. State must be stored externally (database, cache, object storage).

---

## Cold Start

When a function hasn't been invoked recently, the provider needs to initialise a new instance. This first invocation takes longer — typically 100ms–1s.

**Mitigation strategies:**
- Keep functions small (faster init)
- Use provisioned/reserved concurrency (keeps instances warm, costs more)
- Avoid heavy dependencies in the function package

Cold start is less of an issue for event-driven background processing; it matters most for latency-sensitive APIs.

---

## Common triggers

| Trigger type | Example |
|---|---|
| HTTP request | API endpoint hit |
| Message queue | New item in queue |
| Object storage event | File uploaded |
| Schedule (cron) | Run every night at 2am |
| Database stream | Row inserted/updated |
| Another function | Chained invocations |

---

## Use cases

**Good fit:**
- Event-driven background processing
- Scheduled jobs
- Webhook handlers
- APIs with unpredictable or spiky traffic

**Poor fit:**
- Long-running processes (> provider limit)
- Workloads requiring persistent in-memory state
- Steady, high-traffic APIs where a warm server is cheaper

---

## Provider implementations

| Provider | Service |
|---|---|
| AWS | Lambda |
| Google Cloud | Cloud Functions / Cloud Run |
| Azure | Azure Functions |
| Cloudflare | Workers |
| Self-hosted | OpenFaaS, Knative |

---

## Related Topics

- [[AWS]] — AWS Lambda is the most widely used serverless platform
- [[Cloud Native]] — serverless is one of several cloud-native compute models
- [[Kubernetes]] — alternative for containerised workloads; more control, more complexity
- [[API Gateway]] — common front door for serverless HTTP endpoints
- [[Messaging Patterns]] — queues and event buses are the most common serverless triggers
