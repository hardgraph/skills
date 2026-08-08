# Hooks API Reference
URL: /docs/api-reference/hooks

React hooks for assistant-ui: useAui, useAuiState, runtime creation, model context registration, and helpers for building custom AI chat behavior.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Hooks are the escape hatch and integration layer for React applications. Use them to read assistant state, call runtime actions, create runtimes, register model context, and connect custom components to assistant-ui behavior.

## Pages

- [Composer Trigger Hooks](/docs/api-reference/hooks/composer-triggers) — Unstable assistant-ui hooks for mention menus, slash commands, and custom composer trigger popovers.
- [Model Context Hooks](/docs/api-reference/hooks/model-context) — React hooks for registering assistant-ui tools, data renderers, instructions, and model context providers.
- [Primitive Hooks](/docs/api-reference/hooks/primitives) — Primitive hooks for reading scoped assistant-ui runtime state, viewport behavior, timing, and message part data inside React components.
- [Runtime Hooks](/docs/api-reference/hooks/runtimes) — Runtime creation hooks for local, remote, cloud, external-store, and AI SDK powered assistant-ui chat experiences.
- [State Hooks](/docs/api-reference/hooks/state) — State selector and action hooks for reading assistant-ui runtime state and controlling threads, composers, messages, and attachments.