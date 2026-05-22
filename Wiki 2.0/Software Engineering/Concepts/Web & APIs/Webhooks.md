# Webhooks

[Webhook.site – Testing Webhooks](https://webhook.site/) | [Stripe Docs – Webhooks](https://stripe.com/docs/webhooks)

A webhook is an HTTP callback — a server sends an HTTP POST to a URL you provide whenever a specific event occurs. The receiving server processes the event immediately instead of having to poll for it. Also called a "reverse API" because the data flow is initiated by the sender, not the receiver.

---

## Push vs Poll

```
Polling (traditional):
  Your App → GET /api/events → Server
  Your App → GET /api/events → Server  (every 30 seconds, wasteful)
  Your App → GET /api/events → Server

Webhook (event-driven):
  Server → POST /your-webhook-endpoint → Your App  (only when event happens)
```

Webhooks are far more efficient for low-frequency events where polling would waste requests.

---

## How It Works

1. You register a URL with the sending service (e.g. Stripe, GitHub, Slack).
2. When an event occurs (payment completed, PR merged, message sent), the service sends an HTTP POST to your URL.
3. Your endpoint receives the payload and processes it.
4. You respond with `200 OK` to acknowledge receipt.

```
GitHub → POST https://your-app.com/webhooks/github
         Body: { "event": "push", "repository": "...", "commits": [...] }
Your App responds: 200 OK
```

---

## Payload Structure (Example — GitHub)

```json
{
  "event": "push",
  "ref": "refs/heads/main",
  "repository": { "name": "my-repo", "full_name": "user/my-repo" },
  "commits": [
    { "id": "abc123", "message": "Fix login bug", "author": { "name": "Jonas" } }
  ]
}
```

---

## Security: Signature Verification

Webhook endpoints are public URLs — anyone can POST to them. Senders sign the payload with a shared secret so you can verify the request is genuine.

```js
// Example: verifying a GitHub webhook signature
import crypto from 'node:crypto'

function verifySignature(payload, secret, signature) {
  const expected = 'sha256=' + crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex')
  return crypto.timingSafeEqual(
    Buffer.from(signature),
    Buffer.from(expected)
  )
}
```

Always verify signatures. Never process a webhook that fails verification.

---

## Reliability Considerations

| Problem | Solution |
|---|---|
| Your endpoint is down | Sender retries (usually exponential backoff) |
| Duplicate deliveries on retry | Make your handler idempotent (check event ID) |
| Out-of-order delivery | Include timestamps or sequence numbers; design for it |
| Processing takes too long | Respond 200 immediately, queue for async processing |

Responding `200 OK` quickly (within 5–30 seconds) is critical — senders treat slow responses as failures and retry.

---

## Webhooks vs Polling vs Server-Sent Events

| | Webhooks | Polling | [[Server-Sent Events\|SSE]] |
|---|---|---|---|
| Direction | Server → Client | Client → Server | Server → Client |
| Trigger | Event-based | Time-based | Event-based |
| Latency | Near real-time | Depends on interval | Near real-time |
| Setup | Register URL | None | Client opens connection |
| Works behind NAT? | No (needs public URL) | Yes | Yes |

---

## Related Topics

- [[Delivery Semantics]] — webhook senders use at-least-once delivery with retries
- [[Dead Letter Queue]] — failed webhook deliveries need equivalent handling
- [[REST]] — webhooks are HTTP POST calls, essentially REST in reverse
- [[Event-Driven Architecture]] — webhooks are a common event delivery mechanism between services
