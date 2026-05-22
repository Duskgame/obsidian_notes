# pnpm

[pnpm Documentation](https://pnpm.io/) | [pnpm vs npm vs yarn](https://pnpm.io/benchmarks)

pnpm (Performant npm) is a JavaScript package manager that is a drop-in replacement for npm and yarn. It uses a global content-addressable store and hard links instead of copying packages into each project's `node_modules` — this makes it faster and significantly more disk-efficient.

---

## How It Differs from npm

| | npm | pnpm |
|---|---|---|
| `node_modules` layout | Flat (hoisted) | Symlinked from global store |
| Disk usage | One copy per project | One copy total, hard-linked |
| Install speed | Baseline | Faster (cached, no re-downloading) |
| Lock file | `package-lock.json` | `pnpm-lock.yaml` |
| Command prefix | `npm` | `pnpm` |

npm copies every dependency into each project's `node_modules`. pnpm stores each package version once globally and links to it from every project that uses it.

---

## Installing pnpm

```bash
# Using npm (one-time bootstrap)
npm install -g pnpm

# Or using the standalone installer (no Node needed)
curl -fsSL https://get.pnpm.io/install.sh | sh -
```

After installing, `pnpm` is available as a global command.

---

## Switching a Project from npm to pnpm

```bash
# 1. Remove the old npm lock file and node_modules
rm -rf node_modules package-lock.json

# 2. Install with pnpm
pnpm install
# → creates pnpm-lock.yaml

# 3. Use pnpm for all future commands
pnpm dev          # instead of npm run dev
pnpm run build    # instead of npm run build
pnpm add <pkg>    # instead of npm install <pkg>
pnpm remove <pkg> # instead of npm uninstall <pkg>
```

The `package.json` file stays the same — only the lock file and `node_modules` structure change.

---

## Common Commands

| npm | pnpm equivalent |
|---|---|
| `npm install` | `pnpm install` |
| `npm run dev` | `pnpm dev` (or `pnpm run dev`) |
| `npm install <pkg>` | `pnpm add <pkg>` |
| `npm install -D <pkg>` | `pnpm add -D <pkg>` |
| `npm uninstall <pkg>` | `pnpm remove <pkg>` |
| `npx <cmd>` | `pnpm dlx <cmd>` |

---

## SvelteKit with pnpm

SvelteKit works with pnpm out of the box. The `create svelte` scaffold even suggests pnpm:

```bash
pnpm create svelte@latest my-app
cd my-app
pnpm install
pnpm dev
```

---

## Related Topics

- [[SvelteKit]] — SvelteKit projects commonly use pnpm
- [[JavaScript Promises]] — pnpm runs JS tooling that relies on async Node.js APIs
