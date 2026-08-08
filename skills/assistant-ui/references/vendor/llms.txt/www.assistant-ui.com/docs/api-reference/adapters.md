# Adapters API Reference
URL: /docs/api-reference/adapters

Adapter interfaces for connecting chat models, persistence, file attachments, feedback, and suggestions to assistant-ui React runtimes.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Adapters define the boundaries between assistant-ui and the systems around it. Use them when you need custom model execution, attachment handling, persistence, feedback collection, or prompt suggestions.

## Pages

- [Attachment Adapters](/docs/api-reference/adapters/attachments) — Attachment adapters for uploading files, handling lifecycle events, and bringing app-owned content into assistant-ui composers and messages.
- [Feedback Adapter](/docs/api-reference/adapters/feedback) — Capture and respond to message feedback submitted through action primitives or runtime actions.
- [Model Adapters](/docs/api-reference/adapters/model) — Adapter interfaces for connecting chat models, streaming responses, and model execution to assistant-ui runtimes.
- [Persistence Adapters](/docs/api-reference/adapters/persistence) — Persistence adapters for saving assistant-ui message history, remote thread lists, and long-running chat sessions across browser reloads.
- [Runtime Adapter Context](/docs/api-reference/adapters/runtime) — Provide assistant-ui runtime adapters through React context for model, attachment, speech, and feedback behavior.
- [Suggestion Adapters](/docs/api-reference/adapters/suggestions) — Suggestion adapters for providing starter prompts, contextual actions, and guided composer options to assistant-ui runtimes.