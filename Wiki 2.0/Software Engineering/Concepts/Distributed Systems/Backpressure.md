# Backpressure

[Reactive Streams Specification](https://www.reactive-streams.org/) | [Node.js Streams – Backpressure](https://nodejs.org/en/docs/guides/backpressuring-in-streams)

Backpressure is a flow-control mechanism where a consumer signals to its producer to slow down or pause when it cannot keep up. It prevents the consumer from being overwhelmed without losing any data.

---

## The Problem

A fast producer paired with a slow consumer causes unbounded queue growth:

```
Producer (1000 msg/s) → [■■■■■■■■■■ buffer overflows] → Consumer (100 msg/s)
```

Without flow control, the buffer fills up and either crashes or starts dropping messages.

---

## How Backpressure Works

The consumer sends a signal upstream — "I'm full, slow down":

```
Producer ←── (backpressure signal) ──── Consumer
   ↓                                       ↑
slows down                             catching up
```

The producer reacts by pausing or reducing its send rate until the consumer signals readiness again.

---

## Examples

**Node.js streams:**
```js
const readable = fs.createReadStream('large-file.csv')
const writable = fs.createWriteStream('output.csv')

// .pipe() handles backpressure automatically
// writable pauses readable when its buffer is full
readable.pipe(writable)
```

**Reactive streams (RxJS / Kotlin Flow):**
```kotlin
// Consumer requests exactly N items at a time
flow.collect { item ->
    process(item)   // consumer controls the rate
}
```

**TCP:** The receiver advertises a window size, telling the sender how many bytes it can accept. When the window is 0, the sender stops.

**Message queues:** A consumer that processes slowly simply doesn't ACK — the queue holds the message and doesn't deliver more until the consumer is ready.

---

## Backpressure vs Load Shedding

| | Backpressure | [[Load Shedding]] |
|---|---|---|
| Response to overload | Slow the producer | Reject/drop excess work |
| Data loss | None | Yes — some work is lost |
| Producer control needed | Yes | No |
| Applicable | Internal systems, streams | External traffic, cannot slow producer |

---

## Push vs Pull and Backpressure

**Push model** (producer sends when ready) — backpressure must be added explicitly; harder.

**Pull model** (consumer requests when ready) — backpressure is built in; the consumer never receives more than it requests.

Reactive Streams formalise the pull approach: consumers call `request(n)` to ask for exactly `n` items. The producer never sends unsolicited data.

---

## Related Topics

- [[Load Shedding]] — the alternative when the producer cannot be slowed
- [[Throughput and Latency]] — backpressure reduces throughput to prevent latency from exploding
- [[Messaging Patterns]] — queues inherently provide a form of backpressure via bounded capacity
- [[Event-Driven Architecture]] — async systems must explicitly handle flow control between producers and consumers
