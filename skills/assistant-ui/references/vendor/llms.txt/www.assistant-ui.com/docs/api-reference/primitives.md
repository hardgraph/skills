# Primitives API Reference
URL: /docs/api-reference/primitives

Composable React primitives for assistant-ui chat UIs: Thread, Composer, Message, BranchPicker, ActionBar, and the parts that build threads, message lists, attachments, and editing flows.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Primitives are the low-level React components behind assistant-ui interfaces. They provide accessible behavior, state wiring, and composition points while leaving layout and styling under your control.

## Pages

- [ActionBarPrimitive](/docs/api-reference/primitives/action-bar) — Composable message action controls for copy, edit, reload, speech, and feedback in assistant-ui chat interfaces.
- [ActionBarMorePrimitive](/docs/api-reference/primitives/action-bar-more) — Overflow menu primitives for grouping secondary assistant message actions in a custom React UI.
- [AuiIf](/docs/api-reference/primitives/assistant-if) — Conditional rendering primitive for showing React UI from assistant-ui thread, message, composer, and runtime state.
- [AssistantModalPrimitive](/docs/api-reference/primitives/assistant-modal) — Floating assistant modal primitives for building support chat, copilot, and embedded assistant experiences.
- [AttachmentPrimitive](/docs/api-reference/primitives/attachment) — Attachment primitives for rendering file previews, names, thumbnails, and remove controls in assistant-ui messages and composers.
- [BranchPickerPrimitive](/docs/api-reference/primitives/branch-picker) — Branch picker primitives for navigating regenerated assistant responses and alternate message paths inside a chat thread.
- [ChainOfThoughtPrimitive](/docs/api-reference/primitives/chain-of-thought) — Chain of thought primitives for rendering assistant reasoning, step lists, and collapsible disclosure UI in message content.
- [ComposerPrimitive](/docs/api-reference/primitives/composer) — Composable input primitives for assistant-ui prompts, send controls, cancellation, attachments, and composer state.
- [Composition](/docs/api-reference/primitives/composition) — How to compose primitives with custom components using asChild.
- [ErrorPrimitive](/docs/api-reference/primitives/error) — Error primitives for rendering assistant-ui runtime, thread, and message failures inside custom chat interfaces.
- [MessagePrimitive](/docs/api-reference/primitives/message) — Message primitives for rendering assistant and user turns, message parts, attachments, actions, editing, and branch controls.
- [MessagePartPrimitive](/docs/api-reference/primitives/message-part) — Message part primitives for rendering text, tool calls, data parts, reasoning, source content, and custom assistant output.
- [QueueItemPrimitive](/docs/api-reference/primitives/queue-item) — Queue item primitives for rendering pending assistant-ui thread operations, optimistic work, and runtime queue state.
- [SelectionToolbarPrimitive](/docs/api-reference/primitives/selection-toolbar) — Selection toolbar primitives for quote, copy, and contextual actions on selected chat text.
- [SuggestionPrimitive](/docs/api-reference/primitives/suggestion) — Suggestion primitives for rendering starter prompts, follow-up actions, and composer suggestions in assistant-ui threads.
- [ThreadPrimitive](/docs/api-reference/primitives/thread) — Thread primitives for rendering chat transcripts, message lists, viewport state, suggestions, and composers in assistant-ui.
- [ThreadListPrimitive](/docs/api-reference/primitives/thread-list) — Thread list primitives for rendering conversation navigation, new thread actions, and custom assistant sidebars.
- [ThreadListItemPrimitive](/docs/api-reference/primitives/thread-list-item) — Thread list item primitives for rendering selectable conversation rows with titles, archive controls, delete actions, and menus.
- [ThreadListItemMorePrimitive](/docs/api-reference/primitives/thread-list-item-more) — Overflow menu primitives for secondary thread list item actions in custom assistant-ui sidebars.