# CAP Theorem

[Wikipedia – CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem) | [Martin Kleppmann – Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)

A distributed system can only guarantee **2 out of 3** properties simultaneously: Consistency, Availability, and Partition Tolerance. Since network partitions are unavoidable in real distributed systems, the real trade-off is always between **C and A** during a partition.

---

## The Three Properties

| Property | Meaning |
|---|---|
| **Consistency (C)** | Every read returns the most recent write (or an error) |
| **Availability (A)** | Every request receives a response — no timeouts or errors |
| **Partition Tolerance (P)** | System continues operating even when network links between nodes fail |

Partition tolerance is **not optional** — networks fail in the real world. So the choice reduces to: what does the system do *during a partition*?

---

## CP vs AP

**CP system** — prioritises consistency, sacrifices availability during a partition:
- Refuses reads/writes until nodes can communicate again
- "I'd rather return an error than stale data"
- Examples: ZooKeeper, HBase, etcd, most relational DBs in a cluster

**AP system** — prioritises availability, sacrifices consistency during a partition:
- Continues answering requests using local (potentially stale) state
- Nodes may diverge; reconciliation happens after the partition heals
- Examples: Cassandra, DynamoDB, CouchDB, DNS

```
Partition occurs between Node A and Node B:

CP: Node A refuses requests
    → user sees error
    → data is never stale ✓

AP: Node A keeps answering with its last known state
    → user gets a response ✓
    → data may be stale or conflict with Node B
```

---

## Eventual Consistency

The standard trade-off for AP systems. Nodes are allowed to diverge during a partition, but *will converge* once the partition heals — given no further updates.

> "If you stop writing, all nodes will eventually agree."

This window of divergence requires strategies like:
- **Last write wins** — discard earlier writes by timestamp
- **[[Vector Clocks]]** — detect and expose conflicting concurrent writes
- **[[CRDTs]]** — data structures that merge automatically without conflicts
- **Read-repair** — fix stale data lazily during reads

---

## Real-World Nuance

CAP is a simplified model. In practice:
- Most systems are **neither purely CP nor AP** — they offer tunable consistency levels
- DynamoDB, Cassandra: configurable consistency per request
- The theorem only applies during an *active* partition; outside of one, you can have both C and A

---

## Summary

| System type | During a partition | Example |
|---|---|---|
| CP | Returns errors | ZooKeeper, etcd |
| AP | Returns stale data | Cassandra, DynamoDB |
| Tunable | Configurable per request | DynamoDB (consistency modes) |

---

## Related Topics

- [[Eventual Consistency]] — the consistency model most AP systems adopt
- [[Vector Clocks]] — mechanism to detect conflicting concurrent writes in AP systems
- [[Messaging Patterns]] — messaging systems also make CP/AP trade-offs
- [[Offline-first Sync]] — mobile apps face the same CP/AP choice when offline
- [[Database Partitioning]] — partitioning affects which CAP trade-offs apply
