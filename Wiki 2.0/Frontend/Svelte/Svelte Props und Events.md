# Svelte Props und Events

[Svelte Docs — $props](https://svelte.dev/docs/svelte/$props) | [Tutorial: Props](https://learn.svelte.dev/tutorial/declaring-props)

**Props** (Properties) sind der Weg, Daten von einer Eltern-Komponente an eine Kind-Komponente zu übergeben. **Events** (in Svelte 5: normale Event-Handler) steuern die Kommunikation in die andere Richtung — vom Kind zur Eltern-Komponente.

---

## Props mit $props()

```svelte
<!-- KeyCard.svelte -->
<script lang="ts">
  let { keyId, expires, onDelete } = $props<{
    keyId: string;
    expires: string;
    onDelete: () => void;
  }>();
</script>

<div>
  <p>{keyId} — läuft ab: {expires}</p>
  <button onclick={onDelete}>Löschen</button>
</div>
```

```svelte
<!-- App.svelte -->
<script lang="ts">
  import KeyCard from './KeyCard.svelte';

  function handleDelete() {
    console.log("Key gelöscht");
  }
</script>

<KeyCard keyId="key-123" expires="2025-09-01" onDelete={handleDelete} />
```

### Default-Werte für Props

```svelte
<script lang="ts">
  let { title = "Unbekannter Key", required = false } = $props<{
    title?: string;
    required?: boolean;
  }>();
</script>
```

---

## Props weitergeben (Spread)

```svelte
<script lang="ts">
  let { class: className, ...rest } = $props();
</script>

<button class={className} {...rest}>
  <slot />
</button>
```

`...rest` leitet alle nicht explizit destrukturierten Props als HTML-Attribute weiter. Nützlich für generische Wrapper-Komponenten.

---

## Bindbare Props mit $bindable()

In Svelte 5 kann eine Komponente explizit erlauben, dass eine Prop von außen per `bind:` gebunden wird:

```svelte
<!-- SearchInput.svelte -->
<script lang="ts">
  let { value = $bindable("") } = $props<{ value?: string }>();
</script>

<input bind:value />
```

```svelte
<!-- Elternkomponente -->
<script lang="ts">
  let query = $state("");
</script>

<SearchInput bind:value={query} />
<p>Suche: {query}</p>
```

---

## Events in Svelte 5

Svelte 5 verwendet **native DOM-Event-Handler** statt der alten `on:`-Direktiven. Das sind einfach Attribute wie `onclick`, `oninput`, `onsubmit`.

```svelte
<button onclick={() => console.log("Geklickt!")}>Klick mich</button>

<input oninput={(e) => console.log(e.currentTarget.value)} />

<form onsubmit={(e) => { e.preventDefault(); handleSubmit(); }}>
  <button type="submit">Absenden</button>
</form>
```

> **Merke:** In Svelte 4 war es `on:click`, in Svelte 5 ist es `onclick` (kein Doppelpunkt, lowercase).

### Event-Handler als Prop übergeben

```svelte
<!-- Button.svelte -->
<script lang="ts">
  let { onclick, label } = $props<{
    onclick: () => void;
    label: string;
  }>();
</script>

<button {onclick}>{label}</button>
```

```svelte
<!-- App.svelte -->
<Button onclick={() => alert("Geklickt!")} label="Key rotieren" />
```

---

## Datenfluss-Prinzip

```
Eltern → Kind : über Props
Kind → Eltern : über Callback-Funktionen (als Props übergeben)
```

```
App.svelte
  ├── props: keyId, expires, onDelete ──→ KeyCard.svelte
  └── callback: onDelete() ←─────────── KeyCard.svelte (onclick)
```

Das ist das **Unidirectional Data Flow**-Muster: Daten fließen immer von oben nach unten. Änderungen werden über Callbacks nach oben gemeldet.

---

## Verknüpfte Themen

- [[Svelte Komponenten]] — wie Komponenten grundsätzlich aufgebaut sind
- [[Svelte Reaktivität (Runes)]] — `$bindable()` ist ebenfalls eine Rune
- [[Svelte Stores]] — Alternative wenn Props durch viele Ebenen weitergegeben werden müssen (Prop Drilling)
