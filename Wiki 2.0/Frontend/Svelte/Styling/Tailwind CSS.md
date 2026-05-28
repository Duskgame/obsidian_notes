# Tailwind CSS

[Tailwind Docs](https://tailwindcss.com/docs) | [Tailwind with SvelteKit](https://tailwindcss.com/docs/installation/framework-guides/sveltekit)

**Tailwind CSS** is a utility-first CSS framework. Instead of writing CSS classes with custom names, predefined utility classes are used directly in the HTML. No separate CSS file needed — at least for the majority of styles.

> **SAKE uses Tailwind** — all visual classes in the Svelte markup are Tailwind classes.

---

## The concept

**Classic CSS:**
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

No CSS to write — everything in the markup.

---

## Installation with SvelteKit

```bash
npx sv create my-app
# → select Tailwind during setup
```

Or add afterwards:
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

## Key utility categories

### Spacing
```html
<div class="p-4">      <!-- padding: 1rem -->
<div class="px-4">     <!-- padding-left + right: 1rem -->
<div class="py-2">     <!-- padding-top + bottom: 0.5rem -->
<div class="m-4">      <!-- margin: 1rem -->
<div class="mt-2 mb-4"><!-- margin-top + bottom -->
<div class="gap-4">    <!-- gap in flexbox/grid: 1rem -->
```

Tailwind units: number × 4px (p-4 = 16px, p-1 = 4px, p-0.5 = 2px)

### Sizing
```html
<div class="w-full">     <!-- width: 100% -->
<div class="w-64">       <!-- width: 16rem -->
<div class="h-screen">   <!-- height: 100vh -->
<div class="max-w-md">   <!-- max-width: 28rem -->
```

### Colors
```html
<div class="bg-blue-500">     <!-- background blue -->
<p class="text-gray-700">     <!-- text color gray -->
<div class="border-red-500">  <!-- border red -->
```

Color scale: 50 (light) → 950 (dark)

### Typography
```html
<h1 class="text-2xl font-bold text-gray-900">Title</h1>
<p class="text-sm text-gray-600 leading-relaxed">Text</p>
<code class="font-mono text-sm bg-gray-100 px-1 rounded">code</code>
```

### Flexbox
```html
<div class="flex items-center justify-between gap-4">
  <span>Left</span>
  <span>Right</span>
</div>

<div class="flex flex-col gap-2">
  <p>Top</p>
  <p>Bottom</p>
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

### Borders and radius
```html
<div class="border border-gray-200 rounded-lg">
<div class="rounded-full">    <!-- circle -->
<div class="rounded-md">      <!-- slightly rounded -->
```

### Hover, focus, active
```html
<button class="bg-blue-500 hover:bg-blue-600 active:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500">
  Button
</button>
```

### Group hover — propagate hover to children
Mark the parent with `group`, then use `group-hover:` on any child to react to the parent's hover state.

```html
<a class="group border hover:border-blue-400">
  <div class="bg-slate-100 group-hover:bg-blue-50">  <!-- reacts to parent hover -->
    <svg class="text-slate-500 group-hover:text-blue-600">...</svg>
  </div>
</a>
```

### Typography utilities
```html
<p class="leading-snug">   <!-- line-height: 1.375 (tighter) -->
<p class="leading-relaxed"> <!-- line-height: 1.625 (looser) -->

<h1 class="tracking-tight">   <!-- letter-spacing: -0.025em -->
<p class="tracking-widest">   <!-- letter-spacing: 0.1em — used for uppercase labels -->
```

### Flex utilities
```html
<span class="inline-flex items-center gap-2"> <!-- flex but stays inline, doesn't take full width -->
<div class="flex-shrink-0">  <!-- prevents this item from shrinking when space is tight -->
```

### Space between children
```html
<ul class="space-y-2">  <!-- adds margin-top: 0.5rem between each child -->
<ul class="space-x-4">  <!-- adds margin-left: 1rem between each child -->
```
Alternative to `gap` when you can't use flexbox/grid on the parent.

### Transitions
```html
<div class="transition-all">       <!-- transition all properties -->
<div class="transition-colors">    <!-- only color/background/border changes -->
<div class="transition-transform"> <!-- only transform changes (performant) -->
```
Pair with `duration-*` to control speed: `transition-colors duration-200`.

### Transform / translate
```html
<svg class="group-hover:translate-x-0.5"> <!-- nudge 2px right on parent hover -->
<div class="translate-y-1">              <!-- move 4px down -->
```
`translate-x-0.5` = 2px, `translate-x-1` = 4px — same scale as spacing.

---

## Responsive design

Tailwind is mobile-first. Breakpoint prefixes activate styles from a certain screen width:

```html
<div class="flex-col md:flex-row">
  <!-- below md: vertical, from md: horizontal -->
</div>

<!-- Breakpoints: sm(640px), md(768px), lg(1024px), xl(1280px), 2xl(1536px) -->
```

---

## Conditional classes in Svelte

```svelte
<script lang="ts">
  let isError = $state(false);
  let isLoading = $state(false);
</script>

<!-- Ternary -->
<div class={isError ? "bg-red-50 border-red-500" : "bg-white border-gray-200"}>

<!-- Class binding -->
<button
  class="px-4 py-2 rounded font-medium"
  class:bg-blue-500={!isLoading}
  class:bg-gray-400={isLoading}
  class:cursor-not-allowed={isLoading}
  disabled={isLoading}
>
  {isLoading ? "Loading..." : "Submit"}
</button>
```

### With clsx/cn (recommended for complex cases)

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

## Common component patterns

### Button
```html
<button class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700 disabled:opacity-50 disabled:cursor-not-allowed transition">
  Rotate key
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
  Active
</span>
```

---

## Extracting reusable classes with `@layer components`

When the same utility combination is used across multiple pages, extract it into a named class in `app.css` using `@layer components` and `@apply`:

```css
/* src/app.css */
@import "tailwindcss";

@layer components {
  .btn-primary {
    @apply flex w-full items-center justify-center gap-2 rounded-lg bg-blue-600 px-4 py-2.5 text-sm font-semibold text-white transition-colors hover:bg-blue-700 disabled:opacity-50;
  }

  .card-pad {
    @apply rounded-2xl border border-slate-200 bg-white shadow-sm p-6;
  }
}
```

These classes are then used like normal class names in markup:
```html
<button class="btn-primary">Submit</button>
<div class="card-pad">...</div>
```

### Limitation: cannot `@apply` another custom class

In Tailwind v4, `@apply` inside `@layer components` can **only** reference built-in utility classes — not other custom classes from the same layer:

```css
/* ✗ FAILS in Tailwind v4 — card is a custom class, not a utility */
.card-pad {
  @apply card p-6;
}

/* ✓ CORRECT — expand to full utilities */
.card-pad {
  @apply rounded-2xl border border-slate-200 bg-white shadow-sm p-6;
}
```

### When to extract vs. keep inline

Only extract to `@layer components` when the combination appears on **multiple pages**. For patterns used only within one component, use the Svelte `<style>` block. For one-off styles, keep them inline — extracting forces you to invent names for non-semantic patterns and breaks colocation.

---

## `@apply` in Svelte `<style>` blocks

Component-specific repeated patterns can be defined in the component's `<style>` block. However, Svelte's `<style>` is treated as a CSS module, so Tailwind utilities are not automatically in scope. You must add `@reference "tailwindcss"` at the top:

```svelte
<style>
  @reference "tailwindcss";

  .expiry-input {
    @apply w-24;
  }

  .hint {
    @apply mt-2 text-center text-xs text-slate-400;
  }
</style>
```

`@reference` makes utilities available for `@apply` **without emitting duplicate CSS** in the output. Without it, the build fails with "Cannot apply unknown utility class".

---

## Utility cascade override (v4)

In Tailwind v4, the CSS cascade layers are ordered:

```
@layer base  <  @layer components  <  @layer utilities
```

**Utilities always win over components**, regardless of the order of class names in the HTML attribute. This means a utility added directly in the markup will override any conflicting property set by a `@layer components` class:

```html
<!-- .btn-primary sets py-2.5 via @layer components -->
<!-- py-3 is from @layer utilities → wins, overrides py-2.5 -->
<button class="btn-primary py-3">Download</button>
```

No `!important`, no extra `<style>` block, no redefinition of `.btn-primary` needed. The utility wins purely because of layer priority.

### Tailwind v3 vs. v4

| | Tailwind v3 | Tailwind v4 |
|---|---|---|
| Components layer | `@layer components` | `@layer components` |
| Utilities layer | `@layer utilities` | `@layer utilities` |
| Override behavior | Both layers had same specificity — **last one in the stylesheet won**, not the class order in HTML | Utilities layer is explicitly higher priority — **utilities always win** |
| How to override a component | Redefine the class or use `!important` | Just add the utility to the `class` attribute |

### Practical consequence

You can define tight defaults in `@layer components` and override specific properties per-use without any extra CSS:

```css
/* app.css */
@layer components {
  .btn-primary {
    @apply px-4 py-2.5 bg-blue-600 text-white rounded-lg;
  }
}
```

```html
<!-- Taller padding on one specific button — works cleanly in v4 -->
<button class="btn-primary py-4">Big Button</button>

<!-- Different background on another — also works -->
<button class="btn-primary bg-green-600">Success</button>
```

This makes component classes safe to define without worrying about needing escape hatches.

---

## Related Topics

- [[Svelte Components]] — where Tailwind classes are used
- [[SvelteKit Static Adapter]] — Tailwind is compiled to minimal CSS at build time
- [[SVG Positioning and Spacing]] — Tailwind utility classes apply to inline SVG elements
- [[CSS Cascade and Specificity]] — the underlying CSS mechanism that makes layer ordering work
