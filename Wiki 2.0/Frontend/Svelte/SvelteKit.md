# SvelteKit

[SvelteKit Docs](https://kit.svelte.dev/docs/introduction) | [Tutorial](https://learn.svelte.dev/tutorial/kit-introduction)

**SvelteKit** is the official meta-framework for Svelte. It brings everything a complete web app needs:

- File-based routing
- Server-side rendering (SSR) and static site generation (SSG)
- Data loading before rendering (load functions)
- Deployment adapters (static, Node, Cloudflare, etc.)
- Vite as the build tool

> **Relevant for SAKE:** SvelteKit with `adapter-static` + `ssr = false` = pure client-side app, deployable as static HTML/JS/CSS files on Cloud Run (Nginx).

---

## Project structure

```
my-app/
├── src/
│   ├── app.html              ← HTML shell for all pages
│   ├── app.css               ← global styles
│   ├── lib/                  ← shared code (components, utils, stores)
│   │   ├── components/
│   │   └── stores.ts
│   └── routes/               ← pages (file-based routing)
│       ├── +layout.svelte    ← root layout (header, footer)
│       ├── +page.svelte      ← home page (/)
│       └── request/
│           └── +page.svelte  ← /request
├── static/                   ← static assets (images, fonts)
├── svelte.config.js          ← SvelteKit configuration
├── vite.config.ts            ← Vite configuration
└── package.json
```

---

## Creating a new project

```bash
npx sv create my-app
cd my-app
npm install
npm run dev
```

`sv` is the official SvelteKit CLI tool. It asks about TypeScript, Tailwind, Prettier, etc. during setup.

---

## Key config files

### svelte.config.js
```js
import adapter from '@sveltejs/adapter-static';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

export default {
  preprocess: vitePreprocess(),  // TypeScript, Tailwind, etc.
  kit: {
    adapter: adapter({
      fallback: 'index.html'     // for client-side routing
    })
  }
};
```

### src/routes/+layout.ts (for static adapter)
```ts
// Disable SSR for the entire app
export const ssr = false;
export const prerender = true;
```

---

## Dev server and build

```bash
npm run dev       # development server with hot reload
npm run build     # production build
npm run preview   # test the production build locally
```

The output of `npm run build` goes into `build/` — these are the static files hosted on Cloud Run as an Nginx server.

---

## $lib — the importable shared folder

```ts
// In any .svelte or .ts file:
import { user } from '$lib/stores';
import Button from '$lib/components/Button.svelte';
import { formatDate } from '$lib/utils';
```

`$lib` is a path alias for `src/lib/`. No relative paths like `../../lib/stores` needed.

---

## Environments and variables

SvelteKit distinguishes between public and private env variables:

```bash
# .env
PUBLIC_API_URL=https://api.example.com   # visible in the browser
API_SECRET=secret123                      # server-only
```

```ts
// In frontend code
import { PUBLIC_API_URL } from '$env/static/public';

// In server code (+page.server.ts, +server.ts)
import { API_SECRET } from '$env/static/private';
```

> With `ssr = false` there is no server code — all variables must be `PUBLIC_`.

---

## Related Topics

- [[SvelteKit Routing]] — how pages and layouts work
- [[SvelteKit Load Functions]] — loading data before a page renders
- [[SvelteKit Static Adapter]] — how SAKE is deployed
