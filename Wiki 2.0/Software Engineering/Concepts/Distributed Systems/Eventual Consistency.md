# Eventual Consistency

[Amazon – Eventual Consistency](https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/eventual-consistency.html) | [Martin Fowler – LMAX](https://martinfowler.com/articles/lmax.html)

Eventual consistency is a consistency model where, if no new updates are made, all nodes in a distributed system will *eventually* converge to the same value. Reads may return stale data during the convergence window.

> **Relevance:** Understanding this is critical when designing the Kwizz sync strategy — the app will face this trade-off when syncing offline changes.

---

## How It Works

```
User writes value=10 to Node A:

Node A: value=10  ← up to date
Node B: value=5   ← stale (replication in progress)

...after replication propagates...

Node A: value=10  ✓
Node B: value=10  ✓
```

The window between write and full propagation is where reads can return stale data. This is the accepted trade-off in AP systems (see [[CAP Theorem]]).

---

## Eventual vs Strong Consistency

| | Strong Consistency | Eventual Consistency |
|---|---|---|
| Read after write | always latest | might be stale |
| Availability | lower | higher |
| Throughput | lower | higher |
| Complexity | simpler to reason about | requires conflict handling |

---

## Conflict Resolution Strategies

When two nodes independently update the same value during a partition, conflicts arise that must be resolved when the partition heals:

**Last Write Wins (LWW)** — discard the earlier write by timestamp
- Simple, but loses data
- Requires reliable clocks (see [[Monotonic Clocks]])

**[[Vector Clocks]]** — track causality to detect concurrent conflicting writes
- Exposes conflicts rather than silently discarding one
- Application or user must resolve

**CRDTs (Conflict-free Replicated Data Types)** — data structures that merge automatically without conflicts
- e.g. counters, sets, sequences with defined merge rules
- Used by Redis, Riak, collaborative editors

**Read-repair** — when a stale value is read, fix it lazily in the background

---

## Patterns for Living with Eventual Consistency

**Read your own writes** — route a user's reads to the same node they just wrote to, so they see their own updates immediately even if replication hasn't finished.

**Monotonic reads** — ensure a user never sees a value *older* than one they've already seen (once you read value=10, you won't see value=5 again).

**Idempotent writes** — safe to apply the same update multiple times during reconciliation without corrupting state.

---

## When to Use

Appropriate when:
- Brief staleness is acceptable (social media feeds, DNS, CDN caches)
- High availability and throughput matter more than perfect consistency
- Conflicts are rare or resolvable

Not appropriate when:
- Correctness is critical (bank balances, inventory counts, auth tokens)
- Users need to immediately see the effects of their own actions

---

## Related Topics

- [[CAP Theorem]] — eventual consistency is the consistency model of AP systems
- [[Vector Clocks]] — detect concurrent conflicting writes in eventually-consistent systems
- [[Monotonic Clocks]] — clock reliability matters for last-write-wins conflict resolution
- [[Offline-first Sync]] — mobile offline sync is a local form of eventual consistency
- [[Messaging Patterns]] — async message delivery leads to eventual consistency between services
