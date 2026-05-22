# Lamport Timestamps

[Original Paper – Leslie Lamport, 1978](https://lamport.azurewebsites.net/pubs/time-clocks.pdf) | [Wikipedia – Lamport Timestamp](https://en.wikipedia.org/wiki/Lamport_timestamp)

Lamport timestamps are logical counters that capture the "happened-before" relationship between events across distributed nodes — without relying on real-time clocks.

---

## The Problem

Wall clocks on different machines drift and can jump backwards (NTP sync, manual adjustment). You cannot use them to determine which event happened first across nodes:

```
Node A: event at 10:00:00.500
Node B: event at 10:00:00.499  ← happened after A's event
                                  but timestamp says it came first
```

Lamport timestamps replace wall-clock time with a logical counter.

---

## The Three Rules

```
1. Every local event increments your own counter by 1
2. When sending a message, attach your current counter
3. When receiving a message:
   counter = max(local counter, received counter) + 1
```

---

## Example

```
Node A          Node B
counter=0       counter=0

A does event  → counter=1
A sends msg (timestamp=1) ─────→ B receives
                                  counter = max(0,1)+1 = 2
                        B does event → counter=3
                        B sends (timestamp=3)
A receives ←───────────────────────
counter = max(1,3)+1 = 4
A does event → counter=5
```

---

## The Key Property

If event A happened before event B:

```
timestamp(A) < timestamp(B)   ✓ always guaranteed
```

But the **reverse is not guaranteed**:

```
timestamp(A) < timestamp(B)   does NOT mean A happened before B
```

Two events with different timestamps might be **concurrent** — Lamport timestamps cannot distinguish between "A caused B" and "they just happened to get different counters."

---

## Lamport vs Vector Clocks

| | Lamport Timestamps | [[Vector Clocks]] |
|---|---|---|
| Detects happened-before | ✓ | ✓ |
| Detects concurrency | ✗ | ✓ |
| Storage | one integer | one integer per node |
| Complexity | simple | moderate |

Use Lamport timestamps when you just need a total ordering of events. Use [[Vector Clocks]] when you need to detect and handle concurrent conflicting updates.

---

## Related Topics

- [[Vector Clocks]] — extension that adds per-node tracking to detect concurrency
- [[Monotonic Clocks]] — physical clock alternative; reliable within one machine, not across nodes
- [[CAP Theorem]] — ordering events correctly is essential in AP systems with partition tolerance
- [[Eventual Consistency]] — causal ordering matters when reconciling diverged nodes
