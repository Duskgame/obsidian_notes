# HTML Forms & Submission

[MDN — form element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form) | [MDN — submit event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event)

---

## Default browser behaviour

Without JavaScript, clicking a submit button causes the browser to collect all named inputs and send an HTTP request, then navigate to the response (full page reload):

```html
<form action="/submit" method="POST">
    <input name="email" value="jonas@example.com" />
    <input name="ticket" value="JIRA-123" />
    <button type="submit">Send</button>
</form>
```

Sent as: `email=jonas%40example.com&ticket=JIRA-123`

---

## With JavaScript — intercept and prevent reload

```svelte
<form onsubmit={(e) => {
    e.preventDefault();   <!-- stops page reload -->
    handleSubmit();
}}>
    <input bind:value={email} />
    <button type="submit">Send</button>
</form>
```

`e.preventDefault()` cancels the browser's default navigation. You then handle the data yourself with `fetch`.

---

## What triggers submit

- Clicking a `type="submit"` button
- Pressing **Enter** inside any text input (browser default)

Both fire the same `onsubmit` event on the `<form>` element.

---

## Button types

```html
<!-- triggers form submit (default if type omitted) -->
<button type="submit">Send</button>

<!-- does nothing to the form — only runs onclick -->
<button type="button" onclick={cancel}>Cancel</button>
```

A `<button>` inside a form **defaults to `type="submit"`** if no type is specified. This causes accidental submits when clicking non-submit buttons.

---

## Sending data with fetch

```js
async function handleSubmit() {
    const res = await fetch('/api/submit', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, ticket }),
    });
    const result = await res.json();
}
```

In Svelte, `bind:value` keeps variables in sync with inputs, so you read variables directly — no need to touch `FormData` or `e.target`.

---

## The full flow

```
user clicks Submit (or presses Enter)
        ↓
onsubmit fires on <form>
        ↓
e.preventDefault()    ← stops page reload
        ↓
read $state variables
        ↓
fetch POST to API
        ↓
handle response
```

---

## Related

- [[Fetch in Svelte]] — sending data with fetch after form submit
- [[Svelte Bind Directive]] — bind:value for two-way input binding
- [[Svelte Form Validation]] — validating inputs before submit
- [[Event Bubbling and Capturing]] — how submit event propagates
