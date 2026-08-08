---
name: knip
description: Knip — finds and fixes unused dependencies, exports, types, and files in JavaScript and TypeScript projects, including monorepos and workspaces. Use when auditing a JS/TS codebase for dead code, removing unused npm dependencies, cleaning up unexported/unreferenced files, configuring entry/ignore/project files, running in CI, or interpreting Knip's reporters and configuration hints. Covers the CLI, knip.json configuration, plugins, production mode, and auto-fix.
---

# knip

`knip` (pronounced with a hard K, Dutch for "to cut") finds and fixes **unused
dependencies, exports, and files** in JavaScript and TypeScript projects. Less
code and fewer dependencies mean faster startup, smaller bundles, easier
maintenance, simpler onboarding, and a CI gate that prevents dead code from
creeping back in.

## What knip looks for

Three categories of clutter, comprehensively:

- **Unused files** — source files nothing imports or references.
- **Unused exports** — exported symbols (functions, types, constants) nothing
  consumes. Namespace imports and re-exports are tracked, not just direct imports.
- **Unused dependencies** — npm packages listed in `dependencies` /
  `devDependencies` that no source file references.

It does this across greenfield and legacy code, in single packages and in
monorepos/workspaces, by building a dependency graph from configured **entry
files** and **plugins**.

## Quick start

```bash
npx knip                       # analyze the current project
npx @knip/create-config        # scaffold/initialize knip.json
```

Or install it: `npm install -D knip` / `pnpm add -D knip` / `bun add -D knip`.
Editor integrations: `@knip/language-server`, the VS Code extension, and
`@knip/mcp` for AI agents.

## The mental model: entry files, plugins, and the graph

Knip does not guess. It starts from **entry files** (the places execution or
consumption begins) and traces the import graph outward. Anything reachable is
"used"; anything not reachable from any entry is reported. This is why
configuration matters:

- **Entry files** (`entry`, `project`) — the roots of the graph. If knip reports
  something as unused that you know is used, an entry file is usually missing
  or misconfigured.
- **Plugins** — knip ships plugins for frameworks (Next.js, Astro, Vite, Remix,
  Storybook, Tailwind, etc.) so framework-specific entry points and conventions
  are understood without manual configuration.
- **Ignore** (`ignore`, `ignoreBinaries`, `ignoreDependencies`) — deliberately
  allowlisted values that knip should not flag (e.g. globally-injected deps,
  runtime-only binaries referenced in scripts).

## Production mode

`knip --production` analyzes the graph as it would exist in production —
excluding dev-only entry files and devDependencies — so you can catch
dependencies that leaked into the wrong section or code reachable only in
development. Use it before publishing.

## Configuration

Knip reads `knip.json` / `knip.jsonc` / `knip.ts` (or a `knip` field in
`package.json`). A minimal shape:

```json
{
  "entry": ["src/index.ts"],
  "project": ["src/**/*.ts"],
  "ignore": ["src/generated/**"],
  "ignoreDependencies": ["eslint-plugin-x"]
}
```

For monorepos and workspaces, configuration lives per-package and at the root;
`packages/docs/src/content/docs/features/integrated-monorepos.md` and
`monorepos-and-workspaces.md` cover the workspace model. Configuration hints in
the output suggest the exact key to add when knip cannot resolve something.

## Running in CI

Knip exits non-zero when it finds unused code, which makes it a CI gate:

```bash
knip                         # exits 1 on findings → fails the CI step
```

Adopt it gradually (`guides/adopt-gradually.md`) — start with ignores and tighten
over time so the gate is green before it blocks merges. `guides/using-knip-in-ci.md`
covers common CI setups.

## Reporters and auto-fix

- **Reporters** (`--reporter`) control output format (default, symbols, compact,
  JSON, custom). Pick the one a human or a downstream tool can read.
- **Auto-fix** (`--fix`) removes the unused files, exports, and dependencies it
  found, where safe. Review the diff before committing; some removals (e.g.
  re-exported barrel entries) have callers knip cannot statically see.

## What surprises people

- **False "unused" almost always means a missing entry file or plugin.** knip is
  exact about what is reachable; if a used thing is flagged, the graph did not
  start from the right root. Fix configuration before ignore-listing.
- **Namespace and re-exports are tracked.** `export *` and `import * as ns` do
  not silently hide usage; knip follows them, but they also make the graph
  harder to reason about — see `guides/namespace-imports.md`.
- **`--fix` can remove things with dynamic callers.** Anything referenced only
  through string-based dynamic imports or runtime conventions knip cannot see is
  a candidate for removal; protect it with `ignore` or `ignoreExports`.
- **Production mode changes the answer.** A dependency that looks used in
  development can be dev-only; `--production` re-evaluates the graph without dev
  entries.

## What to verify rather than recall

Plugin names, the exact configuration keys, reporter names, supported config
file formats, and CLI flags change between releases. Confirm them against the
mirrored corpus under `references/vendor/` or `knip --help` rather than asserting
a remembered flag — a renamed flag or key fails silently or misconfigures the
graph.

## References

- [Knip website](https://knip.dev)
- [Knip repository](https://github.com/webpro-nl/knip)
- [Getting started](https://knip.dev/overview/getting-started)
- [Configuration reference](https://knip.dev/reference/configuration)
- [How Knip works](https://knip.dev/explanations/how-knip-works)
