# JIT — Just-In-Time Compilation

JIT is a compilation strategy where code is compiled to machine code **during execution**, not before. It is used by modern JavaScript engines (V8 in Chrome/Node, SpiderMonkey in Firefox) to make JS fast.

## The spectrum

```
Pure interpretation          JIT                   AOT (Ahead-of-Time)
       |---------------------|----------------------|
  line by line            during run            before run
  (slow, no optimization) (adaptive, fast)      (fastest, needs build step)
```

## How JIT works

```
JS code arrives in browser
    ↓
Starts executing (interpreted)
    ↓
Engine monitors which code runs frequently ("hot code")
    ↓
Compiles hot code to optimized machine code on the fly
    ↓
Next calls use the fast compiled version
```

## Example

```js
function add(a, b) {
    return a + b;
}

for (let i = 0; i < 10000; i++) {
    add(i, i); // called constantly with numbers
}
```

The JIT compiler notices `add` is always called with numbers and compiles a number-optimized version. If you later call `add("hello", "world")`, the engine **deoptimizes** (throws away the compiled version and falls back to interpretation).

## JIT vs AOT

| | JIT | AOT |
|---|---|---|
| When compiled | While running | Before running (build step) |
| Who triggers it | Browser engine, automatically | Developer (`npm run build`) |
| Examples | JS in V8, Java HotSpot | Svelte, TypeScript, C |
| Tradeoff | Warm-up time, adaptive | Fixed output, no adaptation |

## Practical takeaway

You never write code differently because of JIT — it is completely invisible to you. It is the reason JavaScript can be fast despite being a dynamic, loosely typed language. The engine handles all optimization automatically.

## Sources

- [MDN — Just-in-time compilation](https://developer.mozilla.org/en-US/docs/Glossary/Just_In_Time_Compilation)
- [V8 blog — How V8 works](https://v8.dev/blog)

## Related

- [[Compiler]]
- [[Compile Time vs Runtime]]
