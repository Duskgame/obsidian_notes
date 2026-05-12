# Svelte Stores

[Svelte Docs — Stores](https://svelte.dev/docs/svelte/stores) | [Tutorial: Writable stores](https://learn.svelte.dev/tutorial/writable-stores)

Ein **Store** ist ein Objekt, das einen reaktiven Wert hält, der von **mehreren Komponenten** gleichzeitig abonniert werden kann — unabhängig von der Komponentenhierarchie. Stores lösen das Problem des "Prop Drilling" (Props durch viele Ebenen weiterzureichen).

Stores kommen aus `svelte/store`.

---

## writable — beschreibbarer Store

```ts
// stores.ts
import { writable } from 'svelte/store';

export const currentUser = writable<string | null>(null);
export const keyList = writable<string[]>([]);
```

```svelte
<!-- Login.svelte -->
<script lang="ts">
  import { currentUser } from './stores';

  function login() {
    currentUser.set("jonas@bonprix.net");
  }
</script>

<button onclick={login}>Einloggen</button>
```

```svelte
<!-- Header.svelte -->
<script lang="ts">
  import { currentUser } from './stores';
</script>

{#if $currentUser}
  <p>Eingeloggt als: {$currentUser}</p>
{/if}
```

### Store-API

| Methode | Beschreibung |
|---|---|
| `store.set(value)` | Setzt neuen Wert |
| `store.update(fn)` | Ändert Wert basierend auf aktuellem Wert |
| `store.subscribe(fn)` | Abonniert Änderungen (gibt Unsubscribe-Funktion zurück) |

```ts
keyList.update(keys => [...keys, "key-new-123"]);
```

---

## $ — Auto-Subscription in Svelte-Dateien

In `.svelte`-Dateien kann ein Store mit `$` als Präfix direkt verwendet werden:

```svelte
<script lang="ts">
  import { currentUser } from './stores';
  // Kein manuelles subscribe() nötig!
</script>

<p>{$currentUser}</p>
```

Svelte abonniert den Store automatisch und räumt das Abo beim Unmount auf. In `.ts`-Dateien (ohne Svelte) muss `subscribe()` manuell verwendet werden.

---

## readable — nur-lesbarer Store

```ts
import { readable } from 'svelte/store';

export const timestamp = readable(new Date(), (set) => {
  const interval = setInterval(() => set(new Date()), 1000);
  return () => clearInterval(interval); // Cleanup
});
```

```svelte
<script lang="ts">
  import { timestamp } from './stores';
</script>

<p>Aktuelle Zeit: {$timestamp.toLocaleTimeString()}</p>
```

`readable` bekommt einen Startwert und eine Setup-Funktion. Die Setup-Funktion wird beim ersten Abonnenten aufgerufen, die zurückgegebene Funktion beim letzten Unsubscribe.

---

## derived — abgeleiteter Store

```ts
import { derived } from 'svelte/store';
import { keyList } from './stores';

export const keyCount = derived(keyList, $keys => $keys.length);

export const expiredKeys = derived(keyList, $keys =>
  $keys.filter(k => k.isExpired)
);
```

`derived` berechnet einen neuen Wert jedes Mal wenn der Quell-Store sich ändert. Mehrere Quellen sind möglich:

```ts
export const summary = derived(
  [currentUser, keyList],
  ([$user, $keys]) => `${$user} hat ${$keys.length} Keys`
);
```

---

## Stores vs. $state — wann was?

| Szenario | Empfehlung |
|---|---|
| State nur in einer Komponente | `$state()` |
| State wird an Kind-Komponenten weitergegeben (1–2 Ebenen) | Props + `$state()` |
| State wird von vielen Komponenten benötigt | Store |
| State muss in `.ts`-Dateien leben | Store |
| Globale App-Daten (User, Auth-Token) | Store |

---

## Beispiel: Auth-Store für SAKE

```ts
// authStore.ts
import { writable, derived } from 'svelte/store';

interface User {
  email: string;
  role: "requester" | "supporter";
}

export const user = writable<User | null>(null);
export const isAuthenticated = derived(user, $user => $user !== null);
export const isSupporter = derived(user, $user => $user?.role === "supporter");
```

```svelte
<script lang="ts">
  import { isAuthenticated, isSupporter } from './authStore';
</script>

{#if $isAuthenticated}
  {#if $isSupporter}
    <SupporterView />
  {:else}
    <RequesterView />
  {/if}
{:else}
  <LoginPage />
{/if}
```

---

## Verknüpfte Themen

- [[Svelte Reaktivität (Runes)]] — `$state` für lokalen State
- [[Svelte Props und Events]] — Alternative zu Stores für einfache Weitergabe
- [[Svelte Lifecycle]] — Stores werden oft in `onMount` initialisiert
