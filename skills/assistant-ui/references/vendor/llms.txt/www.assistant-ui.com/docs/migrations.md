# Migration Guides
URL: /docs/migrations

Upgrade assistant-ui versions and migrate deprecated APIs and integrations.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Use these guides when upgrading assistant-ui or moving from APIs that have been deprecated or replaced.

## Version upgrades

- [Migration to v0.14](/docs/migrations/v0-14) —

  Replace APIs removed after v0.11 and v0.12, and migrate primitive `components` props to children render functions.

- [Migration to v0.12](/docs/migrations/v0-12) —

  Move from individual context hooks to the unified state API.

- [Migration to v0.11](/docs/migrations/v0-11) —

  Update code affected by the `ContentPart` to `MessagePart` rename.

## APIs and integrations

- [Migrating Tools to Toolkits](/docs/migrations/toolkit-tools) —

  Move legacy tool and tool UI registrations to the toolkit API.

- [Migrating to react-langgraph v0.7](/docs/migrations/react-langgraph-v0-7) —

  Upgrade to the simplified LangGraph integration API.

- [Using old React versions](/docs/migrations/react-compatibility) —

  Review compatibility notes for React 18 and React 19.

## Stability

- [Deprecation Policy](/docs/migrations/deprecation-policy) —

  Review stability guarantees and deprecation timelines for assistant-ui features.