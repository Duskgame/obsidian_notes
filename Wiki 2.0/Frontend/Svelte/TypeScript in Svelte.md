# TypeScript in Svelte

[Svelte Docs — TypeScript](https://svelte.dev/docs/svelte/typescript) | [SvelteKit Docs — Types](https://kit.svelte.dev/docs/types)

TypeScript kann in Svelte-Dateien direkt verwendet werden — einfach `lang="ts"` zum Script-Tag hinzufügen. SvelteKit ist von Haus aus auf TypeScript ausgelegt.

---

## Grundlegende Einrichtung

```svelte
<!-- lang="ts" aktiviert TypeScript -->
<script lang="ts">
  let count: number = $state(0);
  let label: string = $derived(count === 1 ? "Mal" : "Male");
</script>
```

---

## Typen für $state und $derived

```svelte
<script lang="ts">
  // Expliziter Typ
  let name = $state<string>("");
  
  // TypeScript inferiert den Typ automatisch
  let count = $state(0);           // number
  let active = $state(false);      // boolean
  
  // Arrays und Objekte
  let keys = $state<string[]>([]);
  let user = $state<User | null>(null);
  
  // Mit Interface
  interface ServiceAccountKey {
    id: string;
    serviceAccount: string;
    expires: string;
    isExpired: boolean;
  }
  
  let selectedKey = $state<ServiceAccountKey | null>(null);
</script>
```

---

## Typen für Props ($props)

```svelte
<!-- KeyCard.svelte -->
<script lang="ts">
  interface Props {
    keyId: string;
    serviceAccount: string;
    expires: string;
    onDelete?: () => void;        // Optional
    variant?: "default" | "danger"; // Union Type
  }

  let {
    keyId,
    serviceAccount,
    expires,
    onDelete,
    variant = "default",          // Default-Wert
  } = $props<Props>();
</script>
```

---

## SvelteKit-generierte Typen ($types)

SvelteKit generiert automatisch Typen für jede Route unter `.svelte-kit/types/`. Diese können importiert werden:

```ts
// src/routes/keys/+page.ts
import type { PageLoad } from './$types';

export const load: PageLoad = async ({ params, fetch }) => {
  return { keyId: params.keyId };
};
```

```svelte
<!-- src/routes/keys/+page.svelte -->
<script lang="ts">
  import type { PageData } from './$types';

  let { data } = $props<{ data: PageData }>();
  // data.keyId ist typsicher
</script>
```

Verfügbare `$types`:
- `PageLoad` — Typ für `+page.ts` load Function
- `LayoutLoad` — Typ für `+layout.ts` load Function
- `PageData` — Typ für `data` Prop in `+page.svelte`
- `LayoutData` — Typ für `data` Prop in `+layout.svelte`

---

## Utility-Typen für Svelte

```ts
import type { Component, Snippet } from 'svelte';

// Typ für eine Svelte-Komponente
let MyComp: Component;

// Typ für einen Snippet (Svelte 5)
let header: Snippet;
let itemSnippet: Snippet<[{ id: string; name: string }]>; // mit Parameter
```

---

## tsconfig.json

SvelteKit erstellt automatisch eine `tsconfig.json`:

```json
{
  "extends": "./.svelte-kit/tsconfig.json",
  "compilerOptions": {
    "strict": true,
    "moduleResolution": "bundler"
  }
}
```

Die `.svelte-kit/tsconfig.json` wird von SvelteKit generiert und enthält Path-Aliases wie `$lib`.

---

## Häufige TypeScript-Muster in Svelte

### Event-Handler-Typen

```svelte
<script lang="ts">
  function handleInput(e: Event) {
    const target = e.currentTarget as HTMLInputElement;
    console.log(target.value);
  }

  function handleSubmit(e: SubmitEvent) {
    e.preventDefault();
  }
</script>

<input oninput={handleInput} />
<form onsubmit={handleSubmit}></form>
```

### Async Functions

```svelte
<script lang="ts">
  async function fetchKey(id: string): Promise<ServiceAccountKey> {
    const res = await fetch(`/api/keys/${id}`);
    return res.json() as Promise<ServiceAccountKey>;
  }
</script>
```

### Enums und Union Types

```ts
// src/lib/types.ts
export type UserRole = "requester" | "supporter" | "admin";

export interface KeyRequest {
  jiraTicket: string;
  serviceAccountEmail: string;
  expiresInDays: number;
  status: "pending" | "approved" | "rejected";
}
```

---

## Verknüpfte Themen

- [[Svelte Komponenten]] — `lang="ts"` im Script-Tag
- [[Svelte Reaktivität (Runes)]] — generische Rune-Typen
- [[SvelteKit Load Functions]] — `$types` Importe
