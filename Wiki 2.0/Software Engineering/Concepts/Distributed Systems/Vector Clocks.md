# Vector Clocks

[Wikipedia – Vector Clock](https://en.wikipedia.org/wiki/Vector_clock) | [Riak Docs – Vector Clocks](https://docs.riak.com/riak/kv/2.2.3/learn/concepts/causal-context/index.html)

Vector clocks extend [[Lamport Timestamps]] by tracking a counter *per node*, allowing a system to determine whether two events are causally related or truly concurrent. They are the mechanism behind conflict detection in AP distributed systems.

---

## Structure

Each node maintains a vector — one counter per node in the system:

```
Node A: [A=0, B=0, C=0]
Node B: [A=0, B=0, C=0]
Node C: [A=0, B=0, C=0]
```

---

## The Three Rules

```
1. Every local event increments your own counter
2. When sending a message, attach your full vector
3. When receiving a message:
   - take element-wise max of both vectors
   - then increment your own counter
```

---

## Example (2 nodes)

```
Node A              Node B
[A=0, B=0]          [A=0, B=0]

A does event    →   [A=1, B=0]
A sends ─────────────────────→ B receives
                               max([A=1,B=0],[A=0,B=0]) + own++
                               [A=1, B=1]
                   B does event → [A=1, B=2]
                   B sends ←──────────────────
A receives
max([A=1,B=0],[A=1,B=2]) + own++
[A=2, B=2]
```

---

## Comparing Two Vector Clocks

**X happened before Y** — every counter in X ≤ Y, and at least one is strictly less:
```
[A=1, B=0]  vs  [A=1, B=2]  → first happened before second ✓
```

**X and Y are concurrent** — X has at least one counter greater than Y, and Y has at least one greater than X:
```
[A=2, B=1]  vs  [A=1, B=2]  → CONCURRENT — neither caused the other
```

---

## Why Concurrency Detection Matters

When two nodes independently update the same value during a partition, you have a **conflict** that must be resolved:

```
Node A: user sets name = "Alice"  →  [A=1, B=0]
Node B: user sets name = "Bob"    →  [A=0, B=1]

These are concurrent — neither knew about the other.
System must decide: last write wins? merge? ask the user?
```

[[Lamport Timestamps]] would silently assign different numbers and hide the conflict. Vector clocks expose it honestly so the application can handle it.

---

## Conflict Resolution Options

| Strategy | Description |
|---|---|
| Last write wins | Discard older by timestamp — simple, loses data |
| Merge | Combine both values if domain allows (e.g. shopping cart) |
| User resolution | Expose conflict to the user to pick |
| CRDT | Data structure that merges automatically |

---

## Summary

| | Lamport | Vector |
|---|---|---|
| Detects happened-before | ✓ | ✓ |
| Detects concurrency | ✗ | ✓ |
| Storage cost | one integer | one integer per node |
| Complexity | simple | moderate |

---

## Used In

- **DynamoDB** — uses vector-clock-like versioning for conflict detection
- **Riak** — explicit vector clocks per object
- **CRDTs** — use causal context (similar to vector clocks) to merge without conflicts
- **Offline-first sync** — detect conflicts when a mobile client reconnects

---

## Related Topics

- [[Lamport Timestamps]] — simpler predecessor; cannot detect concurrency
- [[CAP Theorem]] — AP systems need vector clocks to handle partition-induced conflicts
- [[Eventual Consistency]] — vector clocks determine which diverged writes conflict
- [[Offline-first Sync]] — mobile sync faces the same concurrency challenges
