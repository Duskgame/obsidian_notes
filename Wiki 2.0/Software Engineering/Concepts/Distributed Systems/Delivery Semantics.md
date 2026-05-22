# Delivery Semantics

[AWS SQS – Message Delivery](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) | [Apache Kafka – Delivery Semantics](https://kafka.apache.org/documentation/#semantics)

Delivery semantics define how many times a message is guaranteed to be delivered in a messaging system. The choice determines how you must design your consumers.

---

## The Three Guarantees

| Semantic | Delivery count | Data loss? | Duplicates? |
|---|---|---|---|
| **At most once** | 0 or 1 times | Possible | Never |
| **At least once** | 1 or more times | Never | Possible |
| **Exactly once** | Exactly 1 time | Never | Never |

---

## At Most Once

The message is sent and forgotten — no retry if delivery fails. Simple and fast, but data can be lost.

```
Producer sends message → Consumer crashes before processing
                       → Message is gone — no retry
```

Use when: losing some messages is acceptable (metrics, analytics, non-critical notifications).

---

## At Least Once

The message is retried until acknowledged. The consumer must confirm processing with an `ACK`; if no ACK arrives, the message is re-delivered.

```
Producer sends → Consumer processes → forgets to ACK → re-delivered → processed again
```

This is the most common guarantee in practice. Consumers **must be idempotent** — processing the same message twice must produce the same result as processing it once.

```js
// NOT idempotent — running twice charges twice
await stripe.charge(customer, amount)

// Idempotent — running twice has the same effect as once
await db.upsert({ id: event.id, status: 'paid' })
```

**AWS SQS Standard** provides at-least-once delivery.

---

## Exactly Once

No message is lost and no message is delivered twice. This is the hardest guarantee to provide — it requires coordination between producer, broker, and consumer.

```
Strategies:
- Idempotent producer (broker deduplicates identical sends)
- Transactional writes (message acknowledgement and state update in one atomic transaction)
```

**AWS SQS FIFO** + **Kafka transactions** provide exactly-once semantics at significant performance cost.

---

## Idempotency and Deduplication IDs

The practical solution for at-least-once delivery is to make consumers idempotent using a **[[Deduplication ID]]**:

```js
async function handleMessage(message) {
  const alreadyProcessed = await db.exists(message.dedupId)
  if (alreadyProcessed) return        // safe to skip duplicate

  await processMessage(message)
  await db.save(message.dedupId)      // mark as processed
}
```

This gives you at-least-once delivery from the broker, with effectively-once processing in the consumer.

---

## Summary

| Semantic | When to use | Consumer requirement |
|---|---|---|
| At most once | Lossy is acceptable | None |
| At least once | Most production systems | Must be idempotent |
| Exactly once | Payments, financial systems | Expensive coordination |

In practice, **at-least-once + idempotent consumer** is the standard pattern — it's simpler and more available than exactly-once while still being correct.

---

## Related Topics

- [[Deduplication ID]] — the mechanism that makes at-least-once consumers idempotent
- [[Message Group Pattern]] — delivery semantics interact with ordering guarantees
- [[Messaging Patterns]] — the broader queue and pub/sub patterns these semantics apply to
- [[CAP Theorem]] — exactly-once semantics require strong consistency, limiting availability
