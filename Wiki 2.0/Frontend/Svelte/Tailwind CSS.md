# Tailwind CSS

[Tailwind Docs](https://tailwindcss.com/docs) | [Tailwind mit SvelteKit](https://tailwindcss.com/docs/installation/framework-guides/sveltekit)

**Tailwind CSS** ist ein Utility-First CSS-Framework. Statt CSS-Klassen mit eigenem Namen zu schreiben, werden vordefinierte Utility-Klassen direkt im HTML verwendet. Kein separates CSS-File nötig — zumindest für den Großteil der Styles.

> **SAKE nutzt Tailwind** — alle visuellen Klassen im Svelte-Markup sind Tailwind-Klassen.

---

## Das Konzept

**Klassisches CSS:**
```css
.card {
  padding: 1rem;
  border-radius: 0.5rem;
  background-color: white;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}
```
```html
<div class="card">...</div>
```

**Tailwind:**
```html
<div class="p-4 rounded-lg bg-white shadow-sm">...</div>
```

Kein CSS schreiben — alles im Markup.

---

## Installation mit SvelteKit

```bash
npx sv create my-app
# → Tailwind beim Setup auswählen
```

Oder nachträglich:
```bash
npm install -D tailwindcss @tailwindcss/vite
```

```ts
// vite.config.ts
import { sveltekit } from '@sveltejs/kit/vite';
import tailwindcss from '@tailwindcss/vite';

export default {
  plugins: [tailwindcss(), sveltekit()],
};
```

```css
/* src/app.css */
@import "tailwindcss";
```

---

## Wichtigste Utility-Kategorien

### Abstände (Spacing)
```html
<div class="p-4">      <!-- padding: 1rem -->
<div class="px-4">     <!-- padding-left + right: 1rem -->
<div class="py-2">     <!-- padding-top + bottom: 0.5rem -->
<div class="m-4">      <!-- margin: 1rem -->
<div class="mt-2 mb-4"><!-- margin-top + bottom -->
<div class="gap-4">    <!-- gap in flexbox/grid: 1rem -->
```

Tailwind-Einheiten: Zahl × 4px (p-4 = 16px, p-1 = 4px, p-0.5 = 2px)

### Größen
```html
<div class="w-full">     <!-- width: 100% -->
<div class="w-64">       <!-- width: 16rem -->
<div class="h-screen">   <!-- height: 100vh -->
<div class="max-w-md">   <!-- max-width: 28rem -->
```

### Farben
```html
<div class="bg-blue-500">     <!-- Hintergrund blau -->
<p class="text-gray-700">     <!-- Textfarbe grau -->
<div class="border-red-500">  <!-- Rahmen rot -->
```

Farb-Skala: 50 (hell) → 950 (dunkel)

### Typography
```html
<h1 class="text-2xl font-bold text-gray-900">Titel</h1>
<p class="text-sm text-gray-600 leading-relaxed">Text</p>
<code class="font-mono text-sm bg-gray-100 px-1 rounded">code</code>
```

### Flexbox
```html
<div class="flex items-center justify-between gap-4">
  <span>Links</span>
  <span>Rechts</span>
</div>

<div class="flex flex-col gap-2">
  <p>Oben</p>
  <p>Unten</p>
</div>
```

### Grid
```html
<div class="grid grid-cols-3 gap-4">
  <div>1</div>
  <div>2</div>
  <div>3</div>
</div>
```

### Borders und Radius
```html
<div class="border border-gray-200 rounded-lg">
<div class="rounded-full">    <!-- Kreis -->
<div class="rounded-md">      <!-- leicht abgerundet -->
```

### Hover, Focus, Active
```html
<button class="bg-blue-500 hover:bg-blue-600 active:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">
  Button
</button>
```

---

## Responsive Design

Tailwind ist mobile-first. Breakpoint-Präfixe aktivieren Styles ab einer bestimmten Bildschirmbreite:

```html
<div class="flex-col md:flex-row">
  <!-- Unterhalb md: vertikal, ab md: horizontal -->
</div>

<!-- Breakpoints: sm(640px), md(768px), lg(1024px), xl(1280px), 2xl(1536px) -->
```

---

## Konditionale Klassen in Svelte

```svelte
<script lang="ts">
  let isError = $state(false);
  let isLoading = $state(false);
</script>

<!-- Ternary -->
<div class={isError ? "bg-red-50 border-red-500" : "bg-white border-gray-200"}>

<!-- Klassen-Binding -->
<button
  class="px-4 py-2 rounded font-medium"
  class:bg-blue-500={!isLoading}
  class:bg-gray-400={isLoading}
  class:cursor-not-allowed={isLoading}
  disabled={isLoading}
>
  {isLoading ? "Lädt..." : "Senden"}
</button>
```

### Mit clsx/cn (empfohlen für komplexe Fälle)

```bash
npm install clsx
```

```ts
import { clsx } from 'clsx';

const buttonClass = clsx(
  "px-4 py-2 rounded font-medium transition",
  isLoading && "opacity-50 cursor-not-allowed",
  variant === "danger" ? "bg-red-500 text-white" : "bg-blue-500 text-white"
);
```

---

## Häufige Komponenten-Patterns

### Button
```html
<button class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed transition">
  Key rotieren
</button>
```

### Card
```html
<div class="bg-white rounded-lg shadow-sm border border-gray-200 p-6">
  <h2 class="text-lg font-semibold text-gray-900 mb-2">Key Details</h2>
  <p class="text-sm text-gray-600">...</p>
</div>
```

### Input
```html
<input
  type="text"
  class="w-full px-3 py-2 border border-gray-300 rounded-md text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
  placeholder="JIRA-123"
/>
```

### Badge
```html
<span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800">
  Aktiv
</span>
```

---

## Verknüpfte Themen

- [[Svelte Komponenten]] — wo Tailwind-Klassen verwendet werden
- [[SvelteKit Static Adapter]] — Tailwind wird beim Build zu minimalem CSS kompiliert
