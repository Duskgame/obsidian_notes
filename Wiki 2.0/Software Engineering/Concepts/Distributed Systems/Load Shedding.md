# Load Shedding

[Google SRE Book – Handling Overload](https://sre.google/sre-book/handling-overload/) | [AWS – Resilience](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/rel_mitigate_interaction_failure_throttle_requests.html)

Load shedding is the deliberate rejection or dropping of work when a system is overwhelmed, in order to protect it from complete failure. It trades partial availability for system stability.

---

## The Core Idea

It is better to handle 80% of requests well than to attempt 100% and crash entirely.

```
Without load shedding:
→ Queue grows unbounded
→ Memory exhausted
→ Latency spikes for all requests
→ System crashes — 0% served

With load shedding:
→ Excess requests rejected with 429
→ Queue stays bounded
→ Accepted requests processed normally
→ 80% served correctly
```

---

## Common Implementations

**Rate limiting** — reject requests above a threshold per client or globally
```
HTTP 429 Too Many Requests
```

**Queue overflow** — drop new jobs when the queue is full rather than queuing indefinitely
```
Queue capacity: 10,000 messages
Message 10,001 arrives → rejected or dropped
```

**Circuit Breaker** — stop sending requests to an overloaded downstream service entirely

**Connection pool exhaustion** — refuse new database connections when the pool is full

**Priority-based shedding** — drop low-priority work first, protect high-priority work
```
Drop analytics events before dropping payment events
```

---

## Load Shedding vs Backpressure

These are two different responses to overload:

| | [[Backpressure]] | Load Shedding |
|---|---|---|
| Response | Slow the producer down | Reject/drop excess work |
| Data loss | None — just slower | Yes — some work is lost |
| Applicable when | Producer can slow down | Producer cannot be slowed |
| Complexity | Higher | Lower |

Backpressure is more graceful when the producer can be controlled (internal systems). Load shedding is appropriate when the producer is external (user traffic, third-party services) or when slowing down is not feasible.

---

## Real-World Analogy

Power companies literally "shed load" by cutting electricity to certain areas during peak demand — sacrificing service to some customers to prevent the entire grid from going down.

---

## Related Topics

- [[Backpressure]] — the alternative to load shedding when the producer can be slowed
- [[Throughput and Latency]] — load shedding protects throughput by preventing latency from exploding
- [[Dead Letter Queue]] — DLQ captures messages that failed; load shedding drops them intentionally
- [[Circuit Breaker]] — stops calling overloaded services, a form of load shedding at the integration level
- [[Observability]] — load shedding events (rejected requests, dropped messages) must be monitored
