# Total and Partial Order

[Wikipedia – Partially ordered set](https://en.wikipedia.org/wiki/Partially_ordered_set) | [Wikipedia – Total order](https://en.wikipedia.org/wiki/Total_order)

In distributed systems, "order" describes whether events can be sorted into a single definitive sequence. Total order means every event has a clear position; partial order means some events are concurrent and have no defined relationship to each other.

---

## Total Order

Every pair of events can be compared — there is one definitive sequence for all events.

```
1 < 2 < 3 < 4 < 5   — every element has a defined position relative to all others
```

In messaging systems, **FIFO (First In, First Out)** provides total order within a queue: every message has a clear sequence number.

**Cost:** Total order requires serialisation — nothing can be processed in parallel. This limits throughput.

---

## Partial Order

Some events can be compared (one clearly happened before the other), but others cannot — they are **concurrent** and have no defined ordering relationship.

```
Task A must finish before Task C
Task B must finish before Task C
A and B have no relationship — either can run first

A ─┐
   ├─→ C
B ─┘
```

A and B are concurrent — processing them in parallel is safe.

**Benefit:** Partial order allows parallelism wherever events are truly independent.

---

## Connection to Distributed Systems

In a distributed system, events on different nodes happen concurrently with no global clock. This naturally produces a partial order:

- Events on the *same node* are totally ordered (you know their sequence)
- Events on *different nodes* may be concurrent — without a causal relationship, no ordering can be determined

[[Lamport Timestamps]] and [[Vector Clocks]] are tools for determining the partial order of events across nodes:
- Lamport: can tell you "A happened before B" but cannot detect concurrency
- Vector Clocks: can detect when two events are truly concurrent (no causal relationship)

---

## Total Order vs Partial Order in Queues

| Queue type | Order guarantee | Parallelism |
|---|---|---|
| FIFO (global) | Total order — one sequence for all | None — single consumer |
| Message groups | Total within each group, partial across groups | Yes — parallel across groups |
| No ordering | No guarantees | Maximum |

The [[Message Group Pattern]] is essentially a way to enforce total order within a meaningful scope (e.g. per user) while keeping partial order — and therefore parallelism — across scopes.

---

## Related Topics

- [[Lamport Timestamps]] — assigns a total order to events using logical counters
- [[Vector Clocks]] — reveals the partial order by detecting concurrency
- [[Message Group Pattern]] — applies partial order to message queues for practical throughput
- [[CAP Theorem]] — consistency in distributed systems is fundamentally a question of ordering
- [[Eventual Consistency]] — AP systems accept partial ordering and reconcile later
