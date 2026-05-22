# Next.js

[Next.js Documentation](https://nextjs.org/docs) | [Next.js vs SvelteKit Comparison](https://kit.svelte.dev/docs/introduction)

Next.js is a React framework that adds routing, server-side rendering, API routes, and static generation on top of React. It is to React what SvelteKit is to Svelte — the full-stack layer React doesn't provide on its own.

---

## React vs Next.js

React is a UI library — it handles components and reactivity but gives you nothing else. Next.js wraps React and provides the missing pieces:

| Feature | React alone | Next.js |
|---|---|---|
| Routing | ✗ (need React Router) | ✓ File-based |
| SSR / SSG | ✗ | ✓ Built-in |
| API routes | ✗ | ✓ `/app/api/` |
| Data fetching conventions | ✗ | ✓ `getServerSideProps`, `fetch` in Server Components |
| Image optimisation | ✗ | ✓ `<Image>` component |

---

## Comparison with SvelteKit

| | SvelteKit | Next.js |
|---|---|---|
| UI layer | Svelte | React |
| Routing | File-based (`src/routes/`) | File-based (`app/` or `pages/`) |
| SSR | ✓ | ✓ |
| Static export | ✓ (`adapter-static`) | ✓ (`output: 'export'`) |
| Bundle size | Smaller (no virtual DOM) | Larger |
| Learning curve | Moderate | Moderate |
| Ecosystem | Smaller but growing | Very large |

If you know SvelteKit, Next.js feels familiar — same file-based routing concept, same SSR/SSG patterns, just React syntax instead of Svelte.

---

## Basic Routing (App Router)

```
app/
├── page.tsx           → /
├── about/
│   └── page.tsx       → /about
└── users/
    └── [id]/
        └── page.tsx   → /users/123
```

```tsx
// app/users/[id]/page.tsx
export default function UserPage({ params }: { params: { id: string } }) {
  return <h1>User {params.id}</h1>
}
```

---

## Other React Frameworks

| Framework | Focus |
|---|---|
| **Next.js** | Full-stack, SSR/SSG, most popular |
| **Remix** | Web standards, forms, progressive enhancement |
| **Gatsby** | Static site generation, content sites |

Next.js is by far the most widely used — "I use React" in a professional context almost always means Next.js.

---

## Related Topics

- [[SvelteKit]] — the SvelteKit equivalent; same problems, different UI library
- [[SvelteKit Routing]] — file-based routing in SvelteKit for comparison
- [[REST]] — Next.js API routes are a common way to build REST endpoints
