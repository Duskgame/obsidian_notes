# AWS SQS

[AWS SQS Documentation](https://docs.aws.amazon.com/sqs/) | [SQS Best Practices](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-best-practices.html)

Simple Queue Service (SQS) is AWS's managed message queue service. It decouples producers and consumers by buffering messages, enabling asynchronous processing. See [[Messaging Patterns]] for the general message queue concept.

---

## How It Works

1. **Producer** sends a message to the queue
2. Message is stored durably until consumed (up to 14 days)
3. **Consumer** polls the queue and receives the message
4. Consumer processes the message
5. Consumer explicitly deletes the message — only then is it removed

If the consumer crashes before deleting, the message reappears after the **visibility timeout** and is retried.

---

## Queue Types

### Standard Queue
- **At-least-once delivery** — a message may be delivered more than once
- **Best-effort ordering** — messages may arrive out of order
- **Near-unlimited throughput**
- Use when: speed matters more than strict ordering; processing must be idempotent

### FIFO Queue
- **Exactly-once processing** — deduplication prevents duplicate processing
- **Strict first-in, first-out ordering** within a message group
- **Up to 3,000 messages/second** (with batching)
- Use when: order matters (financial transactions, sequential steps)

---

## Key Configuration

| Parameter | Description | Default |
|---|---|---|
| **Visibility Timeout** | How long a received message is hidden from other consumers | 30 seconds |
| **Message Retention** | How long unprocessed messages stay in the queue | 4 days (max 14) |
| **Receive Wait Time** | Long polling — waits up to N seconds for messages (reduces empty responses) | 0 (short poll) |
| **Max Receive Count** | How many times a message is retried before going to DLQ | — |

---

## Dead Letter Queue (DLQ)

When a message fails `maxReceiveCount` times, SQS moves it to a configured **Dead Letter Queue** rather than discarding it.

```
Normal Queue → message fails 3 times → DLQ
```

The DLQ allows:
- Inspecting failed messages without losing them
- Alerting when messages land in the DLQ (via [[AWS SNS|SNS]] or CloudWatch)
- Manual reprocessing after fixing the bug

---

## SQS + Lambda Integration

Lambda can be configured to **poll SQS automatically** — no need to write polling logic:

```
SQS Queue → Lambda Event Source Mapping → Lambda Function
```

Lambda scales the number of concurrent invocations based on queue depth. On success, Lambda deletes the messages. On failure, messages return to the queue and are retried.

---

## SQS in the Fan-Out Pattern

SQS is paired with [[AWS SNS|SNS]] in the fan-out pattern:

```
SNS Topic → SQS Queue A → Lambda A
          → SQS Queue B → Lambda B
          → SQS Queue C → Lambda C
```

Each consumer gets its own isolated queue — independent retry, scaling, and DLQ configuration per consumer.

---

## SQS vs SNS

| SQS | SNS |
|---|---|
| Queue — one consumer processes each message | Topic — all subscribers receive each message |
| Pull-based (consumer polls) | Push-based (SNS pushes to subscribers) |
| Messages persist until consumed | No persistence — if subscriber is down, message is lost (unless SQS is the subscriber) |
| Background job processing | Event broadcasting |

---

## Related Topics

- [[Messaging Patterns]] — SQS implements the point-to-point message queue pattern
- [[AWS SNS]] — combine SNS + SQS for the fan-out pattern
- [[AWS Lambda]] — Lambda is the standard SQS consumer in serverless architectures
- [[AWS IAM]] — queue access is controlled by IAM policies and SQS resource policies
- [[Saga Pattern]] — SQS queues buffer messages between saga steps in choreography
