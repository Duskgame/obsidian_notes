# Firestore

[Firestore Documentation](https://firebase.google.com/docs/firestore) | [Firestore on GCP](https://cloud.google.com/firestore/docs)

Firestore is Google's serverless, fully managed NoSQL document database. It stores data as documents grouped into collections, scales automatically, and requires zero infrastructure setup. It is available both via Firebase and directly through Google Cloud.

> **Relevance:** Recommended as the session storage backend for the SAKE server-side handoff Cloud Function — session documents can be given a TTL field so Firestore deletes them automatically after 2 hours.

---

## Data model

Firestore organises data in a hierarchy: **collections** contain **documents**, and documents contain fields.

```
sessions/               ← collection
  a3f9c2/              ← document (keyed by session token)
    cert: "..."
    samail: "my-service@my-project.iam.gserviceaccount.com"
    jiraTicket: "TICKET-1234"
    expiry: 90
    createdAt: 2026-05-22T14:00:00Z
    result: null        ← filled in when supporter completes
```

Documents can contain nested objects, arrays, and typed primitives (strings, numbers, timestamps, booleans).

---

## Key properties

**Serverless** — no VMs, no clusters, no connection pools to manage. You call the API; Google handles the rest.

**Real-time listeners** — clients can subscribe to a document or collection and receive updates the instant data changes, without polling.

**TTL (Time to Live)** — a field can be designated as a TTL field. Firestore automatically deletes documents once the timestamp in that field passes. This is the cleanest way to expire short-lived sessions.

**Strongly consistent reads** — unlike some NoSQL databases, Firestore guarantees strongly consistent reads by default on single-document lookups.

---

## TTL example (session expiry)

```js
// Create a session that expires in 2 hours
import { Firestore, FieldValue } from '@google-cloud/firestore'
const db = new Firestore()

await db.collection('sessions').doc('a3f9c2').set({
  cert: certPem,
  samail: 'my-service@my-project.iam.gserviceaccount.com',
  jiraTicket: 'TICKET-1234',
  result: null,
  expireAt: new Date(Date.now() + 2 * 60 * 60 * 1000),  // 2h from now
})
// Firestore deletes this document automatically once expireAt passes
// (TTL field must be configured in the Firestore console first)
```

---

## Firestore vs alternatives for session storage

| Option | Pros | Cons |
|---|---|---|
| **Firestore** | Native GCP, TTL built-in, no infra | Slight latency vs in-memory |
| **Cloud Memorystore (Redis)** | Fastest, TTL built-in | Needs a VPC, more setup |
| **In-memory (CF instance)** | Zero infra, instant | Lost on instance restart (CFs are stateless) |

For short-lived sessions in a Cloud Function context, Firestore is the practical default.

---

## Accessing Firestore from a Cloud Function

```js
// Cloud Function (Node.js) reading a session
import { Firestore } from '@google-cloud/firestore'
const db = new Firestore()

export async function getSession(req, res) {
  const token = req.params.token
  const doc = await db.collection('sessions').doc(token).get()

  if (!doc.exists) return res.status(404).json({ error: 'Session not found or expired' })
  return res.json(doc.data())
}
```

Cloud Functions running in the same GCP project as the Firestore database authenticate automatically via the default service account — no credentials to manage.

---

## Related Topics

- [[GoogleCloud]] — Firestore is a core GCP managed service
- [[Serverless]] — Firestore fits the serverless model; no infra to provision
- [[REST]] — Firestore is accessed via REST API or client SDKs
- [[Dead Letter Queue]] — Firestore TTL serves a similar cleanup role to DLQ expiry policies
