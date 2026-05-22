# Monotonic Clocks

[Node.js Docs – performance.now()](https://nodejs.org/api/perf_hooks.html#performancenow) | [MDN – Performance.now()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now)

A monotonic clock is a clock that only ever moves forward — it never jumps backwards. It is the correct tool for measuring elapsed time within a single process, but insufficient for ordering events across distributed nodes.

---

## Monotonic vs Wall Clock

**Wall clock** — real-world time (e.g. `Date.now()`, `System.currentTimeMillis()`)
- Can jump backwards — NTP correction, daylight saving, manual adjustment
- Unsafe for measuring durations or ordering events

**Monotonic clock** — counts forward from an arbitrary start point (e.g. process start)
- Never goes backwards
- Safe for measuring elapsed time within one process

```js
// Wall clock — UNSAFE for durations
const start = Date.now()
// ... clock gets adjusted by NTP ...
const elapsed = Date.now() - start  // could be negative or wrong

// Monotonic clock — SAFE for durations
const start = performance.now()
const elapsed = performance.now() - start  // always correct
```

---

## Why Monotonic Clocks Are Not Enough for Distributed Systems

Even with a perfectly monotonic clock, each machine's clock runs at a slightly different rate. There is no shared reference point across nodes:

```
Node A: event at t=500ms (from Node A's start)
Node B: event at t=499ms (from Node B's start)

These numbers are not comparable — they measure different things
```

For cross-node event ordering, you need logical clocks that track causality:

| Clock type | Reliable for | Scope |
|---|---|---|
| Wall clock | Displaying time to users | Single machine |
| Monotonic clock | Measuring durations, timeouts | Single process |
| [[Lamport Timestamps]] | Causal ordering of events | Distributed system |
| [[Vector Clocks]] | Detecting concurrent conflicts | Distributed system |

---

## Relevance to Conflict Resolution

When using **last-write-wins** conflict resolution in eventually consistent systems, you need timestamps. Wall clocks are unreliable (can go backwards); monotonic clocks are local only. This is why last-write-wins is risky — there is no reliable cross-node clock to determine which write was truly last.

---

## Related Topics

- [[Lamport Timestamps]] — logical clock that replaces physical time for distributed ordering
- [[Vector Clocks]] — tracks causality per node; avoids reliance on physical clocks
- [[Eventual Consistency]] — last-write-wins resolution depends on reliable timestamps
- [[CAP Theorem]] — clock reliability is a hidden constraint in distributed consistency
