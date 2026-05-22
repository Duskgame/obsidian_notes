# RPC (Remote Procedure Call)

[gRPC Documentation](https://grpc.io/docs/) | [tRPC Documentation](https://trpc.io/docs) | [Wikipedia – RPC](https://en.wikipedia.org/wiki/Remote_procedure_call)

RPC (Remote Procedure Call) is a communication pattern where a client calls a function on a remote server as if it were a local function call — hiding the network request behind a normal function interface.

---

## The Idea

```js
// Feels like a local function call
const user = await api.getUser(123)

// Under the hood, it sends a network request to another server
// serialises arguments, deserialises the response — all hidden
```

The goal is to make distributed communication feel like in-process communication.

---

## RPC vs REST

| | RPC | REST |
|---|---|---|
| Mental model | Call a function | Access a resource |
| URL style | `/getUser`, `/createOrder` | `/users/123`, `/orders` |
| Protocol | Usually HTTP/2 or custom | HTTP/1.1 or HTTP/2 |
| Typing | Often strongly typed | Usually loosely typed JSON |
| Streaming | Supported (gRPC) | Limited |

REST models *things* (resources); RPC models *actions* (procedures). REST is technically not RPC, but is often used for the same purposes.

---

## Common Implementations

**gRPC** — Google's RPC framework
- Uses Protocol Buffers (binary serialisation — compact and fast)
- HTTP/2 — supports bidirectional streaming
- Strongly typed via `.proto` schema files
- Popular for internal microservice communication
- Generates client/server code in multiple languages

**tRPC** — TypeScript-only, end-to-end type safety
- No schema files — TypeScript types flow directly from server to client
- Zero-boilerplate API for TypeScript/SvelteKit/Next.js projects
- Not for cross-language use

**JSON-RPC** — simple RPC over plain JSON
- `{ "method": "getUser", "params": [123], "id": 1 }`
- Used by Ethereum and some legacy systems

**GraphQL** — not pure RPC but closer to RPC than REST
- Client specifies exactly what fields it wants
- One endpoint, flexible queries

---

## tRPC in SvelteKit

tRPC is the most relevant for SvelteKit projects — it allows calling server functions from the client with full TypeScript autocomplete and type checking:

```ts
// server — define procedure
export const appRouter = router({
  getUser: publicProcedure
    .input(z.object({ id: z.number() }))
    .query(({ input }) => db.users.findById(input.id))
})

// client — call it like a function
const user = await trpc.getUser.query({ id: 123 })
// 'user' is fully typed — no manual type annotations needed
```

Type errors caught at compile time if the server changes its return shape.

---

## Related Topics

- [[REST]] — the resource-oriented alternative to RPC for HTTP APIs
- [[API]] — RPC is one of several API communication styles
- [[SvelteKit]] — tRPC integrates well with SvelteKit for full-stack type safety
- [[Integration Architecture]] — RPC is one of the four core integration styles
