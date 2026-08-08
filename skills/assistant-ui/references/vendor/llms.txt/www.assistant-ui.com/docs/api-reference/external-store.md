# External Store API Reference
URL: /docs/api-reference/external-store

External store runtime, message conversion helpers, and adapters for assistant-ui React apps that own their chat state outside the runtime.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

External store APIs are for applications that already have their own message, thread, or persistence model. They let assistant-ui render and control that state without forcing the app to adopt the built-in runtime storage shape.

## Pages

- [Message Conversion](/docs/api-reference/external-store/message-conversion) — Convert external message formats into assistant-ui's message and thread state for the external store runtime.
- [External Store Runtime](/docs/api-reference/external-store/runtime) — Runtime components, options, and adapters for using assistant-ui with externally owned chat state.