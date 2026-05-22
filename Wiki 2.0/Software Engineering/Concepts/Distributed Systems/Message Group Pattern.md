# Message Group Pattern

[AWS SQS FIFO – Message Group ID](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagegroupid-property.html) | [Enterprise Integration Patterns](https://www.enterpriseintegrationpatterns.com/)

The message group pattern partitions a queue's messages by a key (the group ID), guaranteeing order *within* each group while allowing parallel processing *across* groups. It is the practical middle ground between full FIFO and unordered queues.

---

## The Problem

**Full FIFO** — all messages processed in one sequence
- Ordering is guaranteed globally
- No parallelism possible — bottleneck

**No ordering** — all messages processed concurrently
- Maximum throughput
- No ordering guarantees — can corrupt dependent operations

**Message groups** — ordered per group, parallel across groups:
```
Group "user-1": [A] → [B] → [C]   (sequential within group)
Group "user-2": [X] → [Y] → [Z]   (sequential within group)
Group "user-3": [P] → [Q]          (sequential within group)

All three groups process in parallel ✓
```

---

## How It Works

Each message is tagged with a **group ID** when published. The queue routes all messages with the same group ID to the same consumer, in order. Different groups can be routed to different consumers simultaneously.

```
Message { groupId: "user-1", seq: 1, data: "place order" }
Message { groupId: "user-1", seq: 2, data: "charge card" }   ← must come after seq 1
Message { groupId: "user-2", seq: 1, data: "place order" }   ← independent of user-1
```

---

## Choosing the Group Key

The group key should reflect the **unit of consistency** your business logic requires — the boundary within which order matters.

| Domain | Group key |
|---|---|
| E-commerce | `orderId` or `userId` |
| Banking | `accountId` |
| IoT sensors | `deviceId` |
| Chat | `conversationId` |

Events from different users/accounts/devices are independent — no reason to serialise them.

---

## Order Scope

This pattern is an application of [[Total and Partial Order]]:
- **Total order** within a group
- **Partial order** across groups — groups are concurrent and have no defined relationship to each other

The group boundary defines where ordering is enforced.

---

## AWS Implementation

AWS SQS FIFO queues implement this directly via the `MessageGroupId` attribute:
- All messages with the same `MessageGroupId` are delivered in order
- Different group IDs are processed in parallel
- FIFO queues also support [[Deduplication ID]] per group

---

## Related Topics

- [[Total and Partial Order]] — groups create total order within scope, partial order across scopes
- [[Delivery Semantics]] — ordering guarantees interact with at-least-once / exactly-once delivery
- [[Deduplication ID]] — used alongside group IDs in FIFO queues to prevent duplicates
- [[Messaging Patterns]] — the broader context of asynchronous message-based communication
- [[CAP Theorem]] — ordering guarantees and consistency are connected trade-offs
