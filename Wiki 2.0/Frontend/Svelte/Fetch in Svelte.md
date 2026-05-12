# Fetch in Svelte — HTTP Requests

[MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) | [SvelteKit Docs — load](https://kit.svelte.dev/docs/load)

HTTP requests in Svelte frontends use the native browser **Fetch API**. No separate HTTP package needed. There are two main places where fetch makes sense: in `onMount` (for client-side fetching) or in SvelteKit load functions (for server/client-side fetching before rendering).

---

## Basic fetch pattern

```svelte
<script lang="ts">
  import { onMount } from 'svelte';

  interface Key {
    id: string;
    expires: string;
    serviceAccount: string;
  }

  let keys = $state<Key[]>([]);
  let loading = $state(true);
  let error = $state<string | null>(null);

  onMount(async () => {
    try {
      const res = await fetch('/api/keys');
      if (!res.ok) throw new Error(`HTTP ${res.status}`);
      keys = await res.json();
    } catch (e) {
      error = e instanceof Error ? e.message : "Unknown error";
    } finally {
      loading = false;
    }
  });
</script>

{#if loading}
  <p>Loading...</p>
{:else if error}
  <p>Error: {error}</p>
{:else}
  {#each keys as key (key.id)}
    <p>{key.id} — {key.serviceAccount}</p>
  {/each}
{/if}
```

---

## POST request (sending data)

```svelte
<script lang="ts">
  let jiraTicket = $state("");
  let serviceAccount = $state("");
  let submitting = $state(false);

  async function submitRequest() {
    submitting = true;
    try {
      const res = await fetch('/api/request-key', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ jiraTicket, serviceAccount }),
      });
      if (!res.ok) throw new Error(await res.text());
      alert("Request sent!");
    } catch (e) {
      alert("Error: " + e);
    } finally {
      submitting = false;
    }
  }
</script>

<form onsubmit={(e) => { e.preventDefault(); submitRequest(); }}>
  <input bind:value={jiraTicket} placeholder="JIRA-123" />
  <input bind:value={serviceAccount} placeholder="sa@project.iam.gserviceaccount.com" />
  <button type="submit" disabled={submitting}>
    {submitting ? "Sending..." : "Submit"}
  </button>
</form>
```

---

## Fetch with {#await} directly in the template

For simple cases without complex error handling:

```svelte
<script lang="ts">
  const keysPromise = fetch('/api/keys').then(r => r.json());
</script>

{#await keysPromise}
  <p>Loading keys...</p>
{:then keys}
  {#each keys as key}
    <p>{key.id}</p>
  {/each}
{:catch error}
  <p>Error: {error.message}</p>
{/await}
```

---

## Fetch with Google API (SAKE context)

SAKE makes GCP API calls directly in the browser using the supporter's Google access token:

```ts
async function listServiceAccountKeys(serviceAccountEmail: string, accessToken: string) {
  const url = `https://iam.googleapis.com/v1/projects/-/serviceAccounts/${serviceAccountEmail}/keys`;
  const res = await fetch(url, {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  });
  if (!res.ok) throw new Error(`GCP API error: ${res.status}`);
  return res.json();
}
```

---

## Fetch options overview

```ts
fetch(url, {
  method: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token',
  },
  body: JSON.stringify(data),    // only for POST/PUT/PATCH
  credentials: 'include',        // send cookies
  signal: abortController.signal, // make request cancellable
})
```

---

## Cancelling a request (AbortController)

```svelte
<script lang="ts">
  import { onDestroy } from 'svelte';

  let controller = new AbortController();
  let result = $state<string | null>(null);

  async function startLongRequest() {
    try {
      const res = await fetch('/api/slow-operation', {
        signal: controller.signal,
      });
      result = await res.text();
    } catch (e) {
      if (e instanceof Error && e.name === 'AbortError') {
        console.log("Request cancelled");
      }
    }
  }

  onDestroy(() => controller.abort());
</script>

<button onclick={startLongRequest}>Start</button>
<button onclick={() => controller.abort()}>Cancel</button>
```

---

## Related Topics

- [[Svelte Template Logic]] — `{#await}` for async data
- [[Svelte Lifecycle]] — `onMount` for initial data loading
- [[SvelteKit Load Functions]] — fetch in SvelteKit before rendering
