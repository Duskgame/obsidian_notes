# SvelteKit Routing

[SvelteKit Docs — Routing](https://kit.svelte.dev/docs/routing) | [Tutorial: Pages](https://learn.svelte.dev/tutorial/pages)

SvelteKit verwendet **datei-basiertes Routing**: Der Ordnerpfad unter `src/routes/` bestimmt die URL. Keine separate Router-Konfiguration nötig.

---

## Dateikonventionen

| Datei | Zweck |
|---|---|
| `+page.svelte` | Die Seite (UI) für diese Route |
| `+page.ts` | Daten laden (client-side) für diese Seite |
| `+page.server.ts` | Daten laden (server-side), Form Actions |
| `+layout.svelte` | Layout-Wrapper für diese Route + alle Unterrouten |
| `+layout.ts` | Daten laden für das Layout |
| `+error.svelte` | Fehlerseite für diese Route |

---

## Grundlegendes Routing

```
src/routes/
├── +page.svelte           → /
├── +layout.svelte         → Layout für alle Seiten
├── request/
│   └── +page.svelte       → /request
├── supporter/
│   └── +page.svelte       → /supporter
└── about/
    └── +page.svelte       → /about
```

---

## Layout mit Navigation

```svelte
<!-- src/routes/+layout.svelte -->
<script lang="ts">
  let { children } = $props();
</script>

<nav>
  <a href="/">Startseite</a>
  <a href="/request">Key anfordern</a>
  <a href="/supporter">Supporter</a>
</nav>

<main>
  {@render children()}
</main>

<style>
  nav { display: flex; gap: 1rem; padding: 1rem; }
</style>
```

`{@render children()}` ist der Platzhalter für den Inhalt der jeweiligen Seite. (Svelte 5 Syntax — in Svelte 4 war es `<slot />`.)

---

## Dynamische Routen

```
src/routes/
└── keys/
    ├── +page.svelte           → /keys
    └── [keyId]/
        └── +page.svelte       → /keys/abc123
```

```svelte
<!-- src/routes/keys/[keyId]/+page.svelte -->
<script lang="ts">
  import { page } from '$app/stores';

  let keyId = $derived($page.params.keyId);
</script>

<h1>Key-Detail: {keyId}</h1>
```

### Mehrere Segmente

```
src/routes/org/[orgId]/project/[projectId]/+page.svelte
→ /org/bpde/project/key-rotation
```

---

## Navigation

```svelte
<script lang="ts">
  import { goto } from '$app/navigation';

  async function afterSubmit() {
    await goto('/success');         // Programmatisch navigieren
    await goto('/keys', { replaceState: true }); // Ohne Browser-History
  }
</script>

<!-- Deklarativ -->
<a href="/request">Key anfordern</a>
```

### $page Store

```svelte
<script lang="ts">
  import { page } from '$app/stores';
</script>

<p>Aktuelle URL: {$page.url.pathname}</p>
<p>Route Param: {$page.params.keyId}</p>
<p>Query: {$page.url.searchParams.get('q')}</p>
```

---

## Verschachtelte Layouts

```
src/routes/
├── +layout.svelte              ← Root Layout (Nav, Footer)
├── +page.svelte                ← /
└── admin/
    ├── +layout.svelte          ← Admin Layout (Sidebar)
    ├── +page.svelte            ← /admin
    └── users/
        └── +page.svelte        ← /admin/users
```

Admin-Seiten erhalten beide Layouts. Das Admin-Layout wird in das Root-Layout eingebettet.

---

## Route Groups

```
src/routes/
├── (public)/
│   ├── +layout.svelte      ← Layout für öffentliche Seiten
│   ├── +page.svelte        → /
│   └── about/
│       └── +page.svelte    → /about
└── (protected)/
    ├── +layout.svelte      ← Layout mit Auth-Check
    ├── request/
    │   └── +page.svelte    → /request
    └── supporter/
        └── +page.svelte    → /supporter
```

Klammern `()` im Ordnernamen erzeugen eine Route Group — der Name erscheint **nicht** in der URL. Nützlich um Seiten zu gruppieren ohne die URL zu beeinflussen.

---

## Links aktiv hervorheben

```svelte
<script lang="ts">
  import { page } from '$app/stores';

  function isActive(href: string) {
    return $page.url.pathname === href;
  }
</script>

<a href="/request" class:active={isActive('/request')}>Key anfordern</a>

<style>
  .active { font-weight: bold; border-bottom: 2px solid currentColor; }
</style>
```

---

## Verknüpfte Themen

- [[SvelteKit]] — Überblick und Projektstruktur
- [[SvelteKit Load Functions]] — Daten für Seiten laden
- [[Svelte Props und Events]] — `children` Prop in Layouts
