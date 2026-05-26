# SVG Positioning and Spacing

[MDN SVG](https://developer.mozilla.org/en-US/docs/Web/SVG) | [MDN SVG transform](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform)

SVG uses a coordinate-based layout system — there is no box model, no flow, and no margin or padding. Every element is placed by explicit position or transform.

---

## No box model

Unlike [[DOM|HTML]], `margin` and `padding` have no effect on SVG elements. Position is always explicit.

```css
rect {
  margin: 10px;   /* ignored */
  padding: 10px;  /* ignored */
}
```

What does work on inline SVG elements in HTML:
- `fill`, `stroke`, `opacity`
- `transform` / `translate`
- `width`, `height`

---

## Positioning with coordinates

Every SVG element accepts `x` and `y` attributes. To create space between elements, calculate the offset manually.

```svg
<!-- 10px gap between two rects (first ends at x=60, second starts at x=70) -->
<rect x="10" y="10" width="50" height="50" />
<rect x="70" y="10" width="50" height="50" />
```

For circles, use `cx` and `cy` (center x/y):
```svg
<circle cx="30" cy="30" r="20" />
<circle cx="90" cy="30" r="20" />
```

---

## transform="translate()"

Use `translate` to move elements without changing their internal coordinates. Useful for groups.

```svg
<g transform="translate(20, 30)">
  <rect width="50" height="50" />
  <text x="5" y="65">Label</text>
</g>
```

Multiple transforms can be chained:
```svg
<g transform="translate(50, 50) rotate(45)">
  <rect width="30" height="30" />
</g>
```

---

## Text spacing: dx and dy

For `<text>` and `<tspan>`, use `dx` (horizontal offset) and `dy` (vertical offset) to shift relative to the previous character or element.

```svg
<text x="10" y="20">
  Hello
  <tspan dx="10">World</tspan>  <!-- 10px gap before "World" -->
</text>
```

`dy` is commonly used to move text down a line:
```svg
<text x="10" y="20">
  Line 1
  <tspan x="10" dy="20">Line 2</tspan>
</text>
```

---

## CSS transform on inline SVG

When SVG is embedded inline in HTML, CSS transforms work:

```css
.icon {
  transform: translateX(10px);
}
```

[[Tailwind CSS]] also works on inline SVG elements:
```html
<svg class="w-5 h-5 text-blue-600 group-hover:translate-x-0.5 transition-transform">
  ...
</svg>
```

Note: `text-*` color classes work because SVG uses `currentColor` for `fill`/`stroke`.

---

## foreignObject — embed HTML inside SVG

To use HTML layout (flexbox, margin, gap) inside an SVG, use `<foreignObject>`:

```svg
<foreignObject x="10" y="10" width="200" height="100">
  <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; gap: 8px;">
    <span>Item A</span>
    <span>Item B</span>
  </div>
</foreignObject>
```

Use sparingly — browser support and rendering edge cases make this fragile.

---

## Summary

| Goal | Approach |
|------|----------|
| Space between shapes | Calculate `x`/`y` manually |
| Move a group | `transform="translate(x, y)"` |
| Space between text segments | `dx` / `dy` on `<tspan>` |
| CSS layout inside SVG | `<foreignObject>` |
| Color via CSS | `fill: currentColor` + `color` / `text-*` class |
| margin / padding | Not supported — ignored |

---

## Related Topics

- [[DOM]] — SVG elements are part of the DOM when used inline in HTML
- [[Tailwind CSS]] — utility classes that apply to inline SVG elements
- [[CSS Styling in Svelte]] — CSS context where SVG is often embedded
- [[Frontend]] — parent topic area
