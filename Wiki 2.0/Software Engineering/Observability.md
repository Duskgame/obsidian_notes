# Observability

[OpenTelemetry Docs](https://opentelemetry.io/docs/) | [CNCF Observability Whitepaper](https://github.com/cncf/tag-observability/blob/main/whitepaper.md)

Observability is the ability to understand the internal state of a system by examining the data it produces externally. It is a prerequisite for operating distributed systems reliably in production.

---

## The Three Pillars

### Logs
Discrete, timestamped records of events that occurred in the system.

- Answer: **What happened?**
- Format: structured (JSON) or unstructured text
- Example: `2026-05-21 14:32:01 ERROR user 42 login failed: invalid token`

**Tools:** Grafana Loki, Elasticsearch/OpenSearch, AWS CloudWatch Logs, Splunk

---

### Metrics
Numeric measurements of system state sampled over time (time-series data).

- Answer: **How is the system performing right now?**
- Examples: requests/second, CPU usage %, error rate, queue depth
- Typically collected by scraping or pushing at regular intervals

```
http_requests_total{method="POST", status="500"} 42
process_cpu_seconds_total 0.87
```

**Tools:** Prometheus, AWS CloudWatch Metrics, Datadog, Grafana

---

### Traces
Records of the full journey of a single request through a distributed system, showing each service call, its duration, and whether it succeeded.

- Answer: **Where did this specific request go, and how long did each step take?**
- A trace is made up of **spans** — one span per service or operation
- Requires instrumentation in every service that participates

```
Trace: request-id abc123
├── API Gateway         5ms
├── Auth Service       12ms
├── Quiz Service       45ms
│   ├── DB Query       38ms
│   └── Cache lookup    4ms
└── Response           2ms
Total: 64ms
```

**Tools:** Jaeger, Zipkin, AWS X-Ray, Datadog APM, Grafana Tempo

---

## OpenTelemetry (OTel)

OpenTelemetry is the **standard** for instrumenting code to emit logs, metrics, and traces — vendor-neutral. You add OTel to your app once; it can export to any compatible backend (Jaeger, Datadog, Grafana, etc.).

Avoids vendor lock-in: switching from Jaeger to Datadog does not require re-instrumenting your code.

---

## Observability Tools Overview

| Category | Open Source | AWS Managed | Commercial |
|---|---|---|---|
| **Logs** | Grafana Loki, OpenSearch | CloudWatch Logs | Splunk, Datadog |
| **Metrics** | Prometheus | CloudWatch Metrics | Datadog, New Relic |
| **Traces** | Jaeger, Zipkin | X-Ray | Datadog APM |
| **Dashboards** | Grafana | CloudWatch Dashboards | Datadog, Dynatrace |
| **All-in-one** | Grafana Stack | CloudWatch | Datadog, Dynatrace |

---

## Observability vs Monitoring

| Monitoring | Observability |
|---|---|
| Checks known failure modes ("is CPU > 90%?") | Enables investigation of unknown failures |
| Alerts when thresholds are crossed | Provides data to answer "why did this break?" |
| Reactive | Exploratory |

Monitoring tells you *something is wrong*. Observability lets you figure out *what and why*.

---

## Related Topics

- [[Cloud Native]] — observability is a core pillar of cloud-native systems
- [[Kubernetes]] — K8s clusters require dedicated observability to operate in production
- [[Circuit Breaker]] — circuit breaker events feed into observability pipelines
- [[Messaging Patterns]] — queue depth and consumer lag are key metrics to track
