# Array.reduce

[MDN — Array.prototype.reduce()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

`reduce` iterates over an array and accumulates all elements into a single value. It is part of the JavaScript standard library and available in any JS/TS environment.

---

## Signature

```js
array.reduce((accumulator, currentElement) => {
    return newAccumulatorValue;
}, startingValue);
```

| Parameter | Meaning |
|---|---|
| `accumulator` | Carries the running result |
| `currentElement` | The current item being processed |
| return value | The new value of `accumulator` for the next step |
| `startingValue` | Initial value of `accumulator` before the first step |

---

## Step-by-step: sum of `[1, 2, 3, 4, 5]`

```js
[1, 2, 3, 4, 5].reduce((acc, n) => acc + n, 0);
```

| Step | acc (before) | n | acc + n |
|---|---|---|---|
| 1 | 0 | 1 | 1 |
| 2 | 1 | 2 | 3 |
| 3 | 3 | 3 | 6 |
| 4 | 6 | 4 | 10 |
| 5 | 10 | 5 | **15** |

Final result: `15`

---

## Common uses

```js
// Sum
[1, 2, 3].reduce((acc, n) => acc + n, 0)         // → 6

// Product
[1, 2, 3].reduce((acc, n) => acc * n, 1)          // → 6

// Max
[3, 1, 4, 1].reduce((a, b) => Math.max(a, b), 0) // → 4

// Flatten
[[1,2],[3,4]].reduce((acc, arr) => acc.concat(arr), []) // → [1,2,3,4]

// Count occurrences
['a','b','a'].reduce((acc, x) => {
    acc[x] = (acc[x] ?? 0) + 1;
    return acc;
}, {})  // → { a: 2, b: 1 }
```

---

## In Svelte with $derived

```svelte
<script>
  let numbers = $state([1, 2, 3, 4, 5]);
  const sum = $derived(numbers.reduce((acc, n) => acc + n, 0));
</script>

<p>Sum: {sum}</p>
```

`$derived.by` is preferred for multi-step logic:

```svelte
let total = $derived.by(() => {
    return items.reduce((sum, n) => sum + n, 0);
});
```

---

## Related

- [[Svelte Reactivity (Runes)]] — `$derived` and `$derived.by` use reduce frequently
