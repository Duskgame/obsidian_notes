# Svelte & SvelteKit — Trainingsplan

[Svelte Docs](https://svelte.dev/docs) | [SvelteKit Docs](https://kit.svelte.dev/docs) | [Svelte Tutorial](https://learn.svelte.dev)

Svelte ist ein **Compiler-basiertes** Frontend-Framework. Anders als React oder Vue läuft Svelte nicht als Runtime im Browser — der Compiler übersetzt Svelte-Code bei Build-Zeit in reines, optimiertes JavaScript. Das Ergebnis ist schneller und schlanker als klassische Frameworks.

**SvelteKit** ist das Meta-Framework über Svelte (vergleichbar mit Next.js für React). Es bringt Routing, Datenladen, und Deployment-Adapter mit.

> **Relevanz für SAKE:** Das SAKE-Frontend ist mit SvelteKit 2 + Svelte 5 + TypeScript + Tailwind CSS gebaut und wird als statische App auf Cloud Run gehostet.

---

## Lernpfad

### Phase 1 — Svelte Grundlagen (Woche 1–2)
1. [[Svelte Komponenten]] — `.svelte`-Dateien, Struktur, Script/Markup/Style
2. [[Svelte Reaktivität (Runes)]] — `$state`, `$derived`, `$effect`
3. [[Svelte Template-Logik]] — `{#if}`, `{#each}`, `{#await}`
4. [[Svelte Props und Events]] — `$props`, Event-Handler

### Phase 2 — Daten & Zustand (Woche 3)
5. [[Svelte Stores]] — `writable`, `readable`, `derived`
6. [[Svelte Lifecycle]] — `onMount`, `onDestroy`
7. [[Fetch in Svelte]] — HTTP-Requests, `async/await` im Browser

### Phase 3 — SvelteKit (Woche 4)
8. [[SvelteKit]] — Überblick, Projektstruktur
9. [[SvelteKit Routing]] — Datei-basiertes Routing, Layouts, `+page.svelte`
10. [[SvelteKit Load Functions]] — Daten vor dem Rendern laden
11. [[SvelteKit Static Adapter]] — Statisches Deployment (`prerender`, `ssr = false`)

### Phase 4 — Projekt-Stack (Woche 5)
12. [[TypeScript in Svelte]] — Typen in `.svelte`-Dateien
13. [[Tailwind CSS]] — Utility-first CSS

---

## Warum Svelte für SAKE?

| Eigenschaft | Vorteil für SAKE |
|---|---|
| Kein Runtime-Bundle | Kleinere Dateigröße, ideal für statisches Hosting |
| Compiler-basiert | TypeScript & Tailwind werden direkt integriert |
| SvelteKit Static Adapter | Perfekt für Cloud Run als reiner Fileserver |
| Svelte 5 Runes | Einfacheres Reaktivitätsmodell als React Hooks |

---

## Schnellreferenz: Svelte 5 vs. Svelte 4

| Feature | Svelte 4 | Svelte 5 |
|---|---|---|
| Reaktiver State | `let count = 0` (implizit) | `let count = $state(0)` |
| Computed Values | `$: doubled = count * 2` | `let doubled = $derived(count * 2)` |
| Side Effects | `$: { console.log(count) }` | `$effect(() => { console.log(count) })` |
| Props | `export let name` | `let { name } = $props()` |
| Events | `<button on:click={handler}>` | `<button onclick={handler}>` |
| Slots | `<slot />` | `{@render children()}` (Snippets) |

> SAKE nutzt Svelte 5 — lerne direkt die neue Syntax!
