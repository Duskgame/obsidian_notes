# Fetch in Svelte — HTTP-Requests

[MDN — Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) | [SvelteKit Docs — load](https://kit.svelte.dev/docs/load)

HTTP-Requests in Svelte-Frontends laufen über die native **Fetch API** des Browsers. Kein separates HTTP-Package nötig. Es gibt zwei Hauptorte wo Fetch sinnvoll eingesetzt wird: in `onMount` (für Client-side Fetching) oder in SvelteKit Load Functions (für Server/Client-side Fetching vor dem Rendern).

---

## Grundlegendes Fetch-Muster

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
      error = e instanceof Error ? e.message : "Unbekannter Fehler";
    } finally {
      loading = false;
    }
  });
</script>

{#if loading}
  <p>Lädt...</p>
{:else if error}
  <p>Fehler: {error}</p>
{:else}
  {#each keys as key (key.id)}
    <p>{key.id} — {key.serviceAccount}</p>
  {/each}
{/if}
```

---

## POST-Request (Daten senden)

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
      alert("Request gesendet!");
    } catch (e) {
      alert("Fehler: " + e);
    } finally {
      submitting = false;
    }
  }
</script>

<form onsubmit={(e) => { e.preventDefault(); submitRequest(); }}>
  <input bind:value={jiraTicket} placeholder="JIRA-123" />
  <input bind:value={serviceAccount} placeholder="sa@project.iam.gserviceaccount.com" />
  <button type="submit" disabled={submitting}>
    {submitting ? "Wird gesendet..." : "Absenden"}
  </button>
</form>
```

---

## Fetch mit {#await} direkt im Template

Für einfache Fälle ohne komplexe Fehlerbehandlung:

```svelte
<script lang="ts">
  const keysPromise = fetch('/api/keys').then(r => r.json());
</script>

{#await keysPromise}
  <p>Lädt Keys...</p>
{:then keys}
  {#each keys as key}
    <p>{key.id}</p>
  {/each}
{:catch error}
  <p>Fehler: {error.message}</p>
{/await}
```

---

## Fetch mit Google API (SAKE-Kontext)

SAKE macht GCP API-Calls direkt im Browser mit dem Google Access Token des Supporters:

```ts
async function listServiceAccountKeys(serviceAccountEmail: string, accessToken: string) {
  const url = `https://iam.googleapis.com/v1/projects/-/serviceAccounts/${serviceAccountEmail}/keys`;
  const res = await fetch(url, {
    headers: {
      Authorization: `Bearer ${accessToken}`,
    },
  });
  if (!res.ok) throw new Error(`GCP API Fehler: ${res.status}`);
  return res.json();
}
```

---

## Fetch-Optionen Übersicht

```ts
fetch(url, {
  method: 'GET' | 'POST' | 'PUT' | 'DELETE' | 'PATCH',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token',
  },
  body: JSON.stringify(data),   // nur bei POST/PUT/PATCH
  credentials: 'include',       // Cookies mitsenden
  signal: abortController.signal, // Request abbrechbar machen
})
```

---

## Request abbrechen (AbortController)

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
        console.log("Request abgebrochen");
      }
    }
  }

  onDestroy(() => controller.abort());
</script>

<button onclick={startLongRequest}>Starten</button>
<button onclick={() => controller.abort()}>Abbrechen</button>
```

---

## Verknüpfte Themen

- [[Svelte Template-Logik]] — `{#await}` für asynchrone Daten
- [[Svelte Lifecycle]] — `onMount` für initiales Datenladen
- [[SvelteKit Load Functions]] — Fetch in SvelteKit vor dem Rendern
