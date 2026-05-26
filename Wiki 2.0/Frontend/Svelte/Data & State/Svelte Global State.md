# Svelte Global State

[Svelte Docs — Shared State](https://svelte.dev/docs/svelte/$state#Universal-reactivity)

`$state` inside a component is local — other components cannot access it. To share state across components, move it into a separate `.svelte.js` file.

---

## Why `.svelte.js` and not `.js`

Runes (`$state`, `$derived`) are compiler features. The Svelte compiler only processes files ending in `.svelte` or `.svelte.js` / `.svelte.ts`. A plain `.js` file will throw an error if it uses runes.

| File extension | Runes work? |
|---|---|
| `store.js` | no |
| `store.svelte.js` | yes |
| `Component.svelte` | yes |

---

## Basic example

```js
// state/counter.svelte.js
export let count = $state(0);

export function increment() {
    count++;
}
```

```svelte
<!-- Counter.svelte -->
<script>
  import { count, increment } from '../state/counter.svelte.js';
</script>

<button onclick={increment}>{count}</button>
```

```svelte
<!-- Display.svelte -->
<script>
  import { count } from '../state/counter.svelte.js';
</script>

<p>Count is: {count}</p>
```

Both components share the same `count`. When one updates it, the other reacts automatically.

---

## With $derived

```js
// state/user.svelte.js
let name = $state("Jonas");
let loggedIn = $state(false);

const greeting = $derived(`Hello, ${name}`);

function login(username) {
    name = username;
    loggedIn = true;
}

function logout() {
    loggedIn = false;
}

export { name, loggedIn, greeting, login, logout };
```

---

## Mental model

```
user.svelte.js      ← shared reactive box outside all components
      ↑        ↑
  Header     Profile   ← both read/write the same box, both react to changes
```

Any change to exported state immediately updates every component that imported it.

---

## Compared to Svelte Stores (Svelte 4)

In Svelte 4, cross-component state required `writable()` stores with `$store` syntax. In Svelte 5, `.svelte.js` files with runes are the idiomatic replacement — simpler and consistent with component syntax.

---

## Related

- [[Svelte Reactivity (Runes)]] — `$state`, `$derived` basics
- [[Svelte Stores]] — Svelte 4 equivalent
- [[Svelte Components]] — where local state lives
- [[Svelte State Factory Pattern]] — page-local logic extraction into `.svelte.ts` using the same runes
