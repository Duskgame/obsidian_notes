Write a new wiki note on the topic the user specified. Proceed step by step.

## Step 1 — Check for an existing note

Search for an existing note on this topic:
- Run: `find "/home/jonas/Documents/obsidian_notes/Wiki 2.0" -iname "*<topic-keyword>*"`
- Also grep for the topic name in existing notes: `grep -rl "<topic>" "/home/jonas/Documents/obsidian_notes/Wiki 2.0" --include="*.md" -l | head -10`

If a note already exists:
- Read it and update it with new or missing information instead of creating a duplicate.
- Skip to Step 4 for backlinks.

## Step 2 — Choose the target directory

Determine the best directory under `/home/jonas/Documents/obsidian_notes/Wiki 2.0/` for the topic:

| Topic area | Directory |
|---|---|
| Frontend (JS, CSS, frameworks) | `Frontend/` |
| Svelte / SvelteKit | `Frontend/Svelte/` (or a subfolder) |
| Android, iOS, Kotlin, Ktor | `App development/` |
| Infrastructure, Docker, Cloud, Networking | `Infrastruktur/` |
| Software concepts, patterns, algorithms | `Software Engineering/` |
| School / Berufsschule topics | `School Wiki/` |

Then count the entries in the chosen directory:
```sh
ls "<target-directory>" | wc -l
```

- If count is **10 or fewer**: place the note directly in that directory.
- If count is **greater than 10**: look at the existing entries and the new topic. Create or choose a fitting subdirectory (e.g., `Core/`, `Data & State/`, `SvelteKit/`, `GoogleCloud/`) and place the note there. If the subdirectory does not exist yet, create it implicitly by writing the file to its path.

## Step 3 — Write the note

Create the file at the chosen path. The filename should match the topic title exactly (e.g., `Svelte Stores.md`).

### Required note format

```markdown
# <Topic Title>

[<Source Name>](<url>) | [<Second Source>](<url>)

<One to two sentences explaining what this topic is and why it matters.>

> **Relevance:** <Optional — only add if this relates to a specific project or context the user is working in.>

---

## <Section: main concept or first subtopic>

<Explanation. Use code blocks where relevant.>

---

## <Section: second subtopic>

<Explanation.>

---

## Summary

| ... | ... |
|---|---|

---

## Related Topics

- [[<Existing note title>]] — <one-line description of the relationship>
- [[<Another note>]] — ...
```

### Rules for the note content

- **Language:** English always.
- **Sources:** Include at least one link to an official, trustworthy source (official docs, MDN, RFC, spec). Do not use random blog posts as the primary source.
- **Wikilinks:** Any technical term or concept that has its own note in the vault must be linked as `[[Note Title]]`. Check for existing notes with:
  ```sh
  find "/home/jonas/Documents/obsidian_notes/Wiki 2.0" -name "*.md" | sed 's|.*/||; s|\.md$||' | sort
  ```
  Link generously — if a term is likely to have a note, link it. A `[[link]]` that points to a not-yet-existing note is fine; it becomes a placeholder for a future note.
- **Code examples:** Add at least one concrete code example if the topic is technical and code-related.
- **Diagrams:** Use a Mermaid diagram (`\`\`\`mermaid`) when illustrating a data flow, lifecycle, or relationship between components adds clarity. Do not force it on every note.
- **Tables:** Use a summary table at the end of long notes.
- **Related Topics section:** Always include this section at the bottom. List 2–5 related notes from the vault by name. If you're unsure whether a note exists, include the `[[link]]` anyway as a placeholder.
- **No opinions or padding:** Write factually. Do not add "In conclusion" or summarizing filler. The content should read like a reference, not an essay.

## Step 4 — Add backlinks to existing notes

After writing the new note, search for existing notes that mention the new topic but don't link to it yet:

```sh
grep -rl "<topic keyword>" "/home/jonas/Documents/obsidian_notes/Wiki 2.0" --include="*.md" -l
```

For each result:
1. Read the file.
2. Find where the topic keyword appears.
3. If the keyword is not already a `[[wikilink]]`, replace the first natural occurrence with `[[<New Note Title>]]`.
4. Do not modify heavily structured content (tables, code blocks) — only add the link in prose text.

Only update notes where the link is genuinely useful and natural. Do not force backlinks.

## Step 5 — Report

Tell the user:
- The full path of the new (or updated) note
- Which existing notes were updated with backlinks, and where
- Any related topics you linked that don't have a note yet (so the user knows what's still missing)
