# Event Broker

[AWS — EventBridge](https://aws.amazon.com/eventbridge/) | [Enterprise Integration Patterns — Message Broker](https://www.enterpriseintegrationpatterns.com/patterns/messaging/MessageBroker.html)

An event broker is the infrastructure component that **receives, stores, and routes events** between producers and consumers in an [[Event-Driven Architecture]]. It is the "post office" of event-based communication — producers send to it, consumers receive from it, and neither needs to know about the other.

---

## What It Does

- **Receives** events from producers
- **Stores** them durably so nothing is lost if a consumer is temporarily down
- **Routes** events to the correct consumers (by topic, rule, or subscription)
- **Decouples** producers from consumers completely — neither knows the other's address

---

## Without vs. With a Broker

```
Without broker:
  Service A --> Service B
  Service A --> Service C    (A must know all consumers, manage all connections)
  Service A --> Service D

With broker:
  Service A --> [Broker] --> Service B
                         --> Service C    (A knows only the broker)
                         --> Service D
```

The broker makes it possible to add new consumers without modifying the producer.

---

## Broker vs. Queue

These terms are often confused:

| | Queue (e.g. SQS) | Broker (e.g. SNS, EventBridge) |
|---|---|---|
| Consumers | One consumer per message | Many consumers per event |
| Direction | Point-to-point | Pub/sub or fan-out |
| Routing | None | Can filter and route by rules |
| Typical use | Task offloading, background jobs | Event notification, fan-out |

In practice on AWS, brokers and queues are combined: the broker ([[SNS]] or EventBridge) fans out to multiple [[SQS]] queues, giving each consumer durable, independent buffering. See [[Messaging Patterns#Fan-Out Pattern]].

---

## AWS Event Brokers

| Service | Best for |
|---|---|
| **[[SNS]]** | Simple pub/sub, fan-out to multiple targets |
| **EventBridge** | Complex routing, filtering by event content and rules, schema registry |
| **Kinesis** | High-volume ordered event streaming, data pipelines |
| **MSK (Kafka)** | Managed Kafka, event streaming at very large scale |

> **[[SQS]]** is technically a queue, not a broker — but it is always used alongside brokers to buffer events per consumer.

---

## EventBridge (AWS's Primary Event Broker)

EventBridge is AWS's fully managed event bus. It adds:
- **Rules:** route events only to the consumers that match a filter (e.g. only `OrderPlaced` events where `total > 100`)
- **Schema registry:** documents and validates event shapes
- **Cross-account routing:** events can flow between AWS accounts

```
Producer --> EventBridge Bus
                |
    ────────────────────────────
    ↓                           ↓
Rule: type = "OrderPlaced"   Rule: type = "PaymentFailed"
    ↓                           ↓
Lambda (send email)          Lambda (alert team)
```

---

## Related Topics

- [[Event-Driven Architecture]] — the architectural style that event brokers enable
- [[Messaging Patterns]] — pub/sub, fan-out, and topic-queue chaining all rely on a broker
- [[SNS]] — AWS managed pub/sub broker
- [[SQS]] — AWS managed queue, used alongside brokers for durable per-consumer buffering
- [[Coupling]] — brokers reduce location, temporal, and behavioral coupling between services
