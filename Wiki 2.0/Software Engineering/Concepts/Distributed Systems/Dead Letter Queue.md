# Dead Letter Queue

[AWS SQS – Dead Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) | [Azure Service Bus – DLQ](https://learn.microsoft.com/en-us/azure/service-bus-messaging/service-bus-dead-letter-queues)

A Dead Letter Queue (DLQ) is a secondary queue that receives messages a broker could not successfully deliver or that a consumer repeatedly failed to process. It isolates poisoned messages so they do not block or loop through the main queue.

---

## How It Works

```
Main Queue
  ┌──────────────────────────────────────────┐
  │  Message → Consumer fails (3× retries)   │
  │            ↓                             │
  │         DLQ ← message lands here         │
  └──────────────────────────────────────────┘
```

1. Consumer receives a message and fails (exception, timeout, bad data).
2. The broker re-delivers the message up to a configured **maxReceiveCount**.
3. After the limit is exceeded, the broker moves the message to the DLQ automatically.
4. The main queue continues processing other messages — it is not blocked.

---

## Why It Matters

Without a DLQ, a poison message loops endlessly in the queue. Other messages behind it may be delayed, or the broker may drop it silently, causing silent data loss. The DLQ gives you:

- **Visibility** — you can inspect failed messages and understand why they failed
- **Recoverability** — after fixing the consumer bug, you can replay DLQ messages
- **Non-blocking main queue** — bad messages are quarantined, not looping

---

## Common Causes of DLQ Entries

| Cause | Example |
|---|---|
| Malformed message | JSON that fails schema validation |
| Consumer bug | Unhandled exception in processing code |
| Dependency down | Database unreachable during processing |
| Message too large | Exceeds consumer's size limit |
| Timeout | Consumer processing took too long |

---

## Handling DLQ Messages

```
Option 1 — Replay after fix:
  Fix bug → redrive DLQ messages back to main queue → process normally

Option 2 — Manual triage:
  Inspect messages in DLQ → decide per message: redrive, drop, or escalate

Option 3 — Alert:
  Monitor DLQ depth → alarm if > 0 (any failure worth investigating)
```

A DLQ depth greater than zero is always a signal something needs attention.

---

## AWS SQS Example

```json
// Redrive policy: move to DLQ after 3 failures
{
  "deadLetterTargetArn": "arn:aws:sqs:eu-west-1:123456789:my-queue-dlq",
  "maxReceiveCount": 3
}
```

---

## Related Topics

- [[Delivery Semantics]] — at-least-once delivery is what causes retries; DLQ is the escape hatch
- [[Messaging Patterns]] — DLQ is a standard safety mechanism in queue-based systems
- [[Deduplication ID]] — deduplication prevents retry loops; DLQ handles messages that truly cannot be processed
- [[Load Shedding]] — load shedding drops messages proactively; DLQ catches messages that fail reactively
