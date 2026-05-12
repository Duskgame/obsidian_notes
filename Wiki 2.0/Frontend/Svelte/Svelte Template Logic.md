# Svelte Template Logic

[Svelte Docs — Logic blocks](https://svelte.dev/docs/svelte/if) | [Tutorial: Logic](https://learn.svelte.dev/tutorial/if-blocks)

Svelte extends HTML with template blocks for conditions, loops, and async data. They start with `{#...}` and end with `{/...}`.

---

## {#if} — Conditionals

```svelte
<script lang="ts">
  let isLoggedIn = $state(false);
</script>

{#if isLoggedIn}
  <p>Welcome, you are logged in.</p>
{:else if role === "supporter"}
  <p>Supporter area</p>
{:else}
  <button onclick={() => isLoggedIn = true}>Login</button>
{/if}
```

- `{#if condition}` — main condition
- `{:else if condition}` — additional condition
- `{:else}` — fallback
- `{/if}` — end of block

---

## {#each} — Loops

```svelte
<script lang="ts">
  let keys = $state([
    { id: "key-1", expires: "2025-06-01" },
    { id: "key-2", expires: "2025-09-01" },
  ]);
</script>

{#each keys as key}
  <p>{key.id} — expires: {key.expires}</p>
{/each}
```

### With index

```svelte
{#each keys as key, index}
  <p>{index + 1}. {key.id}</p>
{/each}
```

### With key (important for lists that change)

```svelte
{#each keys as key (key.id)}
  <p>{key.id}</p>
{/each}
```

The key `(key.id)` helps Svelte correctly track elements when reordering instead of re-rendering everything.

### Fallback for empty list

```svelte
{#each keys as key (key.id)}
  <p>{key.id}</p>
{:else}
  <p>No keys available.</p>
{/each}
```

---

## {#await} — Async data

```svelte
<script lang="ts">
  async function loadKeys(): Promise<string[]> {
    const res = await fetch("/api/keys");
    return res.json();
  }

  let keysPromise = loadKeys();
</script>

{#await keysPromise}
  <p>Loading...</p>
{:then keys}
  {#each keys as key}
    <p>{key}</p>
  {/each}
{:catch error}
  <p>Error: {error.message}</p>
{/await}
```

- `{#await promise}` — shown while the promise is pending
- `{:then value}` — shown when the promise resolves
- `{:catch error}` — shown when the promise rejects

### Shorthand (success state only)

```svelte
{#await keysPromise then keys}
  <p>{keys.length} keys found</p>
{/await}
```

---

## {#key} — Force re-render

```svelte
<script lang="ts">
  let selectedKey = $state("key-1");
</script>

{#key selectedKey}
  <KeyDetail id={selectedKey} />
{/key}
```

When `selectedKey` changes, `KeyDetail` is fully recreated (instead of just updated). Useful for animations or when a component should be completely re-initialized on data change.

---

## Expressions directly in the template

Besides block directives, HTML attributes can also be set conditionally inline:

```svelte
<button
  class={isActive ? "btn-primary" : "btn-secondary"}
  disabled={isLoading}
>
  {isLoading ? "Loading..." : "Submit"}
</button>
```

---

## Related Topics

- [[Svelte Reactivity (Runes)]] — the `$state` variables used inside blocks
- [[Fetch in Svelte]] — fetching data for `{#await}`
- [[Svelte Stores]] — reactive values coming from stores
