# Svelte vs Kotlin — Syntax Comparison

[Svelte 5 Docs](https://svelte.dev/docs) | [Kotlin Docs](https://kotlinlang.org/docs/home.html) | [Jetpack Compose Docs](https://developer.android.com/develop/ui/compose/documentation)

A side-by-side mapping of Svelte 5 concepts to their Kotlin / Jetpack Compose equivalents. Useful when learning Svelte with a Kotlin background.

---

## File structure

A `.svelte` file combines logic, markup, and styles in one file — equivalent to a Composable function with its ViewModel merged in.

```svelte
<script lang="ts">
  // logic (like a mini ViewModel)
</script>

<!-- HTML template (like a @Composable) -->

<style>
  /* scoped CSS — only applies to this component */
</style>
```

Kotlin separates these concerns across a [[ViewModel]], a `@Composable` function, and a `Theme.kt`. Svelte co-locates them by default.

---

## Reactive state — `$state` vs `mutableStateOf`

| Svelte 5 | Kotlin / Compose |
|---|---|
| `let count = $state(0)` | `var count by mutableStateOf(0)` |
| `let user = $state({ name: "Jonas" })` | `var user by mutableStateOf(User("Jonas"))` |

Both trigger a UI re-render when the value changes. `$state` uses deep reactivity (via JavaScript Proxies) for objects and arrays, similar to how Compose tracks `State<T>` reads at the field level.

```svelte
<script lang="ts">
  let count = $state(0);
</script>

<button onclick={() => count++}>Clicked: {count}</button>
```

```kotlin
var count by remember { mutableStateOf(0) }
Button(onClick = { count++ }) { Text("Clicked: $count") }
```

See [[Svelte Reactivity (Runes)]] and [[State in Compose]].

---

## Derived / computed values — `$derived` vs `map {}`

`$derived` recalculates automatically when its dependencies change. The dependencies are tracked at runtime — you don't declare them explicitly.

```svelte
let count = $state(0);
let doubled = $derived(count * 2);
```

```kotlin
// Compose
val doubled by derivedStateOf { count * 2 }

// StateFlow
val doubled = count.map { it * 2 }.stateIn(scope, SharingStarted.Eagerly, 0)
```

For complex multi-step derivations, use `$derived.by(() => { ... })` — equivalent to a longer `derivedStateOf` block or a `combine {}` on [[StateFlow]].

---

## Side effects — `$effect` vs `LaunchedEffect`

```svelte
$effect(() => {
  fetchUser(userId);
  // return a cleanup function (optional):
  return () => cancel();
});
```

```kotlin
LaunchedEffect(userId) {
  fetchUser(userId)
}
```

Both re-run when their dependencies change. `$effect` auto-detects which `$state` variables it reads. `LaunchedEffect` requires an explicit key. Cleanup in `$effect` is a return value; in Compose it goes in `DisposableEffect`.

---

## Props — `$props()` vs function parameters

In [[Svelte Components]], props are received via `$props()` because `.svelte` files are not plain functions.

```svelte
<!-- KeyRow.svelte -->
<script lang="ts">
  let { keyId, expires }: { keyId: string; expires: string } = $props();
</script>
<p>{keyId} — {expires}</p>
```

```kotlin
@Composable
fun KeyRow(keyId: String, expires: String) {
    Text("$keyId — $expires")
}
```

Usage in the parent is nearly identical — named arguments in Kotlin, shorthand attribute syntax in Svelte:

```svelte
<KeyRow {keyId} {expires} />   <!-- shorthand: keyId={keyId} -->
```

```kotlin
KeyRow(keyId = key.id, expires = key.expires)
```

See [[Svelte Props and Events]].

---

## Template logic — `{#if}` / `{#each}` vs `if` / `forEach`

```svelte
{#if isLoggedIn}
  <p>Welcome</p>
{:else}
  <button onclick={login}>Login</button>
{/if}

{#each items as item}
  <ItemRow {item} />
{:else}
  <p>No items.</p>
{/each}
```

```kotlin
if (isLoggedIn) {
    Text("Welcome")
} else {
    Button(onClick = { login() }) { Text("Login") }
}

if (items.isEmpty()) {
    Text("No items.")
} else {
    items.forEach { item -> ItemRow(item) }
    // or: LazyColumn { items(list) { ItemRow(it) } }
}
```

The `{:else}` block on `{#each}` handles empty lists in one construct — Kotlin requires a manual `isEmpty()` check.

See [[Svelte Template Logic]].

---

## Two-way binding — `bind:value` vs manual `onValueChange`

Svelte's `bind:` directive wires both the displayed value and the change callback automatically.

```svelte
let text = $state('');
<input bind:value={text} />
```

```kotlin
var text by remember { mutableStateOf("") }
TextField(value = text, onValueChange = { text = it })
```

See [[Svelte Bind Directive]].

---

## Event handlers — `onclick` vs lambda parameters

```svelte
<button onclick={() => count++}>Click</button>
<input oninput={(e) => filter = e.target.value} />
```

```kotlin
Button(onClick = { count++ }) { Text("Click") }
```

Svelte uses standard HTML event names (lowercase, no `on:` prefix in Svelte 5). The handler is an arrow function. Kotlin uses named lambda parameters.

---

## Global state — Stores vs ViewModel / StateFlow

| Svelte | Kotlin |
|---|---|
| `writable<T>()` in `stores.ts` | `MutableStateFlow<T>` in a [[ViewModel]] |
| `readable<T>()` | `StateFlow<T>` (read-only) |
| `derived(store, fn)` | `StateFlow.map { }` / `combine { }` |
| `$storeName` in template | `.collectAsState()` in Compose |

```typescript
// stores.ts
import { writable } from 'svelte/store'
export const currentUser = writable<User | null>(null)
```

```svelte
<!-- Any component -->
<script>
  import { currentUser } from '$lib/stores'
</script>
<p>Hello {$currentUser?.name}</p>
```

The `$` prefix auto-subscribes and auto-unsubscribes — equivalent to `.collectAsState()` in Compose, which also handles the subscription lifecycle.

See [[Svelte Stores]] and [[Unidirectional data Flow]].

---

## Summary

| Concept | Kotlin / Compose | Svelte 5 |
|---|---|---|
| Local state | `mutableStateOf` / `remember` | `$state` |
| Computed value | `derivedStateOf` / `StateFlow.map` | `$derived` |
| Side effect | `LaunchedEffect` | `$effect` |
| Props | function parameters | `$props()` |
| Conditional UI | `if {}` | `{#if}` |
| List rendering | `forEach` / `LazyColumn` | `{#each}` |
| Two-way binding | manual `onValueChange` | `bind:value` |
| Global state | ViewModel + StateFlow | Svelte Stores |
| Component file | `@Composable` + `ViewModel` (separate) | single `.svelte` file |

---

## Related Topics

- [[Svelte Reactivity (Runes)]] — `$state`, `$derived`, `$effect` in detail
- [[Svelte Components]] — `.svelte` file structure
- [[Svelte Props and Events]] — props and event handling
- [[Svelte Stores]] — global state management
- [[State in Compose]] — the Kotlin/Compose equivalent of `$state`
- [[Jetpack Compose]] — Kotlin UI framework this note compares against
- [[Unidirectional data Flow]] — the shared data-flow pattern behind both frameworks
- [[Model-View-ViewModel]] — architecture pattern used in both ecosystems
