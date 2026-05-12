# Svelte Template-Logik

[Svelte Docs — Logic blocks](https://svelte.dev/docs/svelte/if) | [Tutorial: Logic](https://learn.svelte.dev/tutorial/if-blocks)

Svelte erweitert HTML um Template-Blöcke für Bedingungen, Schleifen und asynchrone Daten. Sie beginnen mit `{#...}` und enden mit `{/...}`.

---

## {#if} — Bedingungen

```svelte
<script lang="ts">
  let isLoggedIn = $state(false);
</script>

{#if isLoggedIn}
  <p>Willkommen, du bist eingeloggt.</p>
{:else if role === "supporter"}
  <p>Supporter-Bereich</p>
{:else}
  <button onclick={() => isLoggedIn = true}>Login</button>
{/if}
```

- `{#if condition}` — Hauptbedingung
- `{:else if condition}` — weitere Bedingung
- `{:else}` — Fallback
- `{/if}` — Ende des Blocks

---

## {#each} — Schleifen

```svelte
<script lang="ts">
  let keys = $state([
    { id: "key-1", expires: "2025-06-01" },
    { id: "key-2", expires: "2025-09-01" },
  ]);
</script>

{#each keys as key}
  <p>{key.id} — läuft ab: {key.expires}</p>
{/each}
```

### Mit Index

```svelte
{#each keys as key, index}
  <p>{index + 1}. {key.id}</p>
{/each}
```

### Mit Key (wichtig bei Listen die sich ändern)

```svelte
{#each keys as key (key.id)}
  <p>{key.id}</p>
{/each}
```

Der Key `(key.id)` hilft Svelte dabei, Elemente beim Neuordnen korrekt zu tracken statt alle neu zu rendern.

### Fallback bei leerer Liste

```svelte
{#each keys as key (key.id)}
  <p>{key.id}</p>
{:else}
  <p>Keine Keys vorhanden.</p>
{/each}
```

---

## {#await} — Asynchrone Daten

```svelte
<script lang="ts">
  async function loadKeys(): Promise<string[]> {
    const res = await fetch("/api/keys");
    return res.json();
  }

  let keysPromise = loadKeys();
</script>

{#await keysPromise}
  <p>Laden...</p>
{:then keys}
  {#each keys as key}
    <p>{key}</p>
  {/each}
{:catch error}
  <p>Fehler: {error.message}</p>
{/await}
```

- `{#await promise}` — wird gezeigt solange der Promise läuft
- `{:then value}` — wird gezeigt wenn der Promise resolved
- `{:catch error}` — wird gezeigt wenn der Promise rejected

### Kurzform (nur Erfolgszustand)

```svelte
{#await keysPromise then keys}
  <p>{keys.length} Keys gefunden</p>
{/await}
```

---

## {#key} — Re-Render erzwingen

```svelte
<script lang="ts">
  let selectedKey = $state("key-1");
</script>

{#key selectedKey}
  <KeyDetail id={selectedKey} />
{/key}
```

Wenn `selectedKey` sich ändert, wird `KeyDetail` vollständig neu erstellt (statt nur geupdated). Nützlich für Animationen oder wenn eine Komponente bei Datenwechsel komplett neu initialisiert werden soll.

---

## Ausdrücke direkt im Template

Neben den Block-Direktiven kann auch direkt in den Tags HTML bedingt gesetzt werden:

```svelte
<button
  class={isActive ? "btn-primary" : "btn-secondary"}
  disabled={isLoading}
>
  {isLoading ? "Lädt..." : "Senden"}
</button>
```

---

## Verknüpfte Themen

- [[Svelte Reaktivität (Runes)]] — die `$state`-Variablen die in Blöcken verwendet werden
- [[Fetch in Svelte]] — Daten für `{#await}` abrufen
- [[Svelte Stores]] — reaktive Werte die aus Stores kommen
