# SvelteKit

[SvelteKit Docs](https://kit.svelte.dev/docs/introduction) | [Tutorial](https://learn.svelte.dev/tutorial/kit-introduction)

**SvelteKit** ist das offizielle Meta-Framework für Svelte. Es bringt alles mit, was eine vollständige Web-App braucht:

- Datei-basiertes Routing
- Server-side Rendering (SSR) und Static Site Generation (SSG)
- Datenladen vor dem Rendern (Load Functions)
- Deployment-Adapter (Static, Node, Cloudflare, etc.)
- Vite als Build-Tool

> **Für SAKE relevant:** SvelteKit mit `adapter-static` + `ssr = false` = reine Client-Side-App, deploybar als statische HTML/JS/CSS-Dateien auf Cloud Run (Nginx).

---

## Projektstruktur

```
my-app/
├── src/
│   ├── app.html              ← HTML-Shell für alle Seiten
│   ├── app.css               ← Globale Styles
│   ├── lib/                  ← Shared Code (Components, Utils, Stores)
│   │   ├── components/
│   │   └── stores.ts
│   └── routes/               ← Seiten (datei-basiertes Routing)
│       ├── +layout.svelte    ← Root-Layout (Header, Footer)
│       ├── +page.svelte      ← Startseite (/)
│       └── request/
│           └── +page.svelte  ← /request
├── static/                   ← Statische Assets (Bilder, Fonts)
├── svelte.config.js          ← SvelteKit Konfiguration
├── vite.config.ts            ← Vite Konfiguration
└── package.json
```

---

## Neues Projekt erstellen

```bash
npx sv create my-app
cd my-app
npm install
npm run dev
```

`sv` ist das offizielle SvelteKit CLI-Tool. Es fragt beim Setup nach TypeScript, Tailwind, Prettier, etc.

---

## Wichtige Config-Dateien

### svelte.config.js
```js
import adapter from '@sveltejs/adapter-static';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

export default {
  preprocess: vitePreprocess(),  // TypeScript, Tailwind etc.
  kit: {
    adapter: adapter({
      fallback: 'index.html'     // für Client-side Routing
    })
  }
};
```

### src/routes/+layout.ts (für static adapter)
```ts
// Schaltet SSR für die gesamte App aus
export const ssr = false;
export const prerender = true;
```

---

## Dev-Server und Build

```bash
npm run dev       # Entwicklungsserver mit Hot Reload
npm run build     # Produktions-Build
npm run preview   # Produktions-Build lokal testen
```

Der Output von `npm run build` landet in `build/` — das sind die statischen Dateien, die auf Cloud Run als Nginx-Server gehostet werden.

---

## $lib — der importierbare Shared-Ordner

```ts
// In jeder .svelte oder .ts Datei:
import { user } from '$lib/stores';
import Button from '$lib/components/Button.svelte';
import { formatDate } from '$lib/utils';
```

`$lib` ist ein Path-Alias für `src/lib/`. Kein relativer Pfad wie `../../lib/stores` nötig.

---

## Environments und Variablen

SvelteKit unterscheidet zwischen Public und Private Env-Variablen:

```bash
# .env
PUBLIC_API_URL=https://api.example.com   # im Browser sichtbar
API_SECRET=secret123                      # nur auf dem Server
```

```ts
// Im Frontend-Code
import { PUBLIC_API_URL } from '$env/static/public';

// Im Server-Code (+page.server.ts, +server.ts)
import { API_SECRET } from '$env/static/private';
```

> Bei `ssr = false` gibt es keinen Server-Code — alle Variablen müssen `PUBLIC_` sein.

---

## Verknüpfte Themen

- [[SvelteKit Routing]] — wie Seiten und Layouts funktionieren
- [[SvelteKit Load Functions]] — Daten laden bevor die Seite gerendert wird
- [[SvelteKit Static Adapter]] — wie SAKE deployed wird
