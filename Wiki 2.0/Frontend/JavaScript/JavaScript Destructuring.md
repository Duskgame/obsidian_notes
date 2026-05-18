# JavaScript Destructuring

[MDN — Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

Destructuring is shorthand syntax to unpack values from objects or arrays into individual variables in one line.

---

## Object destructuring

```js
const person = { name: "Jonas", age: 22, city: "Berlin" };

// Without destructuring
const name = person.name;
const age = person.age;

// With destructuring — same result
const { name, age } = person;
```

Variable names **must match the object keys**.

### Renaming while destructuring

```js
const { name: userName, age: userAge } = person;
// userName = "Jonas", userAge = 22
```

### Default values

```js
const { name, age = 25 } = person;
// if person.age is undefined, age falls back to 25
```

---

## Array destructuring

```js
const colors = ["red", "green", "blue"];

const [first, second] = colors;
// first = "red", second = "green"
```

Position matters here, not names. You can skip elements with a comma:

```js
const [, second, third] = colors;
// second = "green", third = "blue"
```

---

## Rest syntax (`...rest`)

```js
const { name, ...rest } = person;
// name = "Jonas"
// rest = { age: 22, city: "Berlin" }
```

`rest` collects everything not explicitly unpacked. Also works with arrays:

```js
const [first, ...others] = colors;
// first = "red", others = ["green", "blue"]
```

---

## Nested destructuring

```js
const user = { profile: { name: "Jonas", age: 22 } };

const { profile: { name, age } } = user;
```

---

## In function parameters

```js
function greet({ name, age = 25 }) {
    console.log(`${name} is ${age}`);
}

greet({ name: "Jonas" });
```

---

## Why it matters for Svelte props

```js
let { title, count = 0, ...rest } = $props();
```

This one line unpacks `title`, gives `count` a default, and collects the remainder into `rest` for forwarding — all using standard JavaScript destructuring syntax. See [[Svelte Props and Events]].

---

## Related

- [[Svelte Props and Events]] — $props() uses destructuring to declare component props
- [[JavaScript Promises]] — async/await also pairs well with destructuring
