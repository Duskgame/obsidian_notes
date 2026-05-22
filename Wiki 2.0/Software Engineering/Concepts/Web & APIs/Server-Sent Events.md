# Server-Sent Events

[MDN – Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) | [W3C Specification](https://html.spec.whatwg.org/multipage/server-sent-events.html)

Server-Sent Events (SSE) is a browser API for receiving a continuous stream of updates from a server over a single long-lived HTTP connection. The server pushes events to the client as they happen — the client never polls.

---

## How it works

The client opens a connection using the `EventSource` API. The server keeps the connection open and sends newline-delimited text events whenever it has something to say.

```
Client                         Server
  |──── GET /events ──────────►|
  |     (connection stays open) |
  |◄──── data: {"status":"ok"} ─|   (immediately)
  |◄──── data: {"progress":50} ─|   (30 seconds later)
  |◄──── data: {"done":true} ───|   (when complete)
  |      [client closes]        |
```

---

## Browser API

```js
// Client — open a connection
const source = new EventSource('/api/session/a3f9c2/events')

source.onmessage = (event) => {
  const data = JSON.parse(event.data)
  console.log(data)          // { keyId: '...', oauth2ClientId: '...' }
  source.close()             // done — close the connection
}

source.onerror = () => {
  console.error('Connection lost — browser will auto-reconnect')
}
```

The browser reconnects automatically if the connection drops. The `Last-Event-ID` header lets the server resume from where it left off.

---

## Server response format

The server sends plain text in a specific format over `Content-Type: text/event-stream`:

```
HTTP/1.1 200 OK
Content-Type: text/event-stream
Cache-Control: no-cache

data: {"status": "waiting"}\n\n

data: {"keyId": "abc123", "oauth2ClientId": "111222"}\n\n
```

Each event is separated by a blank line (`\n\n`). Optional fields: `id:`, `event:` (named events), `retry:` (reconnect interval).

---

## SSE vs Polling vs WebSockets

| | SSE | Polling | WebSockets |
|---|---|---|---|
| Direction | Server → Client only | Client → Server (repeated) | Bidirectional |
| Protocol | HTTP | HTTP | WS (upgrade from HTTP) |
| Auto-reconnect | Built in | Must implement | Must implement |
| Complexity | Low | Lowest | Higher |
| Good for | Push notifications, live feeds | Rarely preferable | Chat, games, live collaboration |

SSE is the right choice when the server needs to push occasional updates to the client and the client never needs to send data back on the same channel.

---

## SSE vs Polling for SAKE session handoff

The SAKE server-side handoff uses polling (GET every 5 seconds) because Cloud Functions are stateless — they cannot hold an open SSE connection between invocations. SSE would require a persistent process (e.g. Cloud Run with minimum instances > 0).

Polling is simpler and sufficient here: the wait is measured in minutes, not milliseconds, so 5-second intervals are fine.

---

## Related Topics

- [[REST]] — SSE runs over plain HTTP; no special infrastructure needed
- [[Webhooks]] — webhooks push from server to server; SSE pushes from server to browser
- [[Messaging Patterns]] — SSE is a lightweight form of the push pattern
- [[Serverless]] — stateless serverless functions (Cloud Functions, Lambda) cannot hold SSE connections open
