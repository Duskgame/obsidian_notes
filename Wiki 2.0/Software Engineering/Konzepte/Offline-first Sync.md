**Offline-first** is an architecture pattern where the app stores all data locally and works fully without a network connection. A server is used for sharing and backup, not as the primary data source.

## How it works

```
Local DB  ←──── always used for reading/writing
    ↑
    │  sync on demand (upload / download)
    ↓
Remote Server
```

The app reads and writes to the local database exclusively. Sync with the server happens explicitly (e.g. user taps "Upload" or "Browse") rather than automatically on every action.

## Why offline-first?

- Works without internet — no blank screens or spinners for basic usage
- Faster — local reads/writes are instant, no network latency
- Simpler — you don't have to manage what happens when a request fails mid-operation

## Sync operations

**Upload** — user explicitly sends local data to the server so others can see it:
```
local quiz → convert to remote model → POST /quizzes/upload
```

**Browse/Download** — user explicitly fetches quizzes from the server and saves them locally:
```
GET /quizzes/browse → convert to local entities → insert into Room DB
```

## Ownership and forking

A downloaded quiz is a local copy — the user can edit it without affecting the original on the server. If they upload it again, it becomes their own version. This avoids the need for a permission system in early stages.

## Contrast: online-first

In an online-first architecture (like most web apps), the server is the source of truth and the client fetches data on demand. This requires internet connectivity for most actions. Offline-first is better for mobile apps where connectivity is unreliable.

## Related

- [[Database Repository]]
- [[Ktor Client]]
- [[REST]]
