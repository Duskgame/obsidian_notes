# Throughput and Latency

[Google SRE Book – Latency](https://sre.google/sre-book/monitoring-distributed-systems/) | [AWS – Performance Efficiency](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/welcome.html)

Throughput and latency are the two primary dimensions of system performance. They often trade off against each other, and choosing the right balance depends on what the use case actually requires.

---

## Definitions

**Throughput** — how much work a system completes per unit of time
```
10,000 messages/second
500 requests/second
100 MB/second
```

**Latency** — how long a single unit of work takes from start to finish
```
5ms to process one message
200ms to return one API response
```

---

## The Tension Between Them

Optimising one often hurts the other.

**Batching** increases throughput, adds latency:
```
Collect 100 messages → process all at once
→ Each message waits up to N milliseconds before being handled
→ But the system processes far more per second
```

**Immediate processing** minimises latency, limits throughput:
```
Process each message the instant it arrives
→ Low wait time per message
→ Cannot sustain high volume — no opportunity to amortise overhead
```

---

## Relevance to Messaging Patterns

| Pattern / Guarantee | Effect on throughput | Effect on latency |
|---|---|---|
| FIFO global ordering | ↓ throughput (serialised) | ↑ latency (queue waits) |
| [[Message Group Pattern]] | moderate throughput | moderate latency |
| [[Delivery Semantics#Exactly Once\|Exactly-once delivery]] | ↓ throughput (coordination) | ↑ latency |
| [[Load Shedding]] | protects throughput | prevents latency spikes |
| [[Backpressure]] | reduces throughput temporarily | prevents latency from exploding |

---

## Typical Priority by Use Case

| Use case | Priority |
|---|---|
| Chat, live collaboration | Latency — messages must feel instant |
| Data pipelines, ETL | Throughput — process millions of records |
| Payment processing | Correctness — neither throughput nor latency; accuracy |
| Video streaming | Both — enough throughput for the bitrate, low enough latency to buffer |

---

## Percentile Latency (p50, p99)

Average latency hides the worst cases. Use percentiles:

```
p50 = 5ms    — half of requests complete within 5ms
p95 = 50ms   — 95% within 50ms
p99 = 500ms  — 99% within 500ms (1% are slow)
p999 = 2s    — the slowest 0.1% take 2 seconds
```

Production SLAs are usually defined at p99 or p999 — the tail latency experienced by real users.

---

## Related Topics

- [[Load Shedding]] — deliberately reduces throughput to prevent latency from degrading under load
- [[Backpressure]] — slows producers to protect consumers, trading throughput for stability
- [[Messaging Patterns]] — queue designs explicitly trade throughput vs latency
- [[Observability]] — latency percentiles and throughput metrics are the core of production monitoring
