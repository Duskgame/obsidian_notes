# DOM — Document Object Model

The DOM is a programming interface that represents an HTML (or XML) document as a live, in-memory tree of objects. The browser builds it when it parses an HTML page, and JavaScript can read and modify it to update what the user sees — without reloading the page.

## Tree structure

Given this HTML:

```html
<body>
  <h1>Hello</h1>
  <p>World</p>
</body>
```

The browser constructs:

```
document
└── body
    ├── h1  →  "Hello"
    └── p   →  "World"
```

Each node is a JavaScript object with properties and methods.

## Common operations

```js
// Select elements
document.querySelector('h1')
document.getElementById('main')
document.querySelectorAll('.item')

// Read / write content
element.textContent = 'New text'
element.innerHTML = '<span>HTML</span>'

// Create & insert
const div = document.createElement('div')
parent.appendChild(div)

// Remove
element.remove()

// Events
element.addEventListener('click', handler)
```

## Key concepts

| Term | Meaning |
|---|---|
| Node | Any point in the tree (element, text, comment, …) |
| Element | An HTML tag node (`<div>`, `<p>`, …) |
| Document | The root node; entry point via `document` |
| Event | User or browser action (click, keydown, load) |

## DOM vs HTML file

- The HTML file is static source text on disk.
- The DOM is a live object graph in memory — it can differ from the original HTML if JavaScript has modified it.

## Virtual DOM

Frameworks like React maintain a **virtual DOM** — a lightweight in-memory copy of the real DOM. They diff the virtual tree and apply only minimal real DOM changes, which is faster than rebuilding the whole tree.

[[Svelte]] avoids a virtual DOM entirely: it compiles templates to direct, surgical DOM updates at build time.

## Sources

- [MDN — Introduction to the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [MDN — Document](https://developer.mozilla.org/en-US/docs/Web/API/Document)
- [MDN — EventTarget.addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

## Related

- [[Frontend]]
- [[Svelte]]
