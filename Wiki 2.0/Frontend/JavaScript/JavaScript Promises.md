# JavaScript Promises & async/await

[MDN — Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) | [MDN — async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

A **Promise** is an object representing a value that isn't available yet — it will arrive in the future or fail. Promises are the foundation of all async work in JavaScript (fetch, timers, file I/O).

---

## Three states

```
pending   →  still waiting for the result
fulfilled →  data arrived successfully
rejected  →  something went wrong
```

---

## async / await

The modern, readable way to work with Promises. `await` pauses the current function until the promise settles — without blocking the rest of the page.

```js
async function loadUser() {
    const response = await fetch("https://api.example.com/user");
    const data = await response.json();  // response.json() is also a Promise
    return data;
}
```

- `async` marks a function as asynchronous — it always returns a Promise
- `await` can only be used inside an `async` function
- Code after `await` only runs once the promise resolves

---

## Error handling

```js
async function loadUser() {
    try {
        const response = await fetch("https://api.example.com/user");
        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        return await response.json();
    } catch (error) {
        console.error("Failed:", error.message);
    }
}
```

---

## Promise methods

```js
// Run multiple promises in parallel, wait for all
const [users, posts] = await Promise.all([fetchUsers(), fetchPosts()]);

// Return whichever promise resolves first
const fastest = await Promise.race([fetch(url1), fetch(url2)]);
```

---

## In Svelte — {#await}

Svelte has a built-in template block that maps to the three promise states:

```svelte
<script>
    async function getUser() {
        const res = await fetch("/api/user");
        return res.json();
    }

    let promise = getUser();
</script>

{#await promise}
    <p>Loading...</p>
{:then user}
    <p>Hello {user.name}</p>
{:catch error}
    <p>Error: {error.message}</p>
{/await}
```

Note: in the Svelte tutorial sandbox, `$state(roll())` re-runs every time you save a file due to **Hot Module Replacement (HMR)** — this is normal dev-environment behaviour, not a bug in your code.

---

## Related

- [[Svelte Template Logic]] — `{#await}` block
- [[Fetch in Svelte]] — using fetch() with async/await in Svelte
- [[DOM]] — events and async browser APIs
