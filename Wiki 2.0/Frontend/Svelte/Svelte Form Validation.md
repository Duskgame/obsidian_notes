# Svelte Form Validation

[MDN — input type=email](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/email) | [MDN — Constraint Validation](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation)

---

## Option 1 — HTML built-in (no JavaScript)

```svelte
<input type="email" required />
<input type="text" minlength="3" required />
```

The browser validates on submit and shows its own error tooltip. No code needed — good for simple cases.

---

## Option 2 — `$derived` boolean

```svelte
<script>
    let email = $state('');

    let isValidEmail = $derived(
        /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
    );
</script>

<input type="email" bind:value={email} />

{#if email && !isValidEmail}
    <p>Please enter a valid email address.</p>
{/if}

<button disabled={!isValidEmail}>Submit</button>
```

`$derived` recalculates whenever `email` changes — no manual event handling needed.

---

## Email regex explained

```
/^[^\s@]+@[^\s@]+\.[^\s@]+$/

^           start of string
[^\s@]+     one or more chars that are NOT a space or @
@           literal @
[^\s@]+     domain name
\.          literal dot
[^\s@]+     extension (com, de, …)
$           end of string
```

Catches obvious mistakes (`noemail`, `missing@dot`, `@nodomain.com`) without being overly strict.

---

## Only validate after the user leaves the field

Showing an error immediately on load is bad UX. Use a `touched` flag set on `onblur`:

```svelte
<script>
    let email = $state('');
    let touched = $state(false);

    let isValidEmail = $derived(
        /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
    );
</script>

<input
    type="email"
    bind:value={email}
    onblur={() => touched = true}
/>

{#if touched && !isValidEmail}
    <p>Please enter a valid email address.</p>
{/if}
```

`onblur` fires when the input loses focus — the right moment to start showing errors.

---

## Multiple fields — combined canSubmit

```svelte
<script>
    let email = $state('');
    let ticket = $state('');

    let isValidEmail = $derived(
        /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
    );

    let canSubmit = $derived(isValidEmail && ticket.length > 0);
</script>

<input type="text" bind:value={ticket} placeholder="JIRA-123" />
<input type="email" bind:value={email} placeholder="sa@project.iam.gserviceaccount.com" />

{#if canSubmit}
    <button type="submit">Submit</button>
{/if}
```

`canSubmit` is a single derived boolean that gates the submit button — easy to extend with more fields.

---

## Related

- [[Svelte Bind Directive]] — bind:value for two-way input binding
- [[Svelte Reactivity (Runes)]] — $derived and $state
- [[HTML Forms]] — how form submission works
