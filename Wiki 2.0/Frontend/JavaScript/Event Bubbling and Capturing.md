# Event Bubbling and Capturing

[MDN — Event bubbling](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Event_bubbling) | [MDN — addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)

When a DOM event fires, it doesn't just affect the target element — it travels through the DOM tree in two phases.

---

## Two phases

```
CAPTURE (top-down)       BUBBLE (bottom-up)
        ↓                        ↑
    document                 document
        ↓                        ↑
      div                      div
        ↓                        ↑
      button  ←── event fires ───
```

1. **Capture phase** — event travels from `document` down to the target
2. **Bubble phase** — event travels from the target back up to `document`

Most handlers run in the **bubble phase** by default.

---

## Event bubbling

```html
<div onclick="console.log('div')">
    <p onclick="console.log('p')">
        <button onclick="console.log('button')">Click me</button>
    </p>
</div>
```

Clicking the button logs:
```
button   ← target fires first
p        ← bubbles up
div      ← bubbles further
```

### Practical use — event delegation

One parent handler can catch events from all children:

```svelte
<ul onclick={(e) => console.log(e.target)}>
    <li>apple</li>
    <li>banana</li>
    <li>carrot</li>
</ul>
```

`e.target` tells you which specific `<li>` was clicked.

### Stopping bubbling

```svelte
<div onclick={() => closeModal()}>
    <div onclick={(e) => {
        e.stopPropagation();  <!-- prevents closeModal() from firing -->
        doSomething();
    }}>
        content
    </div>
</div>
```

---

## Capture phase handlers

Capture handlers fire **before** the event reaches its target — top-down instead of bottom-up.

In plain JS:
```js
element.addEventListener('click', handler, { capture: true });
```

In Svelte, append `capture` to the event name:
```svelte
<div onclickcapture={() => console.log('div — capture')}>
    <button onclick={() => console.log('button')}>
        click
    </button>
</div>
```

Output:
```
div — capture   ← capture fires first (top-down)
button          ← target fires second
```

### When to use capture

Rarely needed. Use cases:
- Intercepting an event before it reaches the target
- When a child uses `stopPropagation()` — that only stops bubbling, not capturing, so a parent capture handler still fires

---

## Event object reference

```js
element.addEventListener('click', (e) => {
    e.target           // element that was originally clicked
    e.currentTarget    // element this handler is attached to
    e.stopPropagation() // stop bubbling up
    e.preventDefault()  // prevent default browser behaviour (form submit, link nav)
});
```

---

## Related

- [[DOM]] — the DOM tree that events travel through
- [[Svelte Props and Events]] — how events work in Svelte 5
