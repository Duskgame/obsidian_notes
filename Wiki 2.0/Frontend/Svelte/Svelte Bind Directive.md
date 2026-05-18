# Svelte bind: Directive

[Svelte Docs — bind:](https://svelte.dev/docs/svelte/bind) | [Tutorial: Bindings](https://learn.svelte.dev/tutorial/text-inputs)

The `bind:` directive creates **two-way data binding** between a variable and a DOM element. Without it, data flows only one way (variable → DOM). With it, both stay in sync automatically.

---

## Basic usage

```svelte
<script>
    let name = $state('');
</script>

<input bind:value={name} />
<p>Hello {name}</p>
```

- User types → `name` updates
- `name` changes in code → input updates

---

## What it compiles to

`bind:value={name}` is shorthand for:

```svelte
<input
    value={name}
    oninput={(e) => name = e.target.value}
/>
```

Svelte generates the event handler automatically.

---

## Common bindings by input type

```svelte
<!-- text input -->
<input type="text" bind:value={text} />

<!-- number — returns a number, not a string -->
<input type="number" bind:value={count} />

<!-- range slider -->
<input type="range" bind:value={volume} />

<!-- checkbox — binds to boolean -->
<input type="checkbox" bind:checked={isChecked} />

<!-- select dropdown -->
<select bind:value={selected}>
    <option value="a">Option A</option>
    <option value="b">Option B</option>
</select>

<!-- textarea -->
<textarea bind:value={text} />
```

---

## bind:group — radio buttons and checkboxes

`bind:group` links multiple inputs to one shared variable.

### Radio buttons — one value selected

```svelte
<script>
    let picked = $state('apple');
    let options = ['apple', 'banana', 'carrot'];
</script>

{#each options as option}
    <label>
        <input type="radio" bind:group={picked} value={option} />
        {option}
    </label>
{/each}

<p>Picked: {picked}</p>
```

Selecting one radio automatically deselects the others. The selected `value` is written to `picked`.

### Checkboxes — array of selected values

```svelte
<script>
    let selected = $state([]);
</script>

<input type="checkbox" bind:group={selected} value="apple" />
<input type="checkbox" bind:group={selected} value="banana" />
<input type="checkbox" bind:group={selected} value="carrot" />

<p>Selected: {selected}</p>
<!-- e.g. ["apple", "carrot"] if two are checked -->
```

---

## Binding to component props — $bindable()

To allow a parent to `bind:` to a child component's prop, the child must declare it with `$bindable()`:

```svelte
<!-- Child.svelte -->
<script>
    let { value = $bindable('') } = $props();
</script>

<input bind:value={value} />
```

```svelte
<!-- Parent.svelte -->
<script>
    let query = $state('');
</script>

<Child bind:value={query} />
<p>Query: {query}</p>
```

Without `$bindable()`, Svelte will throw an error if the parent tries to bind to the prop.

---

## Related

- [[Svelte Props and Events]] — $bindable() and props
- [[Svelte Reactivity (Runes)]] — $state that powers reactive bindings
- [[JavaScript Destructuring]] — how $props() destructuring works
