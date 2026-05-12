# Svelte Cheat Sheet

[Svelte Docs](https://svelte.dev/docs) | [SvelteKit Docs](https://kit.svelte.dev/docs) | [Svelte Playground](https://svelte.dev/playground)

---

## Runes (Svelte 5)

```svelte
<script lang="ts">
// --- STATE ---
let count = $state(0)                        // reactive value
let obj   = $state({ name: "Jonas" })        // deep reactive object/array
let raw   = $state.raw({ big: "object" })    // only reassignment triggers update
let snap  = $state.snapshot(obj)             // non-reactive snapshot (plain object)

// --- DERIVED ---
let doubled  = $derived(count * 2)           // simple computed value
let total    = $derived.by(() => {           // complex computation
  return items.reduce((s, n) => s + n, 0)
})

// --- EFFECT ---
$effect(() => {                              // runs after every render of deps
  console.log(count)
  return () => { /* cleanup */ }            // runs before re-run and on unmount
})
$effect.pre(() => { /* before DOM update */ })

// --- PROPS ---
let { name, age = 18 } = $props<{           // props with types and default values
  name: string
  age?: number
}>()

// --- BINDABLE PROP ---
let { value = $bindable("") } = $props()    // parent can use bind:value

// --- DEBUG ---
$inspect(count)                              // logs count + stack on every change
$inspect(count, obj).with(console.trace)    // custom handler
</script>
```

---

## Template syntax

```svelte
<!-- Expression -->
{variable}
{a + b}
{condition ? "yes" : "no"}

<!-- Conditional -->
{#if x > 0}
  <p>positive</p>
{:else if x === 0}
  <p>zero</p>
{:else}
  <p>negative</p>
{/if}

<!-- Loop -->
{#each items as item}              <!-- without key -->
  <p>{item.name}</p>
{/each}

{#each items as item (item.id)}    <!-- with key — always use for lists! -->
  <p>{item.name}</p>
{:else}
  <p>No entries.</p>               <!-- when items is empty -->
{/each}

{#each items as item, i (item.id)} <!-- with index i -->
  <p>{i}: {item.name}</p>
{/each}

<!-- Async -->
{#await promise}
  <p>Loading...</p>
{:then value}
  <p>{value}</p>
{:catch error}
  <p>Error: {error.message}</p>
{/await}

{#await promise then value}        <!-- shorthand, no loading state -->
  <p>{value}</p>
{/await}

<!-- Force re-mount -->
{#key expression}
  <Component />                    <!-- recreated whenever expression changes -->
{/key}

<!-- Define snippet (Svelte 5) -->
{#snippet label(text)}
  <strong>{text}</strong>
{/snippet}

<!-- Render snippet -->
{@render label("Hello")}
{@render children()}               <!-- default slot replacement in Svelte 5 -->

<!-- Raw HTML (caution: XSS!) -->
{@html markup}

<!-- Constant in template -->
{@const area = width * height}
<p>{area}</p>

<!-- Debug (dev only, does not break in prod) -->
{@debug variable}
```

---

## Element directives

```svelte
<!-- CLASSES -->
<div class:active={isActive}>          <!-- class "active" when isActive is true -->
<div class:active>                     <!-- shorthand: variable = class name -->
<div class={isActive ? "on" : "off"}> <!-- ternary -->

<!-- STYLES -->
<div style:color="red">
<div style:color={myColor}>
<div style:font-size="{size}px">

<!-- BINDINGS — Inputs -->
<input bind:value={name} />            <!-- string -->
<input type="number" bind:value={n} /> <!-- number (auto-converted) -->
<input type="checkbox" bind:checked={bool} />
<input type="range" bind:value={n} />
<select bind:value={selected}>...</select>

<!-- BINDINGS — Multiple selection -->
<input type="checkbox" bind:group={arr} value="a" />
<input type="radio"    bind:group={str} value="x" />

<!-- BINDINGS — Contenteditable -->
<div contenteditable bind:innerHTML={html} />
<div contenteditable bind:textContent={text} />

<!-- BINDINGS — DOM reference -->
<canvas bind:this={canvasEl} />        <!-- canvasEl: HTMLCanvasElement -->

<!-- BINDINGS — Dimensions (read-only) -->
<div bind:clientWidth={w} bind:clientHeight={h} />
<div bind:offsetWidth={w} bind:offsetHeight={h} />
<div bind:contentRect={rect} />

<!-- EVENTS (Svelte 5: native HTML attributes) -->
<button onclick={handler}>
<button onclick={() => count++}>
<input oninput={(e) => (val = e.currentTarget.value)} />
<form onsubmit={(e) => { e.preventDefault(); submit() }}>
<div onkeydown={handleKey} onkeyup={...} onkeypress={...}>
<div onmouseenter={...} onmouseleave={...} onmouseover={...}>
<div onfocus={...} onblur={...}>
<div onscroll={...} onresize={...}>

<!-- ACTIONS (use:) -->
<div use:tooltip={{ content: "Help" }}>
<!-- An action is a function: (node, params) => { ... return { update, destroy } } -->

<!-- TRANSITION / ANIMATION -->
<div transition:fade>                  <!-- in + out -->
<div in:fly={{ y: 20 }} out:fade>     <!-- separate in/out -->
<div transition:slide={{ duration: 300 }}>
<!-- animate: only for {#each} with key -->
<li animate:flip>

<!-- Available transitions from svelte/transition -->
<!-- fade, fly, slide, scale, draw, crossfade -->
<!-- animate: flip (from svelte/animate) -->
```

---

## Component directives

```svelte
<!-- Bind prop (only if child component uses $bindable) -->
<Input bind:value={myVal} />

<!-- Dynamic component -->
<svelte:component this={CurrentComponent} prop={val} />

<!-- DOM reference to a component -->
<MyComp bind:this={compRef} />
```

---

## Special Svelte elements

```svelte
<!-- Bind window object -->
<svelte:window
  bind:scrollY={y}
  bind:innerWidth={w}
  bind:innerHeight={h}
  bind:outerWidth
  bind:outerHeight
  onkeydown={handleKey}
/>

<!-- Document -->
<svelte:document
  bind:fullscreenElement
  bind:visibilityState
  onfullscreenchange={...}
/>

<!-- Body -->
<svelte:body onmouseenter={...} onmouseleave={...} />

<!-- <head> content (title, meta, link...) -->
<svelte:head>
  <title>My Page</title>
  <meta name="description" content="..." />
</svelte:head>

<!-- Dynamic HTML element -->
<svelte:element this="div">         <!-- this can be "div", "span", "h1"... -->
<svelte:element this={tag}>         <!-- or a variable -->

<!-- Recursive component -->
<svelte:self {...props} />

<!-- Options (at top of file) -->
<svelte:options
  runes={true}                      <!-- enforce Svelte 5 runes (default) -->
  immutable={true}                  <!-- objects are never mutated -->
  accessors={true}                  <!-- props as public getters/setters -->
  namespace="svg"                   <!-- or "mathml" for non-HTML -->
/>
```

---

## Lifecycle (from `svelte`)

```ts
import { onMount, onDestroy, beforeUpdate, afterUpdate, tick, untrack, unstate, mount, unmount, flushSync } from 'svelte'

onMount(() => {
  // after first DOM render (browser only)
  return () => { /* cleanup = onDestroy */ }
})

onDestroy(() => { /* on removal */ })

beforeUpdate(() => { /* before every DOM update */ })
afterUpdate(() => { /* after every DOM update */ })

await tick()         // waits until DOM updates are done

// Svelte 5 extras:
untrack(() => {      // access $state without registering as dependency
  return count
})

unstate(obj)         // removes Svelte proxy → plain object (like $state.snapshot)

mount(Component, { target, props })   // manually mount component into DOM
unmount(component)                    // manually unmount
flushSync(() => { state++ })          // synchronously flush state + DOM
```

---

## Stores (from `svelte/store`)

```ts
import { writable, readable, derived, get, readonly } from 'svelte/store'

// Writable
const count = writable(0)
count.set(5)
count.update(n => n + 1)
const unsubscribe = count.subscribe(val => console.log(val))
unsubscribe()

// Readable (only changeable from inside)
const time = readable(new Date(), (set) => {
  const i = setInterval(() => set(new Date()), 1000)
  return () => clearInterval(i)   // cleanup
})

// Derived
const doubled  = derived(count, $c => $c * 2)
const combined = derived([a, b], ([$a, $b]) => $a + $b)
const async    = derived(source, ($src, set) => {   // async derived
  fetch($src).then(r => r.json()).then(set)
})

// Read once (no subscribe)
const value = get(count)

// Read-only wrapper
const readonlyCount = readonly(count)
```

In `.svelte` files: `$count` subscribes and reads automatically.

---

## SvelteKit — File conventions

| File | Purpose |
|---|---|
| `+page.svelte` | Page content |
| `+page.ts` | Client-side load (also SSR) |
| `+page.server.ts` | Server-only load + form actions |
| `+layout.svelte` | Layout wrapper |
| `+layout.ts` | Layout load (client + server) |
| `+layout.server.ts` | Layout load (server-only) |
| `+error.svelte` | Error page |
| `+server.ts` | API endpoint (GET, POST, ...) |

---

## SvelteKit — Page options

```ts
// in +page.ts or +layout.ts
export const prerender = true        // pre-render statically
export const ssr      = false        // no server rendering (client-only)
export const csr      = true         // client-side rendering (default: true)
export const trailingSlash = 'always' | 'never' | 'ignore'
```

---

## SvelteKit — Load function

```ts
import type { PageLoad } from './$types'
import { error, redirect } from '@sveltejs/kit'

export const load: PageLoad = async ({ fetch, params, url, parent, depends }) => {
  depends('app:keys')                       // for invalidate('app:keys')

  const data = await parent()               // get layout data
  const res = await fetch('/api/keys')

  if (!res.ok) throw error(500, 'Error')
  if (!loggedIn) throw redirect(303, '/login')

  return { keys: await res.json() }
}
```

```svelte
<!-- +page.svelte -->
<script lang="ts">
  import type { PageData } from './$types'
  let { data } = $props<{ data: PageData }>()
</script>
```

---

## SvelteKit — Server routes (+server.ts)

```ts
import { json, text, error } from '@sveltejs/kit'
import type { RequestHandler } from './$types'

export const GET: RequestHandler = async ({ url, locals }) => {
  return json({ hello: 'world' })
}

export const POST: RequestHandler = async ({ request }) => {
  const body = await request.json()
  return json({ ok: true }, { status: 201 })
}
```

---

## SvelteKit — Form actions

```ts
// +page.server.ts
import { fail } from '@sveltejs/kit'
import type { Actions } from './$types'

export const actions: Actions = {
  default: async ({ request }) => {
    const data = await request.formData()
    const name = data.get('name')
    if (!name) return fail(400, { error: 'Name missing' })
    return { success: true }
  },
  delete: async ({ request }) => { /* named action */ }
}
```

```svelte
<!-- +page.svelte -->
<script lang="ts">
  import { enhance } from '$app/forms'
  let { form } = $props()              <!-- form = action return value -->
</script>

<form method="POST" use:enhance>
  <input name="name" />
  <button>Submit</button>
</form>
{#if form?.error}<p>{form.error}</p>{/if}
```

---

## SvelteKit — Imports

```ts
// Navigation & routing
import { goto, invalidate, invalidateAll, preloadData, preloadCode,
         beforeNavigate, afterNavigate, onNavigate } from '$app/navigation'
import { page, navigating, updated } from '$app/stores'

// Environment
import { browser, dev, building, version } from '$app/environment'
import { base, assets } from '$app/paths'

// Forms
import { enhance, applyAction, deserialize } from '$app/forms'

// Env variables
import { PUBLIC_API_URL } from '$env/static/public'    // build-time, visible in browser
import { SECRET_KEY }     from '$env/static/private'   // build-time, server only
import { env }            from '$env/dynamic/public'   // runtime, visible in browser
import { env }            from '$env/dynamic/private'  // runtime, server only

// Shared code
import { myUtil } from '$lib/utils'
import MyComp     from '$lib/components/MyComp.svelte'
```

---

## $page store — reference

```ts
import { page } from '$app/stores'

$page.url           // URL object (pathname, searchParams, ...)
$page.params        // route parameters { keyId: "abc" }
$page.route.id      // route ID e.g. "/keys/[keyId]"
$page.status        // HTTP status (200, 404, ...)
$page.error         // error object on error pages
$page.data          // merged load() data from all layouts + page
$page.form          // last form action result
$page.state         // history state (pushState/replaceState)
```

---

## TypeScript — important types

```ts
import type { Component, Snippet, Action, ActionReturn } from 'svelte'
import type { Writable, Readable, Unsubscriber } from 'svelte/store'
import type { TransitionConfig } from 'svelte/transition'

// SvelteKit ($types from own route)
import type {
  PageLoad, PageServerLoad,
  LayoutLoad, LayoutServerLoad,
  PageData, LayoutData,
  Actions, RequestHandler
} from './$types'
```

---

## Transitions (svelte/transition)

```svelte
<script>
  import { fade, fly, slide, scale, draw, crossfade } from 'svelte/transition'
  import { flip } from 'svelte/animate'
  import { quintOut } from 'svelte/easing'
</script>

<div transition:fade={{ duration: 300 }}>
<div transition:fly={{ x: 0, y: -20, duration: 400, easing: quintOut }}>
<div transition:slide={{ axis: 'y' }}>
<div transition:scale={{ start: 0.5 }}>

<!-- crossfade for shared elements between two lists -->
<script>
  const [send, receive] = crossfade({ duration: 400 })
</script>
<div in:receive={{ key: id }} out:send={{ key: id }}>

<!-- flip only in {#each} with key -->
{#each items as item (item.id)}
  <li animate:flip={{ duration: 300 }}>{item.name}</li>
{/each}
```

---

## Svelte 4 → 5 Migration

| Svelte 4 | Svelte 5 |
|---|---|
| `let x = 0` (reactive) | `let x = $state(0)` |
| `$: y = x * 2` | `let y = $derived(x * 2)` |
| `$: { sideEffect() }` | `$effect(() => { sideEffect() })` |
| `export let name` | `let { name } = $props()` |
| `<button on:click={fn}>` | `<button onclick={fn}>` |
| `<slot />` | `{@render children()}` |
| `<slot name="x" />` | `{@render x()}` — as `$props()` snippet |
| `createEventDispatcher()` | Callback as prop: `let { onClose } = $props()` |
| `$$props` / `$$restProps` | `let { a, ...rest } = $props()` |
