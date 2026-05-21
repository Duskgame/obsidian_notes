# Upstream and Downstream

[Martin Fowler — Upstream/Downstream](https://martinfowler.com/bliki/UpstreamDownstream.html) | [DDD — Context Mapping](https://www.domainlanguage.com/ddd/reference/)

Upstream and downstream describe the **direction of data flow** between systems. A system is upstream if it sends data to you; downstream if it receives data from you. The terms define dependency and failure direction.

---

## Direction of Flow

```
Upstream System --> [Your System] --> Downstream System
```

- **Upstream** — sends data *to* you; you depend on it
- **Downstream** — receives data *from* you; it depends on you

---

## What Changes With Direction

| | Upstream | Downstream |
|---|---|---|
| Data flows | Toward you | Away from you |
| If it breaks | **You** break | **They** break |
| Who depends on who | You depend on them | They depend on you |
| Contract ownership | They define the interface | You define the interface |
| You can control it | Rarely | Yes |

---

## Practical Example

```
AWS RDS (database)
        ↓
  Order Service       ← you are here
        ↓
  ─────────────────
  ↓               ↓
Email Service   Analytics Service
```

- **RDS** = upstream (Order Service reads from it)
- **Email Service, Analytics** = downstream (they consume your data)

---

## Why It Matters

**Upstream failures cascade to you.** If a payment provider (upstream) goes down, your checkout flow fails. This is why patterns like [[Circuit Breaker]] exist — to handle upstream failures gracefully without bringing your service down.

**Downstream failures don't stop you** — unless you use synchronous calls. Switching to async messaging ([[Messaging Patterns]]) means a downstream service going down just causes messages to queue up, not failures to propagate upstream.

**Contract ownership:** You cannot unilaterally change an upstream system's interface. If it changes without notice, you must adapt. This is why the [[Anti-Corruption Layer]] pattern exists — to insulate your system from upstream changes.

---

## In Domain-Driven Design

DDD uses upstream/downstream explicitly in **Context Mapping** to describe relationships between Bounded Contexts:

- The upstream context defines the model; the downstream context must conform or translate
- A **Conformist** downstream team adopts the upstream model as-is
- An **Anti-Corruption Layer** lets the downstream team translate without polluting their own model

---

## Related Topics

- [[Circuit Breaker]] — protects your system from upstream failures
- [[Anti-Corruption Layer]] — translates upstream models to protect your downstream domain
- [[Coupling]] — upstream dependency is a form of domain and temporal coupling
- [[Messaging Patterns]] — async messaging decouples upstream from downstream in time
- [[Integration Architecture]] — integration styles define how upstream and downstream connect
