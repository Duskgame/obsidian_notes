# Svelte Learning Plan — SAKE Frontend

**Goal:** Understand Svelte 5 + SvelteKit 2 + TypeScript + Tailwind well enough to read, understand, and extend the SAKE frontend.

**Total duration:** ~5 weeks, ~1–1.5h per day
**Resources:** [learn.svelte.dev](https://learn.svelte.dev) (interactive tutorial), [[Svelte]] (wiki index)

---

## Phase 1 — Svelte Basics
*Goal: Be able to write a Svelte component from scratch.*
*Duration: ~1.5 weeks*

### 1.1 Read theory
- [x] [[Svelte Components]] — `.svelte` file structure, expressions, imports, bindings
- [x] [[Svelte Reactivity (Runes)]] — `$state`, `$derived`, `$effect`
- [x] [[Svelte Template Logic]] — `{#if}`, `{#each}`, `{#await}`
- [x] [[Svelte Props and Events]] — `$props()`, event handlers

### 1.2 Work through interactive tutorial
- [x] [Svelte basics tutorial](https://learn.svelte.dev/tutorial/welcome-to-svelte) (chapters 1–4)
  - Chapter 1: Introduction
  - Chapter 2: Reactivity
  - Chapter 3: Props
  - Chapter 4: Logic

### 1.3 Exercises
- [ ] **Exercise A — Counter:** Create a component with a button that increments a counter. Show the doubled value next to it using `$derived`.
- [ ] **Exercise B — Filter list:** Create a list of 5 fictional service account keys. Add a text field that filters the list by key name. Use `$derived` for the filtered list.
- [ ] **Exercise C — Form:** Create a form with two required fields (JIRA ticket, service account email). Only show the submit button when both fields are filled (`{#if}`).
- [ ] **Exercise D — Split component:** Extract the key entry from Exercise B into its own `KeyRow.svelte` component. Pass the data as props.

### 1.4 Checkpoint
- [ ] I can read a `.svelte` file from the SAKE codebase and understand the structure, `$state`, `{#if}`, `{#each}`, and props.

---

## Phase 2 — Data & State
*Goal: Load data, share it between components, and control side effects.*
*Duration: ~1 week*

### 2.1 Read theory
- [ ] [[Svelte Stores]] — `writable`, `readable`, `derived`, `$` syntax
- [ ] [[Svelte Lifecycle]] — `onMount`, `onDestroy`
- [ ] [[Fetch in Svelte]] — HTTP requests, error handling, AbortController

### 2.2 Interactive tutorial
- [ ] [Tutorial: Lifecycle](https://learn.svelte.dev/tutorial/onmount)
- [ ] [Tutorial: Stores](https://learn.svelte.dev/tutorial/writable-stores)

### 2.3 Exercises
- [ ] **Exercise E — onMount fetch:** Load a list of data from [jsonplaceholder.typicode.com/todos](https://jsonplaceholder.typicode.com/todos) when the component starts and display it. Show a "Loading..." text while loading.
- [ ] **Exercise F — Loading + error:** Extend Exercise E: show an error message when the fetch fails (test with a wrong URL).
- [ ] **Exercise G — Global store:** Create a `stores.ts` with a `currentUser` writable store. Set the user when a "Login" button is clicked. Show the user's name in a separate `Header.svelte` component (without passing it as props).
- [ ] **Exercise H — Derived store:** Create an `isLoggedIn` derived store based on `currentUser`.

### 2.4 Checkpoint
- [ ] I can load data via fetch, initialize it in `onMount`, and manage global state using stores.

---

## Phase 3 — SvelteKit
*Goal: Build a multi-page app with SvelteKit and understand how SAKE's routing works.*
*Duration: ~1 week*

### 3.1 Read theory
- [ ] [[SvelteKit]] — project structure, `$lib`, env variables, build commands
- [ ] [[SvelteKit Routing]] — file-based routing, layouts, `[param]`, `$page` store
- [ ] [[SvelteKit Load Functions]] — `+page.ts`, `$types`, errors and redirects
- [ ] [[SvelteKit Static Adapter]] — `ssr = false`, `prerender`, Nginx, Dockerfile

### 3.2 Create a new SvelteKit project
- [ ] Run `npx sv create sake-learn` in the terminal (select TypeScript + Tailwind)
- [ ] Start `npm run dev` and view the starter app in the browser
- [ ] Compare the project structure with the wiki entry

### 3.3 Interactive tutorial
- [ ] [SvelteKit tutorial](https://learn.svelte.dev/tutorial/kit-introduction) (chapters: Routing, Loading data)

### 3.4 Exercises
- [ ] **Exercise I — Two pages:** Create two pages in your learning project: `/requester` and `/supporter`. Add navigation to the root layout.
- [ ] **Exercise J — Layout:** Create a `+layout.svelte` with a header (app name + active navigation). The active link should be visually highlighted.
- [ ] **Exercise K — Load function:** Create a `+page.ts` on `/requester` that returns simulated key data (a simple array constant). Display the data on the page.
- [ ] **Exercise L — Dynamic route:** Create a route `/keys/[keyId]` that reads the URL parameter and displays it.
- [ ] **Exercise M — Static adapter:** Add `adapter-static`, set `ssr = false` and `prerender = true`, and run `npm run build`. Inspect the `build/` folder.

### 3.5 Checkpoint
- [ ] I understand how SAKE's `src/routes/` structure maps to URLs, what `+layout.svelte` does, and why `adapter-static` + `ssr = false` is used.

---

## Phase 4 — Project stack: TypeScript & Tailwind
*Goal: Read the SAKE code with correct types and style UI elements.*
*Duration: ~0.5 weeks*

### 4.1 Read theory
- [ ] [[TypeScript in Svelte]] — types for props, runes, events, `$types`
- [ ] [[Tailwind CSS]] — utility classes, spacing, flexbox, conditional classes

### 4.2 Exercises
- [ ] **Exercise N — Typing:** Create a `src/lib/types.ts` in your learning project with a `ServiceAccountKey` interface (id, serviceAccount, expires, isExpired). Type the props of `KeyRow.svelte` with it.
- [ ] **Exercise O — Tailwind components:** Build with Tailwind:
  - [ ] A button (blue, hover effect, disabled state)
  - [ ] A card with title and subtext
  - [ ] A form field with label and error text
  - [ ] A status badge (green = active, red = expired)
- [ ] **Exercise P — Responsive:** Make your layout show the navigation as a vertical list on mobile and as a horizontal bar on desktop (`md:` prefix).

### 4.3 Checkpoint
- [ ] I can write TypeScript interfaces for my data and style a page entirely with Tailwind without custom CSS.

---

## Phase 5 — Reading and understanding the SAKE code
*Goal: Navigate the real SAKE code and make first small changes.*
*Duration: ~0.5 weeks*

### 5.1 Explore the SAKE repo
- [ ] Look at the `src/routes/` folder: what pages exist?
- [ ] Look at the `src/lib/` folder: what stores, types, components exist?
- [ ] Look at `svelte.config.js` and compare with [[SvelteKit Static Adapter]]
- [ ] Look at `package.json`: what dependencies are used?

### 5.2 Trace the core flow
- [ ] Find and read the requester flow page in the code
- [ ] Where is `pkijs` / Web Crypto API used? (→ `window.crypto.subtle`)
- [ ] Which store holds the state of the running key request?
- [ ] How is the certificate passed from the browser to the supporter link?

### 5.3 First changes
- [ ] Change a text on a page and test it in the browser (`npm run dev`)
- [ ] Visually adjust a button with Tailwind classes
- [ ] Create a new `/info` page with explanatory text about the SAKE process

### 5.4 Checkpoint
- [ ] I can trace the requester flow in the code from start to finish and explain which Svelte concepts are used where.

---

## Progress

| Phase | Status |
|---|---|
| Phase 1 — Basics | Not started |
| Phase 2 — Data & State | Not started |
| Phase 3 — SvelteKit | Not started |
| Phase 4 — TypeScript & Tailwind | Not started |
| Phase 5 — SAKE code | Not started |

---

## Helpful resources

| Resource | Purpose |
|---|---|
| [learn.svelte.dev](https://learn.svelte.dev) | Interactive tutorial (Svelte + SvelteKit) |
| [svelte.dev/docs](https://svelte.dev/docs) | Official Svelte 5 reference |
| [kit.svelte.dev/docs](https://kit.svelte.dev/docs) | Official SvelteKit 2 reference |
| [tailwindcss.com/docs](https://tailwindcss.com/docs) | Tailwind reference (all utility classes) |
| [typescript-lang.org/docs](https://www.typescriptlang.org/docs/) | TypeScript reference |
| [svelte.dev/playground](https://svelte.dev/playground) | Try Svelte in the browser (no setup) |
