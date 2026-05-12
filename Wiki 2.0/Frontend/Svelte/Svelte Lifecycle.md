# Svelte Lifecycle

[Svelte Docs — Lifecycle](https://svelte.dev/docs/svelte/lifecycle-hooks) | [Tutorial: onMount](https://learn.svelte.dev/tutorial/onmount)

Svelte-Komponenten durchlaufen einen Lebenszyklus: Erstellt werden, im DOM erscheinen, sich updaten, und wieder verschwinden. Lifecycle-Funktionen erlauben es, Code zu bestimmten Momenten dieses Zyklus auszuführen.

---

## onMount — nach dem ersten Render

```svelte
<script lang="ts">
  import { onMount } from 'svelte';

  let keys = $state<string[]>([]);

  onMount(async () => {
    const res = await fetch('/api/keys');
    keys = await res.json();
  });
</script>

{#each keys as key}
  <p>{key}</p>
{/each}
```

`onMount` wird aufgerufen **nachdem die Komponente im DOM erschienen ist**. Perfekt für:
- API-Calls beim Laden
- DOM-Manipulationen (z.B. Fokus setzen)
- Externe Libraries initialisieren (z.B. Chart.js)

> **Wichtig für SAKE:** Die Web Crypto API (`window.crypto`) ist nur im Browser verfügbar. Sie in `onMount` zu initialisieren stellt sicher, dass SSR keinen Fehler wirft — obwohl SAKE `ssr = false` hat, ist es eine gute Gewohnheit.

### Cleanup aus onMount

```svelte
<script lang="ts">
  import { onMount } from 'svelte';

  onMount(() => {
    const handler = () => console.log("Fenster resized");
    window.addEventListener('resize', handler);

    return () => window.removeEventListener('resize', handler); // Cleanup
  });
</script>
```

Die von `onMount` zurückgegebene Funktion wird beim Unmount ausgeführt (entspricht `onDestroy`).

---

## onDestroy — beim Entfernen der Komponente

```svelte
<script lang="ts">
  import { onDestroy } from 'svelte';

  const interval = setInterval(() => console.log("tick"), 1000);

  onDestroy(() => {
    clearInterval(interval);
  });
</script>
```

Nützlich um aufzuräumen: Subscriptions kündigen, Intervals löschen, Event Listener entfernen.

---

## $effect als Lifecycle-Alternative

In Svelte 5 übernimmt `$effect` viele Aufgaben die früher `afterUpdate` brauchten:

```svelte
<script lang="ts">
  let query = $state("");

  $effect(() => {
    // läuft nach jedem Render in dem `query` sich geändert hat
    document.title = `Suche: ${query}`;
  });
</script>
```

Vergleich:

| Lifecycle | Wann | Svelte 5 Alternative |
|---|---|---|
| `onMount` | Nach erstem DOM-Render | `onMount` (weiterhin empfohlen) |
| `onDestroy` | Beim Entfernen der Komponente | Cleanup-Funktion in `onMount` oder `$effect` |
| `beforeUpdate` | Vor jedem Re-Render | Selten benötigt |
| `afterUpdate` | Nach jedem Re-Render | `$effect` |

---

## Reihenfolge beim Laden

```
1. <script> wird ausgeführt (synchron)
2. Template wird gerendert (DOM-Elemente erstellt)
3. onMount-Callbacks werden ausgeführt
   └── async onMount: wartet nicht das Rendern ab
4. Bei Änderungen: Re-Render + $effect
5. Bei Komponent-Entfernung: onDestroy / $effect-Cleanup
```

---

## Beispiel: Crypto-Key-Generierung bei Mount (SAKE-Kontext)

```svelte
<script lang="ts">
  import { onMount } from 'svelte';

  let keyPair = $state<CryptoKeyPair | null>(null);

  onMount(async () => {
    keyPair = await window.crypto.subtle.generateKey(
      { name: "RSA-OAEP", modulusLength: 2048, publicExponent: new Uint8Array([1, 0, 1]), hash: "SHA-256" },
      true,
      ["encrypt", "decrypt"]
    );
    console.log("Keypair generiert:", keyPair);
  });
</script>

{#if keyPair}
  <p>Keypair bereit</p>
{:else}
  <p>Generiere Schlüsselpaar...</p>
{/if}
```

---

## Verknüpfte Themen

- [[Svelte Reaktivität (Runes)]] — `$effect` als moderner Lifecycle-Hook
- [[Fetch in Svelte]] — API-Calls typischerweise in `onMount`
- [[SvelteKit Load Functions]] — Alternative zu onMount für datenlastiges Laden
