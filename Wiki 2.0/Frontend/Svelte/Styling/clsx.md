# clsx

[npm — clsx](https://www.npmjs.com/package/clsx) | [GitHub — clsx](https://github.com/lukeed/clsx)

`clsx` is a tiny utility library for **conditionally building CSS class strings**. It filters out falsy values (`false`, `null`, `undefined`) automatically.

---

## Installation

```bash
npm install clsx
```

---

## The problem it solves

Without clsx, conditional classes get messy:

```svelte
<div class="{isActive ? 'active' : ''} {hasError ? 'error' : ''} card">
```

---

## Usage

```js
import clsx from 'clsx';

clsx('card', isActive && 'active', hasError && 'error');
// isActive=true,  hasError=false  → "card active"
// isActive=false, hasError=true   → "card error"
// both true                       → "card active error"
// both false                      → "card"
```

### Supported input forms

```js
// strings
clsx('foo', 'bar')                       // "foo bar"

// conditionals
clsx('foo', isActive && 'bar')           // "foo" or "foo bar"

// objects — key = class name, value = condition
clsx({ active: isActive, error: hasError })

// arrays (nested, mixed)
clsx(['foo', 'bar'])
clsx('base', { active: isActive }, extra && 'extra')
```

---

## In Svelte

```svelte
<script>
    import clsx from 'clsx';

    let isActive = $state(false);
    let hasError = $state(true);
</script>

<div class={clsx('card', { active: isActive, error: hasError })}>
    content
</div>
```

---

## Related

- [[Svelte Bind Directive]] — bind:class for dynamic class toggling without a library
- [[Svelte Components]] — component structure where clsx is commonly used
- [[Tailwind CSS]] — clsx is often paired with Tailwind for conditional utility classes
