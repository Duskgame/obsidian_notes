# SvelteKit Load Functions

[SvelteKit Docs — Loading data](https://kit.svelte.dev/docs/load) | [Tutorial: Page data](https://learn.svelte.dev/tutorial/page-data)

**Load Functions** sind spezielle Funktionen in SvelteKit, die Daten laden **bevor** eine Seite gerendert wird. Das Ergebnis wird als Props an die `+page.svelte` übergeben.

> **Für SAKE:** Da SAKE `ssr = false` und `prerender = true` nutzt (nur statische Dateien), werden nur `+page.ts` (client-side load) verwendet — kein `+page.server.ts`.

---

## +page.ts — Client-Side Load

```ts
// src/routes/keys/+page.ts
import type { PageLoad } from './$types';

export const load: PageLoad = async ({ fetch, params, url }) => {
  const res = await fetch('/api/keys');
  if (!res.ok) throw new Error('Laden fehlgeschlagen');
  
  const keys = await res.json();
  return { keys };
};
```

```svelte
<!-- src/routes/keys/+page.svelte -->
<script lang="ts">
  import type { PageData } from './$types';

  let { data } = $props<{ data: PageData }>();
  // data.keys ist jetzt verfügbar
</script>

{#each data.keys as key}
  <p>{key.id}</p>
{/each}
```

Die Load Function gibt ein Objekt zurück → dieses Objekt kommt als `data` Prop in der Seite an.

---

## Verfügbare Parameter in load()

```ts
export const load: PageLoad = async ({
  fetch,          // Svelte-eigenes fetch (mit Cookies, Base URL, etc.)
  params,         // Route-Parameter, z.B. params.keyId
  url,            // URL-Objekt mit pathname, searchParams, etc.
  parent,         // Daten aus dem übergeordneten Layout-Load
}) => {
  return {};
};
```

### fetch in Load vs. Browser-fetch

Der `fetch` in Load Functions ist eine erweiterte Version des Browser-`fetch`:
- Fügt automatisch Cookies hinzu
- Respektiert SvelteKit-interne Routing-Mechanismen
- Funktioniert auch bei SSR (serverseitig)

---

## +layout.ts — Daten für alle Seiten

```ts
// src/routes/+layout.ts
import type { LayoutLoad } from './$types';

export const load: LayoutLoad = async ({ fetch }) => {
  const res = await fetch('/api/user');
  const user = await res.json();
  return { user };
};
```

```svelte
<!-- src/routes/+layout.svelte -->
<script lang="ts">
  let { data, children } = $props();
  // data.user ist in allen Seiten verfügbar
</script>

<p>Eingeloggt als: {data.user.email}</p>
{@render children()}
```

Die Layout-Daten werden automatisch in den Seiten-Daten zusammengeführt.

---

## Fehler werfen

```ts
import { error } from '@sveltejs/kit';

export const load: PageLoad = async ({ params }) => {
  const key = await fetchKey(params.keyId);
  
  if (!key) {
    throw error(404, 'Key nicht gefunden');
  }
  
  return { key };
};
```

SvelteKit zeigt automatisch die `+error.svelte` der nächsten übergeordneten Route.

---

## Weiterleitung

```ts
import { redirect } from '@sveltejs/kit';

export const load: PageLoad = async ({ url }) => {
  const isLoggedIn = checkAuth();
  
  if (!isLoggedIn) {
    throw redirect(303, `/login?redirect=${url.pathname}`);
  }
  
  return {};
};
```

---

## Wann Load Functions vs. onMount?

| Situation | Empfehlung |
|---|---|
| Daten die die Seite grundlegend braucht | Load Function (`+page.ts`) |
| Seite rendert, dann Daten nachladen | `onMount` |
| Daten abhängig von User-Interaktion | `onMount` / Event-Handler |
| Daten aus URL-Params/Query | Load Function (hat Zugriff auf `params`, `url`) |

---

## Beispiel: Key-Detail-Seite (SAKE-Kontext)

```ts
// src/routes/keys/[keyId]/+page.ts
import type { PageLoad } from './$types';
import { error } from '@sveltejs/kit';

export const load: PageLoad = async ({ params, fetch }) => {
  const res = await fetch(`/api/keys/${params.keyId}`);
  
  if (res.status === 404) throw error(404, 'Key nicht gefunden');
  if (!res.ok) throw error(500, 'Server-Fehler');
  
  const key = await res.json();
  return { key, keyId: params.keyId };
};
```

```svelte
<!-- src/routes/keys/[keyId]/+page.svelte -->
<script lang="ts">
  let { data } = $props();
</script>

<h1>Service Account Key</h1>
<p>ID: {data.keyId}</p>
<p>SA: {data.key.serviceAccount}</p>
<p>Läuft ab: {data.key.expires}</p>
```

---

## Verknüpfte Themen

- [[SvelteKit Routing]] — Dateistruktur für Routes
- [[SvelteKit Static Adapter]] — was bei `ssr = false` passiert
- [[Fetch in Svelte]] — fetch in Komponenten direkt
