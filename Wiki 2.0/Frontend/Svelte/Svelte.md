# Svelte & SvelteKit — Training Plan

[Svelte Docs](https://svelte.dev/docs) | [SvelteKit Docs](https://kit.svelte.dev/docs) | [Svelte Tutorial](https://learn.svelte.dev)

Svelte is a **compiler-based** frontend framework. Unlike React or Vue, Svelte does not run as a runtime in the browser — the compiler translates Svelte code at build time into plain, optimized JavaScript. The result is faster and leaner than traditional frameworks.

**SvelteKit** is the meta-framework on top of Svelte (comparable to Next.js for React). It provides routing, data loading, and deployment adapters.

> **Relevance for SAKE:** The SAKE frontend is built with SvelteKit 2 + Svelte 5 + TypeScript + Tailwind CSS and hosted as a static app on Cloud Run.

---

## Learning Path

### Phase 1 — Svelte Basics (Week 1–2)
1. [[Svelte Components]] — `.svelte` files, structure, script/markup/style
2. [[Svelte Reactivity (Runes)]] — `$state`, `$derived`, `$effect`
3. [[Svelte Template Logic]] — `{#if}`, `{#each}`, `{#await}`
4. [[Svelte Props and Events]] — `$props`, event handlers

### Phase 2 — Data & State (Week 3)
5. [[Svelte Stores]] — `writable`, `readable`, `derived`
6. [[Svelte Lifecycle]] — `onMount`, `onDestroy`
7. [[Fetch in Svelte]] — HTTP requests, `async/await` in the browser

### Phase 3 — SvelteKit (Week 4)
8. [[SvelteKit]] — overview, project structure
9. [[SvelteKit Routing]] — file-based routing, layouts, `+page.svelte`
10. [[SvelteKit Load Functions]] — loading data before rendering
11. [[SvelteKit Static Adapter]] — static deployment (`prerender`, `ssr = false`)

### Phase 4 — Project Stack (Week 5)
12. [[TypeScript in Svelte]] — types in `.svelte` files
13. [[Tailwind CSS]] — utility-first CSS

---

## Why Svelte for SAKE?

| Property | Benefit for SAKE |
|---|---|
| No runtime bundle | Smaller file size, ideal for static hosting |
| Compiler-based | TypeScript & Tailwind integrate directly |
| SvelteKit Static Adapter | Perfect for Cloud Run as a pure file server |
| Svelte 5 Runes | Simpler reactivity model than React Hooks |

---

## Quick Reference: Svelte 5 vs. Svelte 4

| Feature | Svelte 4 | Svelte 5 |
|---|---|---|
| Reactive state | `let count = 0` (implicit) | `let count = $state(0)` |
| Computed values | `$: doubled = count * 2` | `let doubled = $derived(count * 2)` |
| Side effects | `$: { console.log(count) }` | `$effect(() => { console.log(count) })` |
| Props | `export let name` | `let { name } = $props()` |
| Events | `<button on:click={handler}>` | `<button onclick={handler}>` |
| Slots | `<slot />` | `{@render children()}` (Snippets) |

> SAKE uses Svelte 5 — learn the new syntax directly!
