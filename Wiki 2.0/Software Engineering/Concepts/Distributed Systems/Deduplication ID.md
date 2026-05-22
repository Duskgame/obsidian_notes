# Deduplication ID

[AWS SQS FIFO – Deduplication](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html) | [Stripe – Idempotency Keys](https://stripe.com/docs/api/idempotent_requests)

A deduplication ID is a unique identifier attached to a message or request so the system can detect and discard duplicate deliveries. It is the practical mechanism that makes an at-least-once consumer behave as effectively-once.

---

## The Problem It Solves

With [[Delivery Semantics#At Least Once|at-least-once delivery]], the same message can arrive more than once (retry after failed ACK, network hiccup, etc.). Without deduplication, every delivery triggers processing:

```
Message "charge $50" delivered twice → customer charged $100
```

A deduplication ID prevents the second delivery from causing a second effect.

---

## How It Works

```
Message arrives: { dedupId: "abc-123", data: "charge $50" }
→ Check: has "abc-123" been processed before?
→ No  → process, store "abc-123" as processed
→ Yes → skip (duplicate)

Same message arrives again: { dedupId: "abc-123", data: "charge $50" }
→ Check: has "abc-123" been processed before?
→ Yes → skip ✓
```

```js
async function handleMessage(message) {
  const seen = await db.exists(message.dedupId)
  if (seen) return

  await processMessage(message)
  await db.save(message.dedupId)
}
```

---

## Where Deduplication IDs Are Used

**AWS SQS FIFO queues** — `MessageDeduplicationId` attribute. SQS itself deduplicates within a 5-minute window.

**Stripe** — Idempotency keys on API requests. Stripe returns the cached response for the same key without re-running the charge.

**Payment systems** — critical to avoid charging twice. Deduplication key is usually an order ID or transaction reference.

**Webhooks** — event delivery services (Stripe, GitHub) may send the same event more than once. The event `id` field serves as the deduplication key.

---

## Storing Processed IDs

Options for tracking which IDs have been seen:

| Storage | Trade-off |
|---|---|
| Database (unique constraint) | Durable, survives restart — but adds DB write per message |
| Redis / cache with TTL | Fast — but IDs expire, risks false negatives over long windows |
| Kafka consumer offset | Built-in for Kafka consumers — no extra storage needed |

---

## Idempotency Keys vs Deduplication IDs

These terms are used interchangeably but have a subtle distinction:

- **Deduplication ID** — prevents a message from being *processed* more than once
- **Idempotency key** — makes an *operation* safe to retry (producing the same result regardless of how many times it runs)

Both achieve the same goal: exactly-once effect from at-least-once delivery.

---

## Related Topics

- [[Delivery Semantics]] — deduplication IDs are the mechanism for at-least-once + idempotent consumers
- [[Messaging Patterns]] — DLQ and retry logic create duplicate deliveries that require deduplication
- [[Message Group Pattern]] — FIFO queues use both group IDs and deduplication IDs together
