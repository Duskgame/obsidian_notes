# Svelte State Factory Pattern

[Svelte Docs — Runes in .svelte.ts files](https://svelte.dev/docs/svelte/what-are-runes#runes-in-ts-files) | [Svelte Docs — $state](https://svelte.dev/docs/svelte/$state)

A design pattern for Svelte 5 that extracts all `$state`, `$derived`, and action functions out of a `+page.svelte` file into a dedicated `.svelte.ts` file. The file exports a factory function (`createXState()`) that returns a public interface object with typed getters and setters.

> **Relevance:** Used in the SAKE project — `request/logic.svelte.ts` and `upload/logic.svelte.ts` each export a state factory consumed by their respective pages.

---

## Why this pattern exists

Svelte 5 runes (`$state`, `$derived`) only work inside `.svelte` files or files ending in `.svelte.ts` / `.svelte.js`. The factory pattern lets you move complex logic out of the component while keeping reactivity intact.

| Approach | Where logic lives | Reactive? |
|---|---|---|
| Inline in `<script>` | `+page.svelte` | ✓ |
| [[Svelte Stores]] | Separate `.ts` file | ✓ (but different API) |
| State factory | `.svelte.ts` file | ✓ (runes work here) |

Use the factory pattern when a page's `<script>` block is getting long and you want to separate logic from markup without giving up rune-based reactivity.

---

## File naming

The file **must** end in `.svelte.ts` for runes to work:

```
src/routes/request/
├── +page.svelte          ← markup only, imports the factory
└── logic.svelte.ts       ← all state, derived, and actions
```

A plain `.ts` file will not compile — the Svelte compiler only activates rune support for `.svelte.ts` / `.svelte.js` extensions.

---

## Structure of the factory function

```ts
// src/routes/request/logic.svelte.ts

export function createRequestState() {
  // ── State ──────────────────────────────────────────────
  let currentStep = $state(1);
  let jiraTicket  = $state('');
  let generating  = $state(false);

  // ── Derived ────────────────────────────────────────────
  let emailValid = $derived(jiraTicket.length > 0);

  // ── Actions ────────────────────────────────────────────
  function nextStep() {
    currentStep++;
  }

  // ── Public interface ───────────────────────────────────
  return {
    get currentStep() { return currentStep; },
    get jiraTicket()  { return jiraTicket; },
    set jiraTicket(v: string) { jiraTicket = v; },
    get generating()  { return generating; },
    get emailValid()  { return emailValid; },
    nextStep,
  };
}
```

---

## Consuming the factory in a component

```svelte
<!-- src/routes/request/+page.svelte -->
<script lang="ts">
  import { createRequestState } from './logic.svelte.ts';

  const s = createRequestState();
</script>

<input bind:value={s.jiraTicket} />
<p>Step: {s.currentStep}</p>
<button onclick={s.nextStep} disabled={!s.emailValid}>Next</button>
```

The returned object `s` is reactive — reading `s.currentStep` in the template registers a dependency, and any change inside the factory re-renders the template automatically.

---

## Why getters and setters (not plain properties)

If you return plain values (`return { currentStep, jiraTicket }`), the values are copied at return time and lose reactivity. Getters ensure every read goes back to the live `$state` variable:

```ts
// ✗ Loses reactivity — currentStep is a snapshot
return { currentStep };

// ✓ Reactive — getter reads live $state on every access
return {
  get currentStep() { return currentStep; }
};
```

For writable state, add a setter so the component can bind to it:

```ts
get jiraTicket()          { return jiraTicket; },
set jiraTicket(v: string) { jiraTicket = v; },
```

This allows `bind:value={s.jiraTicket}` in the template to work correctly.

---

## Comparison with Svelte Stores

| | State Factory | [[Svelte Stores]] |
|---|---|---|
| Syntax | Svelte 5 runes | Svelte 4 `writable()` / `readable()` |
| Scope | Instance per component call | Singleton (shared across components) |
| Cross-component sharing | No (one factory per page) | Yes |
| TypeScript | Natural (plain TS return type) | Requires `Writable<T>` types |
| Access in template | `s.value` | `$store` (auto-subscription) |

Use the factory for **page-local** logic. Use stores (or [[Svelte Global State]]) when state must be shared across multiple pages/components.

---

## Related Topics

- [[Svelte Reactivity (Runes)]] — `$state`, `$derived`, `$effect` are the building blocks used inside the factory
- [[Svelte Global State]] — alternative for state shared across multiple components
- [[Svelte Stores]] — Svelte 4 approach; still valid for cross-component state
- [[TypeScript in Svelte]] — typing the factory's return value
- [[Svelte Components]] — where the factory is instantiated and consumed
- [[Unidirectional data Flow]] — the pattern this reinforces: state lives in one place, components read and dispatch
