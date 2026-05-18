# Compile Time vs Runtime

Code goes through two distinct phases: **compile time** (before it runs) and **runtime** (while it runs).

## The two phases

```
Source code  →  [Compiler]  →  Output code  →  [Browser / Engine]  →  Executes
              ↑ compile time                   ↑ runtime
```

| | Compile time | Runtime |
|---|---|---|
| When | During build (`npm run build`) | In the browser / Node.js |
| Who | Compiler / build tool | JS engine |
| Input | Your source code | Compiled output |
| Errors caught | Type errors, syntax errors, missing variables | Logic bugs, missing values, network errors |

## Compile time

The compiler reads your source and transforms it before any user ever runs it.

Examples of compile-time work:
- TypeScript checking `let age: number = "hello"` → error before running
- Svelte transforming `$state(0)` into real reactive JS
- Stripping `$inspect` calls from production output
- Bundlers (Vite, Webpack) combining files

```ts
// Caught at compile time — never reaches the browser
let age: number = "hello"; // ❌ Type 'string' is not assignable to type 'number'
```

## Runtime

The browser (or Node.js) executes the compiled output. Errors here only appear when the code actually runs.

```js
// Only fails at runtime — when this function is actually called
function getUser(id) {
    return users[id].name; // ❌ TypeError if users[id] is undefined
}
```

## Why the distinction matters

- **Compile-time errors** are caught early, before deployment, without needing a user to trigger them.
- **Runtime errors** can reach production — they require defensive coding, error boundaries, or tests.

Preferring compile-time guarantees (TypeScript types, Svelte runes) over runtime checks makes code safer and easier to debug.

## Svelte example

```svelte
<script>
  let count = $state(0); // $state is a compile-time construct
</script>
```

`$state` does not exist in JavaScript — the Svelte compiler replaces it at compile time. If you sent `$state(0)` directly to a browser (skipping compilation), you'd get a `ReferenceError` at runtime.

## Sources

- [MDN — Glossary: Compile time](https://developer.mozilla.org/en-US/docs/Glossary/Compile_time)
- [MDN — Glossary: Runtime](https://developer.mozilla.org/en-US/docs/Glossary/Runtime)

## Related

- [[Compiler]]
- [[JIT]] — compilation that happens at runtime
- [[Svelte Reactivity (Runes)]] — runes are compile-time constructs
