# Svelte Komponenten

[Svelte Docs — Components](https://svelte.dev/docs/svelte/overview) | [Tutorial: Your first component](https://learn.svelte.dev/tutorial/welcome-to-svelte)

Eine **Komponente** in Svelte ist eine `.svelte`-Datei. Sie enthält alles was eine UI-Einheit braucht: Logik, Markup, und Styles — in einer einzigen Datei.

---

## Aufbau einer .svelte-Datei

```svelte
<script lang="ts">
  // JavaScript / TypeScript Logik
  let name = "SAKE";
</script>

<!-- HTML-Markup (Template) -->
<h1>Willkommen bei {name}</h1>
<p>Ein Tool für GCP Key-Rotation.</p>

<style>
  h1 {
    color: navy;
  }
</style>
```

Die drei Bereiche:
- `<script>` — Logik, Variablen, Importe
- HTML-Bereich — Template mit `{ }` für Ausdrücke
- `<style>` — CSS, **automatisch scoped** (betrifft nur diese Komponente)

---

## Ausdrücke im Template

Jeder JavaScript-Ausdruck kann mit `{ }` in das Template eingebettet werden:

```svelte
<script lang="ts">
  let user = "Jonas";
  let now = new Date().toLocaleDateString();
</script>

<p>Hallo, {user}!</p>
<p>Heute ist {now}.</p>
<p>2 + 2 = {2 + 2}</p>
```

---

## Komponenten importieren und verwenden

```svelte
<!-- Button.svelte -->
<button>Klick mich</button>
```

```svelte
<!-- App.svelte -->
<script lang="ts">
  import Button from './Button.svelte';
</script>

<Button />
```

Komponenten werden wie HTML-Tags verwendet. Der Dateiname wird zum Tag-Namen.

---

## Attribute und Bindings

### Statische Attribute
```svelte
<img src="/logo.png" alt="Logo" />
```

### Dynamische Attribute
```svelte
<script lang="ts">
  let src = "/logo.png";
  let disabled = true;
</script>

<img {src} alt="Logo" />
<button {disabled}>Senden</button>
```

Kurzschreibweise: Wenn Attributname = Variablenname, reicht `{src}` statt `src={src}`.

### Two-Way Binding mit `bind:`
```svelte
<script lang="ts">
  let value = $state("");
</script>

<input bind:value />
<p>Eingabe: {value}</p>
```

`bind:value` synchronisiert die Variable mit dem Input-Feld in beide Richtungen.

---

## Scoped Styles

```svelte
<style>
  p {
    color: red; /* betrifft NUR <p> in dieser Komponente */
  }
</style>
```

Svelte fügt automatisch einen eindeutigen Klassen-Hash hinzu, damit Styles nicht in andere Komponenten "auslaufen". Globale Styles brauchen `:global()`:

```svelte
<style>
  :global(body) {
    margin: 0;
  }
</style>
```

---

## Verknüpfte Themen

- [[Svelte Reaktivität (Runes)]] — wie State in Komponenten funktioniert
- [[Svelte Props und Events]] — wie Daten zwischen Komponenten fließen
- [[TypeScript in Svelte]] — `lang="ts"` im Script-Tag
