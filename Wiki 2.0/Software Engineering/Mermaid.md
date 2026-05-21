# Mermaid

[Mermaid Docs](https://mermaid.js.org/intro/) | [Live Editor](https://mermaid.live) | [Obsidian Mermaid Support](https://help.obsidian.md/Editing+and+formatting/Advanced+formatting+syntax#Diagram)

Mermaid is a JavaScript-based diagramming tool that renders diagrams from plain text using a Markdown-like syntax. It is natively supported in Obsidian and on GitHub.

---

## Diagram types

| Type | Keyword | Use case |
|---|---|---|
| Flowchart | `flowchart LR` / `TD` | Logic flows, decision trees |
| Sequence diagram | `sequenceDiagram` | Message flows between participants |
| Class diagram | `classDiagram` | OOP structure |
| State diagram | `stateDiagram-v2` | State machines |
| Entity relationship | `erDiagram` | Database schemas |
| Gantt chart | `gantt` | Project timelines |

---

## Sequence diagram

```
sequenceDiagram
    participant A as Client
    participant B as Server
    A->>B: HTTP request
    B-->>A: HTTP response
```

Arrow types:
| Syntax | Meaning |
|---|---|
| `->>` | Solid arrow (synchronous call) |
| `-->>` | Dashed arrow (response / async) |
| `-x` | Solid arrow with X (failed / terminated) |

Grouping:
- `loop <label> ... end` — repeated calls
- `alt <cond> / else / end` — conditional branches
- `par ... and ... end` — parallel flows

---

## Flowchart

```
flowchart TD
    A[Start] --> B{Condition?}
    B -- yes --> C[Do this]
    B -- no --> D[Do that]
    C --> E[End]
    D --> E
```

Node shapes: `[rectangle]`, `(rounded)`, `{diamond}`, `((circle))`

---

## Special characters in labels

Angle brackets (`<`, `>`) and HTML tags in labels cause parsing errors because Mermaid renders into HTML and the parser confuses them with HTML tags.

**Broken:**
```
participant Script as <script>
```

**Fix 1 — Mermaid entity syntax** (works in all renderers):
```
participant Script as "#lt;script#gt;"
```

**Fix 2 — Drop the alias** (simplest when the participant name is already clear):
```
participant Script
```

Avoid `&lt;` / `&gt;` HTML entities directly in Mermaid source — they are not reliably decoded by the Mermaid parser before rendering.

---

## Usage in Obsidian

Wrap diagrams in a fenced code block with the `mermaid` language tag:

````
```mermaid
sequenceDiagram
    ...
```
````

Obsidian renders Mermaid diagrams in preview mode. The Live Editor at [mermaid.live](https://mermaid.live) is useful for debugging syntax errors before pasting into a note.

---

## Related Topics

- [[Design Patterns]] — Mermaid class and sequence diagrams are useful for documenting patterns
- [[Software Engineering]] — overview note for this section
