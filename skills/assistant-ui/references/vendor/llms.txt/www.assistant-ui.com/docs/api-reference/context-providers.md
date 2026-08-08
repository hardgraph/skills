# Context Providers API Reference
URL: /docs/api-reference/context-providers

React context providers including AssistantRuntimeProvider that scope assistant-ui runtime, thread, message part, and attachment state for primitives, hooks, and custom chat components.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Context providers define where assistant-ui primitives and hooks read their state. Most apps only need `AssistantRuntimeProvider`, while scoped providers support custom renderers, isolated message parts, and advanced composition.

## Pages

- [AssistantRuntimeProvider](/docs/api-reference/context-providers/assistant-runtime-provider) — Root React provider that connects an assistant-ui runtime to primitives, hooks, threads, and composer state.
- [Scoped Providers](/docs/api-reference/context-providers/scoped-providers) — Lower-level assistant-ui providers for custom renderers, scoped message parts, attachments, and advanced composition.