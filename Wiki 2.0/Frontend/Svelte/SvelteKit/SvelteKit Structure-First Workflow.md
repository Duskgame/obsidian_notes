# SvelteKit Structure-First Workflow

[SvelteKit Docs — Project structure](https://kit.svelte.dev/docs/project-structure) | [Svelte Docs — Components](https://svelte.dev/docs/svelte/overview)

A workflow for building SvelteKit pages by writing the HTML structure first — no classes, no CSS — then extracting reusable components, and finally adding styles. This separates the structural problem from the visual problem so each can be solved independently.

> **Relevance:** Used when translating the SAKE HTML wireframes (`sake-prototype/`) into the real SvelteKit app.

---

## The Three Steps

```
1. Markup      →  Write the raw HTML structure in +page.svelte
2. Components  →  Extract repeated or self-contained blocks into $lib/components/
3. Styling     →  Add CSS via <style> blocks or app.css
```

Each step is independently verifiable: structure is readable in the browser without styles, components can be checked for correct prop flow, and styles are added last without touching logic.

---

## Step 1 — Write raw markup in +page.svelte

Translate the wireframe into semantic HTML. No `class=` attributes. Use `{#each}` for repeated items, keeping data in the `<script>` block.

```svelte
<!-- src/routes/+page.svelte -->
<script>
  const steps = [
    { n: 1, label: 'Fill in your details' },
    { n: 2, label: 'Generate & share link' },
  ];
</script>

<div>
  <h1>SAKE</h1>
  <p>Service Account Key Exchange Helper</p>

  <div>
    <p>How this works</p>
    <ol>
      {#each steps as step}
        <li><span>{step.n}</span> {step.label}</li>
      {/each}
    </ol>
  </div>

  <p>Do not close this tab. Your private key only lives in memory.</p>

  <div>
    <a href="/request">I need a Service Account Key</a>
    <a href="/upload">I received a link from a requester</a>
  </div>
</div>
```

At this stage the page renders as unstyled HTML. It is already navigable and the data flow is visible.

---

## Step 2 — Extract reusable components

When a block of markup is repeated or self-contained, move it to `src/lib/components/`. Pass varying content as props using `export let`.

```svelte
<!-- src/lib/components/RoleCard.svelte -->
<script>
  export let href;
  export let title;
  export let subtitle;
  export let cta;
</script>

<a {href}>
  <p>{title}</p>
  <p>{subtitle}</p>
  <span>{cta}</span>
</a>
```

```svelte
<!-- Back in +page.svelte -->
<script>
  import RoleCard from '$lib/components/RoleCard.svelte';
</script>

<RoleCard href="/request" title="I need a Service Account Key" subtitle="Start the request process" cta="Request" />
<RoleCard href="/upload"  title="I received a link from a requester" subtitle="GCP access required" cta="Open Upload" />
```

`export let` is the Svelte 4 way to declare props. In Svelte 5 this is `let { href, title } = $props()` — see [[Svelte Props and Events]].

---

## Step 3 — Add styling

Two options, used together:

**Global styles** in `src/app.css`, imported once in `+layout.svelte`:
```css
/* app.css */
.role-card { border: 2px solid #e2e8f0; border-radius: 1rem; padding: 1.25rem; }
.role-card:hover { border-color: #60a5fa; }
```

**Scoped styles** in each component's `<style>` block:
```svelte
<!-- RoleCard.svelte -->
<a {href} class="card">...</a>

<style>
  .card { border: 2px solid #e2e8f0; border-radius: 1rem; padding: 1.25rem; }
  .card:hover { border-color: #60a5fa; }
</style>
```

See [[CSS Styling in Svelte]] for a full comparison of all styling approaches.

---

## Wireframe to SvelteKit mapping

| HTML wireframe | SvelteKit equivalent |
|---|---|
| `<a href="request.html">` | `<a href="/request">` |
| Repeated card HTML | `{#each}` loop or `<RoleCard>` component |
| Inline `<script>` at bottom | `<script>` block at top of `.svelte` file |
| Single `.html` file | `src/routes/+page.svelte` |
| Shared nav/footer | `src/routes/+layout.svelte` |
| Utility classes (Tailwind) | Same — copy over directly |

---

## Related Topics

- [[SvelteKit]] — framework this workflow is used in
- [[SvelteKit Routing]] — how `+page.svelte` and `+layout.svelte` map to URLs
- [[Svelte Components]] — structure of a `.svelte` file
- [[Svelte Props and Events]] — `export let` and `$props()` for passing data into components
- [[CSS Styling in Svelte]] — all styling approaches: scoped block, global file, Tailwind
- [[Wireframes]] — the design artefacts this workflow starts from
- [[Separation of concerns]] — the principle behind splitting markup, components, and styles
