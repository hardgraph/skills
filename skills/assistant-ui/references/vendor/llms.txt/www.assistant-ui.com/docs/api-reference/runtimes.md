# Runtime State API Reference
URL: /docs/api-reference/runtimes

Runtime state and actions exposed through useAui and useAuiState, covering AssistantRuntime, ThreadRuntime, ThreadListRuntime, ComposerRuntime, MessageRuntime, and attachment APIs for controlling assistant-ui chat.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Runtime state is the typed control surface behind assistant-ui. These pages document the actions and state objects available through `useAui`, `useAuiState`, and scoped runtimes.

## Pages

- [AssistantRuntime](/docs/api-reference/runtimes/assistant-runtime) — Top-level assistant-ui runtime actions and state for tools, threads, composers, messages, and assistant behavior.
- [AttachmentRuntime](/docs/api-reference/runtimes/attachment-runtime) — AttachmentRuntime state and actions for reading attachment data and controlling files inside assistant-ui messages and composers.
- [ComposerRuntime](/docs/api-reference/runtimes/composer-runtime) — ComposerRuntime state and actions for controlling assistant-ui composer text, attachments, submission, cancellation, and pending input.
- [MessagePartRuntime](/docs/api-reference/runtimes/message-part-runtime) — MessagePartRuntime state and helpers for inspecting assistant-ui text, tool calls, data parts, reasoning, and custom message content.
- [MessageRuntime](/docs/api-reference/runtimes/message-runtime) — MessageRuntime state and actions for editing, reloading, copying, rating, speaking, and branching assistant-ui messages.
- [QueueItemState](/docs/api-reference/runtimes/queue-state) — State shape for queued assistant-ui thread operations and pending runtime work.
- [ThreadListItemRuntime](/docs/api-reference/runtimes/thread-list-item-runtime) — ThreadListItemRuntime state and actions for selecting, archiving, unarchiving, deleting, and renaming assistant-ui conversations.
- [ThreadListRuntime](/docs/api-reference/runtimes/thread-list-runtime) — ThreadListRuntime state and actions for managing remote assistant-ui conversations, active thread selection, and new thread creation.
- [ThreadRuntime](/docs/api-reference/runtimes/thread-runtime) — ThreadRuntime state and actions for controlling assistant-ui messages, composers, suggestions, model context, and the full thread lifecycle.