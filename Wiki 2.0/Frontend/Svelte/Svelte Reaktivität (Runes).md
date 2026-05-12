# Svelte Reaktivität — Runes

[Svelte Docs — Runes](https://svelte.dev/docs/svelte/what-are-runes) | [Tutorial: State](https://learn.svelte.dev/tutorial/state)

**Runes** sind spezielle Compiler-Direktiven in Svelte 5, die mit `$` beginnen. Sie ersetzen das implizite Reaktivitätssystem aus Svelte 4. Runes sehen aus wie Funktionsaufrufe, sind aber keine echten Funktionen — der Svelte-Compiler versteht sie und übersetzt sie.

> Runes funktionieren auch in normalen `.ts`/`.js`-Dateien (nicht nur in `.svelte`), wenn die Datei auf `.svelte.ts` / `.svelte.js` endet.

---

## $state — reaktiver Zustand

```svelte
<script lang="ts">
  let count = $state(0);
</script>

<button onclick={() => count++}>
  Geklickt: {count}
</button>
```

Jede Änderung an `count` löst automatisch ein Re-Render aus. Vergleich mit Svelte 4: früher reichte `let count = 0` — in Svelte 5 muss explizit `$state()` verwendet werden.

### Reaktive Objekte und Arrays

```svelte
<script lang="ts">
  let user = $state({ name: "Jonas", role: "Requester" });
  let items = $state<string[]>([]);
</script>

<p>{user.name} — {user.role}</p>
<button onclick={() => items.push("neuer Eintrag")}>Hinzufügen</button>
```

Bei Objekten und Arrays werden auch tief verschachtelte Änderungen erkannt (Deep Reactivity via Proxies).

---

## $derived — berechnete Werte

```svelte
<script lang="ts">
  let count = $state(0);
  let doubled = $derived(count * 2);
  let label = $derived(count === 1 ? "Mal" : "Male");
</script>

<p>{count} × 2 = {doubled}</p>
<p>Geklickt {count} {label}</p>
```

`$derived` berechnet einen Wert neu, sobald sich eine seiner Abhängigkeiten ändert. Der Ausdruck darf keine Seiteneffekte haben.

### $derived.by für komplexe Berechnungen

```svelte
<script lang="ts">
  let items = $state([1, 2, 3, 4, 5]);
  let total = $derived.by(() => {
    return items.reduce((sum, n) => sum + n, 0);
  });
</script>
```

---

## $effect — Seiteneffekte

```svelte
<script lang="ts">
  import { onMount } from 'svelte';

  let query = $state("");

  $effect(() => {
    console.log("Query geändert:", query);
    // Hier können API-Calls, localStorage, etc. stehen
  });
</script>
```

`$effect` läuft:
1. Einmal nach dem ersten Render
2. Erneut, sobald eine der im Effect verwendeten `$state`-Variablen sich ändert

### Cleanup in $effect

```svelte
<script lang="ts">
  $effect(() => {
    const interval = setInterval(() => console.log("tick"), 1000);
    return () => clearInterval(interval); // wird vor jedem Re-Run und beim Unmount aufgerufen
  });
</script>
```

---

## $state.raw — nicht-reaktive Werte

Wenn ein großes Objekt nicht tief beobachtet werden soll (Performance):

```svelte
<script lang="ts">
  let data = $state.raw({ large: "object" });
  // Nur Neuzuweisung löst Update aus, keine Property-Änderungen
  data = { large: "updated" }; // ✓ reaktiv
  data.large = "mutiert";      // ✗ kein Update
</script>
```

---

## Zusammenfassung

| Rune | Zweck | Wann benutzen |
|---|---|---|
| `$state(value)` | Reaktiver State | Variablen die sich ändern und die UI updaten sollen |
| `$derived(expr)` | Berechneter Wert | Werte die von anderem State abhängen |
| `$derived.by(fn)` | Komplexe Berechnung | Mehrere Schritte, `.reduce()`, etc. |
| `$effect(fn)` | Seiteneffekte | API-Calls, localStorage, Logging |
| `$state.raw(v)` | Nicht-deep-reaktiv | Große Objekte, wo nur Neuzuweisung relevant ist |

---

## Verknüpfte Themen

- [[Svelte Komponenten]] — wo Runes verwendet werden
- [[Svelte Props und Events]] — `$props()` ist ebenfalls eine Rune
- [[Svelte Stores]] — Alternative für komponentenübergreifenden State
