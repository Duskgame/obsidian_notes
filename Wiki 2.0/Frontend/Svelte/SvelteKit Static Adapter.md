# SvelteKit Static Adapter

[SvelteKit Docs — adapter-static](https://kit.svelte.dev/docs/adapter-static) | [Docs — Prerendering](https://kit.svelte.dev/docs/page-options#prerender)

Der **Static Adapter** (`@sveltejs/adapter-static`) baut eine SvelteKit-App als **reine statische Dateien** (HTML, CSS, JavaScript) — kein Node.js-Server zur Laufzeit nötig. Der Output kann auf jedem Webserver gehostet werden, der statische Dateien ausliefert.

> **SAKE nutzt genau das:** Die SvelteKit-App wird als statische Dateien gebaut und in einem Nginx-Docker-Container auf Cloud Run gehostet.

---

## Konfiguration

### Installation
```bash
npm install -D @sveltejs/adapter-static
```

### svelte.config.js
```js
import adapter from '@sveltejs/adapter-static';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

export default {
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter({
      pages: 'build',          // Output-Ordner für HTML-Seiten
      assets: 'build',         // Output-Ordner für Assets
      fallback: 'index.html',  // Für Client-side Routing (SPA-Modus)
      precompress: false,       // gzip/brotli vorab komprimieren?
    })
  }
};
```

### src/routes/+layout.ts
```ts
// Schaltet SSR für die gesamte App aus
export const ssr = false;
export const prerender = true;
```

Diese zwei Zeilen sind **zentral** für den SPA-Modus mit Static Adapter.

---

## Was bedeuten ssr und prerender?

| Option | Bedeutung |
|---|---|
| `ssr = true` (Standard) | Server rendert HTML → braucht laufenden Server |
| `ssr = false` | Kein Server-Rendering → App läuft nur im Browser |
| `prerender = true` | Seiten werden bei Build-Zeit zu statischen HTML gebaut |
| `prerender = false` | Seiten werden dynamisch gerendert (bei `ssr = false` also im Browser) |

**SAKE-Kombination** (`ssr = false` + `prerender = true` + `fallback = 'index.html'`):
- Es wird eine `index.html` gebaut
- Der Browser übernimmt Routing und Rendering komplett
- Nginx gibt immer `index.html` zurück, SvelteKit-Router übernimmt

---

## Was passiert bei npm run build?

```
npm run build
         ↓
    Vite + SvelteKit
         ↓
    build/
    ├── index.html          ← Shell-HTML
    ├── _app/
    │   ├── immutable/
    │   │   ├── *.js        ← Kompiliertes JavaScript
    │   │   └── *.css       ← Kompiliertes CSS (inkl. Tailwind)
    │   └── chunks/
    └── favicon.png
```

Diese Dateien werden in den Docker-Container kopiert und von Nginx ausgeliefert.

---

## Nginx-Konfiguration für SPA

Da alle Routen zu `index.html` fallen sollen:

```nginx
server {
    listen 80;
    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Ohne `try_files` würde Nginx bei `/request` einen 404 zurückgeben, weil keine Datei `/request/index.html` existiert.

---

## Dockerfile für SAKE-Style Deployment

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json .
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

---

## Einschränkungen des Static Adapters

| Feature | Verfügbar? |
|---|---|
| Client-side Fetch | ✓ |
| `+page.ts` (load) | ✓ |
| `$env/static/public` | ✓ |
| `+page.server.ts` | ✗ (kein Server) |
| `+server.ts` (API Routes) | ✗ (kein Server) |
| `$env/static/private` | ✗ (kein Server) |
| Cookies server-side | ✗ |

Da SAKE keine Server-Funktionen braucht (GCP API-Calls direkt im Browser über Google-Login des Supporters), ist der Static Adapter ideal.

---

## Verknüpfte Themen

- [[SvelteKit]] — Überblick und Konfigurationsdateien
- [[SvelteKit Load Functions]] — was bei `ssr = false` in Load Functions funktioniert
- [[SvelteKit Routing]] — wie Routing im SPA-Modus funktioniert
