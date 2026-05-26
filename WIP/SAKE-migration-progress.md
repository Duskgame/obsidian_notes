# SAKE Migration Progress
Migrating `wip-gcp-project-key-rotation` (original) to match the state of `wip-gcp-project-key-rotation-test`.

Both repos point to the same remote: `git@github.com:bonprix/wip-gcp-project-key-rotation.git`
All changes are being applied **manually** to the original, with explanations along the way.

---

## Step 1 — Config files ✅ DONE

| File | Change | Status |
|---|---|---|
| `webapp/.npmrc` | Added `approve-builds=esbuild` | ✅ |
| `webapp/package.json` | Added `"@tailwindcss/vite": "^4.1.4"` to devDependencies | ✅ |
| `webapp/vite.config.ts` | Imported `tailwindcss` from `@tailwindcss/vite`, added `tailwindcss()` to plugins | ✅ |
| `webapp/src/app.html` | Added Inter font preconnect + stylesheet link (3 lines after viewport meta) | ✅ |
| `infrastructure/dev/cloudfunctions/main.py` | Already had the import — no change needed | ✅ |
| `infrastructure/dev/cloudfunctions/manual.py` | Already had the import — no change needed | ✅ |

---

## Step 2 — New files (creating manually with explanations) 🔄 IN PROGRESS

### File 1 of 6: `webapp/src/app.css` 🔄 IN PROGRESS
Global CSS entry point. Contains:
- `@import 'tailwindcss'` — Tailwind v4 entry point
- `@layer base` — sets Inter as default font
- `@layer components` — design system classes (`.card`, `.btn-primary`, `.input`, etc.) using `@apply`

**Resume here next session.**

### File 2 of 6: `webapp/src/routes/+layout.svelte` ⏳ TODO
6-line SvelteKit layout. Imports `app.css` and renders `{@render children()}`.

### File 3 of 6: `webapp/src/lib/components/TopBar.svelte` ⏳ TODO
Sticky top navigation bar. Props: `title`, `backHref`, `steps[]`, `currentStep`.
Internally uses `StepIndicator`.

### File 4 of 6: `webapp/src/lib/components/StepIndicator.svelte` ⏳ TODO
Progress indicator component. Props: `steps[]`, `current`.
Renders numbered circles: blue=active, green+checkmark=done, grey=future.

### File 5 of 6: `webapp/src/routes/request/logic.svelte.ts` ⏳ TODO
All crypto/cert/state logic extracted from the request page into a `createRequestState()` factory function.
Uses Svelte 5 runes (`$state`, `$derived`). Returns a public interface object with getters/setters.

### File 6 of 6: `webapp/src/routes/upload/logic.svelte.ts` ⏳ TODO
Same pattern as request logic. Contains Google OAuth, gapi key upload, CLI command generation.
Exports `createUploadState()` and types `UploadStep`, `ExistingKey`.

---

## Step 3 — Page rewrites ⏳ TODO

| File | Change |
|---|---|
| `webapp/src/routes/+page.svelte` | Full Tailwind redesign of landing page |
| `webapp/src/routes/request/+page.svelte` | Tailwind UI + logic moved to `logic.svelte.ts` |
| `webapp/src/routes/upload/+page.svelte` | Tailwind UI + logic moved to `logic.svelte.ts` |

---

## Step 4 — Install & verify ⏳ TODO
```bash
cd webapp && pnpm install && pnpm run dev
```
