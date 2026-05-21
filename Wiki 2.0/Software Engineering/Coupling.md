# Coupling

[Martin Fowler — Coupling](https://martinfowler.com/bliki/CouplingAndCohesion.html) | [Sam Newman — Building Microservices](https://samnewman.io/books/building_microservices_2nd_edition/)

Coupling describes how much one component depends on another. The goal in distributed systems and microservice architecture is **loose coupling** — components that can change, fail, deploy, and scale independently.

---

## Why It Matters

Tightly coupled services cannot:
- Be deployed independently
- Fail without affecting each other
- Scale separately
- Evolve at different speeds

Reducing coupling is a primary goal of [[Messaging Patterns|event-driven]] and [[Integration Architecture|integration]] patterns.

---

## Common Dimensions

### Temporal Coupling
Both sides must be **available at the same time**. Caused by synchronous communication (REST, RPC). If the downstream service is slow or down, the upstream caller is affected.

```
A --[sync REST call]--> B
(if B is down, A fails)
```

**Solution:** Use async messaging — [[Messaging Patterns|queues or pub/sub]].

---

### Location / Spatial Coupling
A must know **where** B lives — its IP, hostname, or URL. If B moves, A breaks.

**Solution:** Service discovery, DNS-based routing, API Gateway.

---

### Data / Format Coupling
Both sides must agree on the **exact structure** of exchanged data — field names, types, nesting, encoding (JSON, Protobuf, etc.). Changing the schema on one side breaks the other.

**Solution:** Versioned APIs, schema registries, tolerant reader pattern.

---

### Behavioral Coupling
A knows **what B does internally** — calling specific internal logic rather than a stable interface. Changes to B's implementation break A.

**Solution:** Expose stable, intent-based interfaces; hide implementation.

---

### Platform / Technology Coupling
Both components must run on the **same technology stack** — same language, runtime, or vendor.

**Solution:** Technology-agnostic protocols (HTTP, gRPC, AMQP).

---

## Often-Forgotten Dimensions

### Semantic Coupling
Both sides share the **meaning** of data, not just its format. Even without a shared schema, if two services interpret `status: "active"` to mean the same business concept, changing that meaning on one side silently breaks the other.

> This is why [[Anti-Corruption Layer|Anti-Corruption Layers]] and Domain-Driven Design's *ubiquitous language* matter — they make semantic contracts explicit.

---

### Change / Logical Coupling
Two components **always change together** in practice, even if technically separate. Measurable empirically from git history — files that appear in the same commits repeatedly are change-coupled.

> The architecture looks decoupled on a diagram, but the team cannot touch one without touching the other.

---

### Deployment Coupling
Having to **release two services together**, even if they run independently. Common in monorepos or shared CI/CD pipelines. Different from temporal coupling — they don't need to be *running* at the same time, but they *move* as a unit.

---

### Cognitive / Organizational Coupling
Developers must mentally coordinate to understand how two components interact. Closely tied to **Conway's Law**: teams that are tightly coordinated produce tightly coupled systems.

---

## Domain Coupling vs. Format Coupling

| | What is shared |
|---|---|
| **Format coupling** | The shape/structure of data (fields, types, encoding) |
| **Domain coupling** | Business knowledge and concepts (what the data *means*) |

Some domain coupling is unavoidable — an Order service must know a User exists. The goal is to minimize *how much* it knows.

```
Too much domain coupling:
  Order service knows User's subscription tier, payment history, address rules

Acceptable:
  Order service knows only a User ID
```

---

## Tight vs. Loose in Practice

```
Tight:
  A --[sync REST]--> B
  A knows B's URL, format, and waits for response

Loose:
  A --[event]--> [SQS/SNS/EventBridge] --> B
  A fires an event, doesn't know or care who handles it
```

AWS services that reduce coupling:
- **SQS** — breaks temporal coupling (async queue)
- **SNS / EventBridge** — breaks location and behavioral coupling (pub/sub, event routing)
- **API Gateway** — breaks location coupling (stable endpoint in front of any backend)

---

## Summary

| Dimension | What's coupled | Common fix |
|---|---|---|
| Temporal | Availability in time | Async messaging |
| Location | Address / URL | Service discovery, API Gateway |
| Format / Data | Data schema | Versioned APIs, schema registry |
| Behavioral | Internal implementation | Stable interfaces |
| Platform | Technology stack | Standard protocols |
| Semantic | Meaning of data | Ubiquitous language, ACL |
| Change | Change velocity | Good module boundaries |
| Deployment | Release cycle | Independent CI/CD per service |
| Cognitive | Team coordination | Conway's Law, team topology |

---

## Related Topics

- [[Messaging Patterns]] — async messaging is the primary tool for reducing temporal and location coupling
- [[Integration Architecture]] — coupling appears across all integration styles (shared DB, RPC, messaging)
- [[Anti-Corruption Layer]] — protects against semantic and domain coupling from upstream systems
- [[Saga Pattern]] — manages domain coupling across distributed transactions
- [[Observability]] — change coupling is detectable via deployment correlation in traces and logs
