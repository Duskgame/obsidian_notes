# SvelteKit Load Functions

[SvelteKit Docs — Loading data](https://kit.svelte.dev/docs/load) | [Tutorial: Page data](https://learn.svelte.dev/tutorial/page-data)

**Load functions** are special functions in SvelteKit that load data **before** a page is rendered. The result is passed as props to `+page.svelte`.

> **For SAKE:** Since SAKE uses `ssr = false` and `prerender = true` (static files only), only `+page.ts` (client-side load) is used — no `+page.server.ts`.

---

## +page.ts — client-side load

```ts
// src/routes/keys/+page.ts
import type { PageLoad } from './$types';

export const load: PageLoad = async ({ fetch, params, url }) => {
  const res = await fetch('/api/keys');
  if (!res.ok) throw new Error('Loading failed');

  const keys = await res.json();
  return { keys };
};
```

```svelte
<!-- src/routes/keys/+page.svelte -->
<script lang="ts">
  import type { PageData } from './$types';

  let { data } = $props<{ data: PageData }>();
  // data.keys is now available
</script>

{#each data.keys as key}
  <p>{key.id}</p>
{/each}
```

The load function returns an object → this object arrives as the `data` prop on the page.

---

## Available parameters in load()

```ts
export const load: PageLoad = async ({
  fetch,          // Svelte's own fetch (with cookies, base URL, etc.)
  params,         // route parameters, e.g. params.keyId
  url,            // URL object with pathname, searchParams, etc.
  parent,         // data from the parent layout's load
}) => {
  return {};
};
```

### fetch in load vs. browser fetch

The `fetch` in load functions is an enhanced version of browser `fetch`:
- Automatically adds cookies
- Respects SvelteKit's internal routing mechanisms
- Also works during SSR (server-side)

---

## +layout.ts — data for all pages

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
  // data.user is available in all pages
</script>

<p>Logged in as: {data.user.email}</p>
{@render children()}
```

Layout data is automatically merged into the page data.

---

## Throwing errors

```ts
import { error } from '@sveltejs/kit';

export const load: PageLoad = async ({ params }) => {
  const key = await fetchKey(params.keyId);

  if (!key) {
    throw error(404, 'Key not found');
  }

  return { key };
};
```

SvelteKit automatically shows the `+error.svelte` of the nearest parent route.

---

## Redirecting

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

## When to use load functions vs. onMount?

| Situation | Recommendation |
|---|---|
| Data the page fundamentally needs | Load function (`+page.ts`) |
| Page renders first, then loads data | `onMount` |
| Data depends on user interaction | `onMount` / event handler |
| Data from URL params/query | Load function (has access to `params`, `url`) |

---

## Example: key detail page (SAKE context)

```ts
// src/routes/keys/[keyId]/+page.ts
import type { PageLoad } from './$types';
import { error } from '@sveltejs/kit';

export const load: PageLoad = async ({ params, fetch }) => {
  const res = await fetch(`/api/keys/${params.keyId}`);

  if (res.status === 404) throw error(404, 'Key not found');
  if (!res.ok) throw error(500, 'Server error');

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
<p>Expires: {data.key.expires}</p>
```

---

## Related Topics

- [[SvelteKit Routing]] — file structure for routes
- [[SvelteKit Static Adapter]] — what happens with `ssr = false`
- [[Fetch in Svelte]] — fetch directly in components
