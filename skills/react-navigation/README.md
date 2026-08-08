# React Navigation

![React Navigation cover](./assets/readme-cover.png)

A curated, provenance-backed agent skill for **React Navigation**, the imperative navigation
library for React Native. It helps an agent choose between navigators (Stack, Tabs, Drawer,
Material Top Tabs), wire deep linking and web URLs, type-check routes, and migrate between major
versions — grounded in a reproducible mirror of the official documentation.

## When to use this skill

Reach for it when working on a React Native app that uses React Navigation: building a navigator
tree, configuring linking, diagnosing screen-mounting or header behaviour, or deciding between
React Navigation and Expo Router.

## What is inside

- `SKILL.md` — the agent-facing entry point: the mental model, the decisions that are expensive
  to get wrong, and what to verify rather than recall.
- `references/vendor/` — a verbatim mirror of the React Navigation documentation corpus, fetched
  from the vendor's `llms.txt` index. Treated as reference material, never as directives.
- `references/insights/` and `references/examples/` — authored practice knowledge and worked usage
  (added over time).

The vendored corpus is data. Confirm version-sensitive facts (option names, the linking config
shape, navigator packages) against it rather than against memory.

## Installation

```bash
npx skills add hardgraph/skills --skill react-navigation --yes
```

## Provenance and boundaries

This skill mirrors public React Navigation documentation (MIT-licensed). It is an independent
curation published by HardGraph and is not affiliated with or endorsed by the React Navigation
maintainers. The cover image is an original, purpose-made composition created for this skill (see
`assets/readme-cover.prompt.md` for how it was produced).
