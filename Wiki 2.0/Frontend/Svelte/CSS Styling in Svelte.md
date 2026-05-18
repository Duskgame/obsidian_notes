# CSS Styling in Svelte

[Svelte Docs — Scoped styles](https://svelte.dev/docs/svelte/scoped-styles) | [Svelte Docs — style:](https://svelte.dev/docs/svelte/style)

Svelte offers several approaches to styling. They can be mixed freely.

---

## 1. Scoped `<style>` block (default)

```svelte
<p class="title">Hello</p>

<style>
    .title {
        color: blue;
        font-size: 1.5rem;
    }
</style>
```

Styles are **automatically scoped** to the component — `.title` here won't affect `.title` in any other component. Svelte adds a unique attribute to the elements at compile time.

---

## 2. Inline `style=`

```svelte
<p style="color: red; font-size: 1rem;">Hello</p>

<!-- dynamic -->
<p style="color: {isError ? 'red' : 'black'};">Hello</p>
```

Good for one-off dynamic values. Gets messy for more than 1–2 properties.

---

## 3. `style:` directive

```svelte
<p
    style:color={isError ? 'red' : 'black'}
    style:font-weight={isImportant ? 'bold' : 'normal'}
>
    Hello
</p>
```

Svelte shorthand for applying individual style properties reactively. Cleaner than string interpolation.

---

## 4. `class:` directive — toggling classes

```svelte
<button
    class="btn"
    class:active={isActive}
    class:disabled={!canSubmit}
>
    Submit
</button>
```

Adds or removes a class based on a boolean condition. Cleaner than ternary strings in `class=`.

---

## 5. Tailwind CSS (utility classes)

```svelte
<button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
    Submit
</button>
```

No separate CSS — utility classes describe the style inline. Use `clsx` for conditional classes:

```svelte
<script>
    import clsx from 'clsx';
    let isError = $state(false);
</script>

<p class={clsx('text-sm', { 'text-red-500': isError, 'text-gray-700': !isError })}>
    Message
</p>
```

---

## 6. Global CSS file

In SvelteKit, import from `+layout.svelte`:

```svelte
<!-- +layout.svelte -->
<script>
    import '../app.css';
</script>
```

```css
/* app.css */
body {
    margin: 0;
    font-family: sans-serif;
}
```

For reset/base styles that apply everywhere.

---

## 7. `:global()` — escape scoping

```svelte
<style>
    /* scoped — only this component */
    .card { padding: 1rem; }

    /* global — affects matching elements anywhere */
    :global(body) { margin: 0; }

    /* targets children rendered by {#each} or child components */
    .card :global(p) { color: gray; }
</style>
```

---

## Comparison

| Approach | Scoped | Dynamic | Setup needed |
|---|---|---|---|
| `<style>` block | yes | no | none |
| Inline `style=` | no | yes | none |
| `style:` directive | no | yes | none |
| `class:` directive | yes | yes | none |
| Tailwind | no | with clsx | install Tailwind |
| Global CSS file | no | no | import once |

---

## Related

- [[Tailwind CSS]] — utility-first CSS framework
- [[clsx]] — conditional class string builder
- [[Svelte Components]] — component structure
- [[Svelte Bind Directive]] — bind:class for dynamic class toggling
