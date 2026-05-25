# Wireframes

[Nielsen Norman Group – Wireframing](https://www.nngroup.com/articles/wireframing-study-guide/) | [Figma – What are wireframes?](https://www.figma.com/resource-library/what-is-a-wireframe/)

A wireframe is a simple skeletal sketch of a UI that shows layout and structure without real design (no colours, final fonts, or polished visuals). The goal is to plan what goes where and how the user flows through an app before any code or visual design is written.

---

## Why wireframe first

- **Cheap to change** — moving a box in ASCII or Excalidraw takes seconds; moving a built component takes much longer
- **Forces flow thinking** — you think through every user action before getting distracted by styling
- **Easy to discuss** — a sketch can be reviewed with a team or supervisor before committing to anything
- **Exposes missing states** — empty states, error states, loading states are often forgotten until you draw the whole flow

---

## Levels of fidelity

| Level | What it looks like | Typical tool |
|---|---|---|
| **Low-fi** | ASCII boxes, pencil sketch, hand-drawn shapes | Paper, Excalidraw, plain text |
| **Mid-fi** | Grayscale blocks, real proportions, placeholder text | Figma (rough), Whimsical |
| **High-fi** | Looks like the final product, real colors and fonts | Figma with a design system |

For internal tools and early planning, **low-fi is almost always enough** — it communicates the idea without wasting time on visuals that will change anyway.

---

## What a wireframe shows

- Page layout and element placement
- Navigation structure and user flow between screens
- Which elements are interactive (buttons, inputs, links)
- Content hierarchy (what is prominent vs secondary)
- Step sequences (wizards, multi-step forms)

## What a wireframe does NOT show

- Final colours, fonts, spacing
- Real content (use placeholder text like "SA Mail:" with a box)
- Animations or transitions
- Implementation details

---

## ASCII wireframe example

```
┌─────────────────────────────┐
│  ← Back      Page Title     │
│  ─────────────────────────  │
│  ① Step 1  ──  ② Step 2     │  ← step indicator
│  ─────────────────────────  │
│                             │
│  Field label *              │
│  ┌───────────────────────┐  │
│  │ placeholder text      │  │
│  └───────────────────────┘  │
│                             │
│           [ Button → ]      │
└─────────────────────────────┘
```

---

## Tools

| Tool | Best for | Free? |
|---|---|---|
| **Excalidraw** | Quick low-fi sketches, hand-drawn feel | ✓ (built into Obsidian) |
| **Figma** | Mid-fi to high-fi, collaboration | Freemium |
| **Penpot** | Open source Figma alternative | ✓ |
| **Whimsical** | Clean flow diagrams and wireframes | Freemium |
| **draw.io** | Structured diagrams and flowcharts | ✓ |
| **Plain text / ASCII** | Ultra-fast, version-controllable | ✓ |

---

## Related Topics

- [[Design Patterns]] — wireframes visualise the UI patterns being applied
- [[Svelte Components]] — components map directly to the boxes in a wireframe
- [[SvelteKit Routing]] — each wireframe screen typically corresponds to a route
- [[SvelteKit Structure-First Workflow]] — how to translate a finished wireframe into SvelteKit markup and components step by step
