# Memory Leak

A memory leak occurs when a program allocates memory or resources but never releases them. Over time, leaked resources accumulate and slow down or crash the application.

---

## What "memory" means in a browser

- RAM (JS objects that are never garbage collected)
- Running timers (`setInterval`, `setTimeout` that re-schedules itself)
- Event listeners attached to removed DOM elements
- Open network connections or subscriptions

---

## Classic example: timer without cleanup

```js
// Component mounts and is destroyed 5 times — WITHOUT cleanup

$effect(() => {
    setInterval(() => elapsed += 1, 1000); // new timer on every mount
    // no return → old timers are never stopped
});

// After 5 mount/destroy cycles: 5 timers running simultaneously
```

```js
// WITH cleanup — always exactly 1 timer
$effect(() => {
    const id = setInterval(() => elapsed += 1, 1000);
    return () => clearInterval(id); // cleanup runs before re-run and on destroy
});
```

---

## Why it gets worse over time

| Mounts/destroys | Without cleanup | With cleanup |
|---|---|---|
| 1 | 1 timer | 1 timer |
| 10 | 10 timers | 1 timer |
| 60 | 60 timers | 1 timer |

Each leaked timer uses CPU and memory. In long-running single-page apps where users never refresh, this compounds quickly.

---

## How to prevent leaks in Svelte

Always return a cleanup function from `$effect` when setting up side effects:

```js
$effect(() => {
    // setup
    const id = setInterval(fn, 1000);
    window.addEventListener('resize', handler);

    // teardown
    return () => {
        clearInterval(id);
        window.removeEventListener('resize', handler);
    };
});
```

Svelte calls the cleanup:
- Before every re-run of the effect
- When the component is destroyed (removed from the DOM)

---

## Related

- [[Svelte Reactivity (Runes)]] — `$effect` cleanup pattern
- [[Svelte Lifecycle]] — `onDestroy` as an alternative cleanup location
- [[DOM]] — components mount/destroy into the DOM
