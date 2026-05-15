# Compiler

A compiler is a program that reads code written in one language and translates it into another language. The output is functionally equivalent — it does the same thing — just in a form a different environment can understand.

## General flow

```
Source code  →  [Compiler]  →  Output code
```

## Examples

| Compiler / Tool | Input | Output |
|---|---|---|
| Svelte compiler | `.svelte` files with runes | Plain JavaScript |
| TypeScript compiler (`tsc`) | `.ts` files with types | Plain JavaScript |
| Babel | Modern JS (`?.`, `??`) | Older JS for legacy browsers |
| C compiler (`gcc`) | C code | Machine code (binary) |

## How a compiler works internally (simplified)

```
Source code
    ↓
[Lexer]       — splits text into tokens ("let", "count", "=", "0")
    ↓
[Parser]      — builds a tree structure from the tokens (AST)
    ↓
[Transformer] — modifies or analyzes the tree
    ↓
[Generator]   — writes the output code from the tree
    ↓
Output code
```

The intermediate tree structure is called an **AST** (Abstract Syntax Tree).

## Compiler vs interpreter

| | Compiler | Interpreter |
|---|---|---|
| What it does | Translates all code first, then runs | Reads and runs code line by line |
| When errors appear | Before running (compile time) | While running (runtime) |
| Examples | Svelte, TypeScript, C | Python, older JavaScript |

Modern JavaScript engines use **both**: they compile JS to machine code just before running it — see [[JIT]].

## Why compilers enable better developer experience

Languages like TypeScript and Svelte can offer syntax that browsers don't natively understand. The compiler bridges the gap — letting you write ergonomic, type-safe, or declarative code while still producing standard output the browser can run.

## Sources

- [MDN — JavaScript engine](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript_engine)
- [Svelte Docs — Overview](https://svelte.dev/docs/svelte/overview)

## Related

- [[Compile Time vs Runtime]]
- [[JIT]]
- [[Svelte Reactivity (Runes)]] — runes are a compile-time feature
- [[Svelte Global State]] — `.svelte.js` files processed by the Svelte compiler
