# assistant-ui documentation sitemap

Base URL: https://www.assistant-ui.com

## Documentation

- [Documentation](https://www.assistant-ui.com/docs)
  Markdown: https://www.assistant-ui.com/docs.md
  Description: Components, runtimes, and primitives for building AI chat interfaces in React, React Native, and the terminal.
  Last updated: 2026-08-08

- [Adapters API Reference](https://www.assistant-ui.com/docs/api-reference/adapters)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters.md
  Description: Adapter interfaces for connecting chat models, persistence, file attachments, feedback, and suggestions to assistant-ui React runtimes.
  Last updated: 2026-08-08

- [Attachment Adapters](https://www.assistant-ui.com/docs/api-reference/adapters/attachments)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters/attachments.md
  Description: Attachment adapters for uploading files, handling lifecycle events, and bringing app-owned content into assistant-ui composers and messages.
  Last updated: 2026-08-08

- [Feedback Adapter](https://www.assistant-ui.com/docs/api-reference/adapters/feedback)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters/feedback.md
  Description: Capture and respond to message feedback submitted through action primitives or runtime actions.
  Last updated: 2026-08-08

- [Model Adapters](https://www.assistant-ui.com/docs/api-reference/adapters/model)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters/model.md
  Description: Adapter interfaces for connecting chat models, streaming responses, and model execution to assistant-ui runtimes.
  Last updated: 2026-08-08

- [Persistence Adapters](https://www.assistant-ui.com/docs/api-reference/adapters/persistence)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters/persistence.md
  Description: Persistence adapters for saving assistant-ui message history, remote thread lists, and long-running chat sessions across browser reloads.
  Last updated: 2026-08-08

- [Runtime Adapter Context](https://www.assistant-ui.com/docs/api-reference/adapters/runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters/runtime.md
  Description: Provide assistant-ui runtime adapters through React context for model, attachment, speech, and feedback behavior.
  Last updated: 2026-08-08

- [Suggestion Adapters](https://www.assistant-ui.com/docs/api-reference/adapters/suggestions)
  Markdown: https://www.assistant-ui.com/docs/api-reference/adapters/suggestions.md
  Description: Suggestion adapters for providing starter prompts, contextual actions, and guided composer options to assistant-ui runtimes.
  Last updated: 2026-08-08

- [Context Providers API Reference](https://www.assistant-ui.com/docs/api-reference/context-providers)
  Markdown: https://www.assistant-ui.com/docs/api-reference/context-providers.md
  Description: React context providers including AssistantRuntimeProvider that scope assistant-ui runtime, thread, message part, and attachment state for primitives, hooks, and custom chat components.
  Last updated: 2026-08-08

- [AssistantRuntimeProvider](https://www.assistant-ui.com/docs/api-reference/context-providers/assistant-runtime-provider)
  Markdown: https://www.assistant-ui.com/docs/api-reference/context-providers/assistant-runtime-provider.md
  Description: Root React provider that connects an assistant-ui runtime to primitives, hooks, threads, and composer state.
  Last updated: 2026-08-08

- [Scoped Providers](https://www.assistant-ui.com/docs/api-reference/context-providers/scoped-providers)
  Markdown: https://www.assistant-ui.com/docs/api-reference/context-providers/scoped-providers.md
  Description: Lower-level assistant-ui providers for custom renderers, scoped message parts, attachments, and advanced composition.
  Last updated: 2026-08-08

- [External Store API Reference](https://www.assistant-ui.com/docs/api-reference/external-store)
  Markdown: https://www.assistant-ui.com/docs/api-reference/external-store.md
  Description: External store runtime, message conversion helpers, and adapters for assistant-ui React apps that own their chat state outside the runtime.
  Last updated: 2026-08-08

- [Message Conversion](https://www.assistant-ui.com/docs/api-reference/external-store/message-conversion)
  Markdown: https://www.assistant-ui.com/docs/api-reference/external-store/message-conversion.md
  Description: Convert external message formats into assistant-ui's message and thread state for the external store runtime.
  Last updated: 2026-08-08

- [External Store Runtime](https://www.assistant-ui.com/docs/api-reference/external-store/runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/external-store/runtime.md
  Description: Runtime components, options, and adapters for using assistant-ui with externally owned chat state.
  Last updated: 2026-08-08

- [Generative UI API Reference](https://www.assistant-ui.com/docs/api-reference/generative-ui)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui.md
  Description: Spec-driven generative UI for assistant-ui. The data format an assistant streams, the component registry that resolves it, and the renderer that turns it into React elements.
  Last updated: 2026-08-08

- [A2UI](https://www.assistant-ui.com/docs/api-reference/generative-ui/a2ui)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/a2ui.md
  Description: Convert A2UI surface operations into generative UI specs.
  Last updated: 2026-08-08

- [Generative UI Actions](https://www.assistant-ui.com/docs/api-reference/generative-ui/actions)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/actions.md
  Description: Register handlers for model-emitted $action payloads and dispatch them from interactive generative UI components.
  Last updated: 2026-08-08

- [Generative UI Components](https://www.assistant-ui.com/docs/api-reference/generative-ui/components)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/components.md
  Description: Define the component library JSONGenerativeUI can render, including schemas, render functions, and the default vocabulary.
  Last updated: 2026-08-08

- [JSONGenerativeUI](https://www.assistant-ui.com/docs/api-reference/generative-ui/json-generative-ui)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/json-generative-ui.md
  Description: Create present and prompt_user tools from a generative UI component library, with options for display mode and action dispatch.
  Last updated: 2026-08-08

- [Generative UI Rendering](https://www.assistant-ui.com/docs/api-reference/generative-ui/rendering)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/rendering.md
  Description: Render generative UI trees, build present-tool schemas, and inspect or serialize model-produced UI nodes.
  Last updated: 2026-08-08

- [Slack Block Kit](https://www.assistant-ui.com/docs/api-reference/generative-ui/slack)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/slack.md
  Description: Convert generative UI trees to Slack Block Kit payloads and decode block_actions interactions back into $action payloads.
  Last updated: 2026-08-08

- [Generative UI Spec](https://www.assistant-ui.com/docs/api-reference/generative-ui/spec)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/spec.md
  Description: The serializable node tree an assistant emits to describe generative UI. Covers the GenerativeUISpec format, its nodes, and the message part that carries the spec.
  Last updated: 2026-08-08

- [Microsoft Teams](https://www.assistant-ui.com/docs/api-reference/generative-ui/teams)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/teams.md
  Description: Convert generative UI trees to Teams Adaptive Cards and decode Action.Submit payloads back into $action payloads.
  Last updated: 2026-08-08

- [Generative UI Tokens](https://www.assistant-ui.com/docs/api-reference/generative-ui/tokens)
  Markdown: https://www.assistant-ui.com/docs/api-reference/generative-ui/tokens.md
  Description: Shared token arrays used by the default generative UI vocabulary for sizing, color, alignment, and component variants.
  Last updated: 2026-08-08

- [Hooks API Reference](https://www.assistant-ui.com/docs/api-reference/hooks)
  Markdown: https://www.assistant-ui.com/docs/api-reference/hooks.md
  Description: React hooks for assistant-ui: useAui, useAuiState, runtime creation, model context registration, and helpers for building custom AI chat behavior.
  Last updated: 2026-08-08

- [Composer Trigger Hooks](https://www.assistant-ui.com/docs/api-reference/hooks/composer-triggers)
  Markdown: https://www.assistant-ui.com/docs/api-reference/hooks/composer-triggers.md
  Description: Unstable assistant-ui hooks for mention menus, slash commands, and custom composer trigger popovers.
  Last updated: 2026-08-08

- [Model Context Hooks](https://www.assistant-ui.com/docs/api-reference/hooks/model-context)
  Markdown: https://www.assistant-ui.com/docs/api-reference/hooks/model-context.md
  Description: React hooks for registering assistant-ui tools, data renderers, instructions, and model context providers.
  Last updated: 2026-08-08

- [Primitive Hooks](https://www.assistant-ui.com/docs/api-reference/hooks/primitives)
  Markdown: https://www.assistant-ui.com/docs/api-reference/hooks/primitives.md
  Description: Primitive hooks for reading scoped assistant-ui runtime state, viewport behavior, timing, and message part data inside React components.
  Last updated: 2026-08-08

- [Runtime Hooks](https://www.assistant-ui.com/docs/api-reference/hooks/runtimes)
  Markdown: https://www.assistant-ui.com/docs/api-reference/hooks/runtimes.md
  Description: Runtime creation hooks for local, remote, cloud, external-store, and AI SDK powered assistant-ui chat experiences.
  Last updated: 2026-08-08

- [State Hooks](https://www.assistant-ui.com/docs/api-reference/hooks/state)
  Markdown: https://www.assistant-ui.com/docs/api-reference/hooks/state.md
  Description: State selector and action hooks for reading assistant-ui runtime state and controlling threads, composers, messages, and attachments.
  Last updated: 2026-08-08

- [Integrations API Reference](https://www.assistant-ui.com/docs/api-reference/integrations)
  Markdown: https://www.assistant-ui.com/docs/api-reference/integrations.md
  Description: Package-level APIs for connecting assistant-ui React to the Vercel AI SDK, Assistant Cloud, and adjacent chat ecosystem hooks and runtimes.
  Last updated: 2026-08-08

- [@assistant-ui/cloud-ai-sdk](https://www.assistant-ui.com/docs/api-reference/integrations/cloud-ai-sdk)
  Markdown: https://www.assistant-ui.com/docs/api-reference/integrations/cloud-ai-sdk.md
  Description: Assistant Cloud AI SDK hooks for connecting cloud-backed threads, persistence, and chat state to assistant-ui React runtimes.
  Last updated: 2026-08-08

- [@assistant-ui/eve](https://www.assistant-ui.com/docs/api-reference/integrations/eve)
  Markdown: https://www.assistant-ui.com/docs/api-reference/integrations/eve.md
  Description: Eve runtime hook and message conversion utilities for assistant-ui React applications.
  Last updated: 2026-08-08

- [@assistant-ui/react-ai-sdk](https://www.assistant-ui.com/docs/api-reference/integrations/react-ai-sdk)
  Markdown: https://www.assistant-ui.com/docs/api-reference/integrations/react-ai-sdk.md
  Description: Vercel AI SDK runtime hooks, chat transports, and message conversion utilities for assistant-ui React applications.
  Last updated: 2026-08-08

- [@assistant-ui/react-data-stream](https://www.assistant-ui.com/docs/api-reference/integrations/react-data-stream)
  Markdown: https://www.assistant-ui.com/docs/api-reference/integrations/react-data-stream.md
  Description: Data stream runtime hook and message conversion utilities for custom assistant-ui streaming backends.
  Last updated: 2026-08-08

- [Model Context API Reference](https://www.assistant-ui.com/docs/api-reference/model-context)
  Markdown: https://www.assistant-ui.com/docs/api-reference/model-context.md
  Description: Model instructions, contextual state, provider registries, and renderers for giving assistant-ui runtimes app-aware context.
  Last updated: 2026-08-08

- [Model Context](https://www.assistant-ui.com/docs/api-reference/model-context/context)
  Markdown: https://www.assistant-ui.com/docs/api-reference/model-context/context.md
  Description: Provide model instructions, contextual state, and inline renderers to assistant-ui runtimes.
  Last updated: 2026-08-08

- [Model Context Registry](https://www.assistant-ui.com/docs/api-reference/model-context/registry)
  Markdown: https://www.assistant-ui.com/docs/api-reference/model-context/registry.md
  Description: Register and manage assistant-ui model context providers that contribute instructions and app state.
  Last updated: 2026-08-08

- [API Reference](https://www.assistant-ui.com/docs/api-reference/overview)
  Markdown: https://www.assistant-ui.com/docs/api-reference/overview.md
  Description: Complete assistant-ui React API reference for building AI chat UIs with primitives, hooks, runtimes, adapters, tools, transport, voice, and integrations.
  Last updated: 2026-08-08

- [Primitives API Reference](https://www.assistant-ui.com/docs/api-reference/primitives)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives.md
  Description: Composable React primitives for assistant-ui chat UIs: Thread, Composer, Message, BranchPicker, ActionBar, and the parts that build threads, message lists, attachments, and editing flows.
  Last updated: 2026-08-08

- [ActionBarPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/action-bar)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/action-bar.md
  Description: Composable message action controls for copy, edit, reload, speech, and feedback in assistant-ui chat interfaces.
  Last updated: 2026-08-08

- [ActionBarMorePrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/action-bar-more)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/action-bar-more.md
  Description: Overflow menu primitives for grouping secondary assistant message actions in a custom React UI.
  Last updated: 2026-08-08

- [AuiIf](https://www.assistant-ui.com/docs/api-reference/primitives/assistant-if)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/assistant-if.md
  Description: Conditional rendering primitive for showing React UI from assistant-ui thread, message, composer, and runtime state.
  Last updated: 2026-08-08

- [AssistantModalPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/assistant-modal)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/assistant-modal.md
  Description: Floating assistant modal primitives for building support chat, copilot, and embedded assistant experiences.
  Last updated: 2026-08-08

- [AttachmentPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/attachment)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/attachment.md
  Description: Attachment primitives for rendering file previews, names, thumbnails, and remove controls in assistant-ui messages and composers.
  Last updated: 2026-08-08

- [BranchPickerPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/branch-picker)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/branch-picker.md
  Description: Branch picker primitives for navigating regenerated assistant responses and alternate message paths inside a chat thread.
  Last updated: 2026-08-08

- [ChainOfThoughtPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/chain-of-thought)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/chain-of-thought.md
  Description: Chain of thought primitives for rendering assistant reasoning, step lists, and collapsible disclosure UI in message content.
  Last updated: 2026-08-08

- [ComposerPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/composer)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/composer.md
  Description: Composable input primitives for assistant-ui prompts, send controls, cancellation, attachments, and composer state.
  Last updated: 2026-08-08

- [Composition](https://www.assistant-ui.com/docs/api-reference/primitives/composition)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/composition.md
  Description: How to compose primitives with custom components using asChild.
  Last updated: 2026-08-08

- [ErrorPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/error)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/error.md
  Description: Error primitives for rendering assistant-ui runtime, thread, and message failures inside custom chat interfaces.
  Last updated: 2026-08-08

- [MessagePrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/message)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/message.md
  Description: Message primitives for rendering assistant and user turns, message parts, attachments, actions, editing, and branch controls.
  Last updated: 2026-08-08

- [MessagePartPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/message-part)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/message-part.md
  Description: Message part primitives for rendering text, tool calls, data parts, reasoning, source content, and custom assistant output.
  Last updated: 2026-08-08

- [QueueItemPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/queue-item)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/queue-item.md
  Description: Queue item primitives for rendering pending assistant-ui thread operations, optimistic work, and runtime queue state.
  Last updated: 2026-08-08

- [SelectionToolbarPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/selection-toolbar)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/selection-toolbar.md
  Description: Selection toolbar primitives for quote, copy, and contextual actions on selected chat text.
  Last updated: 2026-08-08

- [SuggestionPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/suggestion)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/suggestion.md
  Description: Suggestion primitives for rendering starter prompts, follow-up actions, and composer suggestions in assistant-ui threads.
  Last updated: 2026-08-08

- [ThreadPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/thread)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/thread.md
  Description: Thread primitives for rendering chat transcripts, message lists, viewport state, suggestions, and composers in assistant-ui.
  Last updated: 2026-08-08

- [ThreadListPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/thread-list)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/thread-list.md
  Description: Thread list primitives for rendering conversation navigation, new thread actions, and custom assistant sidebars.
  Last updated: 2026-08-08

- [ThreadListItemPrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/thread-list-item)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/thread-list-item.md
  Description: Thread list item primitives for rendering selectable conversation rows with titles, archive controls, delete actions, and menus.
  Last updated: 2026-08-08

- [ThreadListItemMorePrimitive](https://www.assistant-ui.com/docs/api-reference/primitives/thread-list-item-more)
  Markdown: https://www.assistant-ui.com/docs/api-reference/primitives/thread-list-item-more.md
  Description: Overflow menu primitives for secondary thread list item actions in custom assistant-ui sidebars.
  Last updated: 2026-08-08

- [Runtime State API Reference](https://www.assistant-ui.com/docs/api-reference/runtimes)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes.md
  Description: Runtime state and actions exposed through useAui and useAuiState, covering AssistantRuntime, ThreadRuntime, ThreadListRuntime, ComposerRuntime, MessageRuntime, and attachment APIs for controlling assistant-ui chat.
  Last updated: 2026-08-08

- [AssistantRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/assistant-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/assistant-runtime.md
  Description: Top-level assistant-ui runtime actions and state for tools, threads, composers, messages, and assistant behavior.
  Last updated: 2026-08-08

- [AttachmentRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/attachment-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/attachment-runtime.md
  Description: AttachmentRuntime state and actions for reading attachment data and controlling files inside assistant-ui messages and composers.
  Last updated: 2026-08-08

- [ComposerRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/composer-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/composer-runtime.md
  Description: ComposerRuntime state and actions for controlling assistant-ui composer text, attachments, submission, cancellation, and pending input.
  Last updated: 2026-08-08

- [MessagePartRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/message-part-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/message-part-runtime.md
  Description: MessagePartRuntime state and helpers for inspecting assistant-ui text, tool calls, data parts, reasoning, and custom message content.
  Last updated: 2026-08-08

- [MessageRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/message-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/message-runtime.md
  Description: MessageRuntime state and actions for editing, reloading, copying, rating, speaking, and branching assistant-ui messages.
  Last updated: 2026-08-08

- [QueueItemState](https://www.assistant-ui.com/docs/api-reference/runtimes/queue-state)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/queue-state.md
  Description: State shape for queued assistant-ui thread operations and pending runtime work.
  Last updated: 2026-08-08

- [ThreadListItemRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/thread-list-item-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/thread-list-item-runtime.md
  Description: ThreadListItemRuntime state and actions for selecting, archiving, unarchiving, deleting, and renaming assistant-ui conversations.
  Last updated: 2026-08-08

- [ThreadListRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/thread-list-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/thread-list-runtime.md
  Description: ThreadListRuntime state and actions for managing remote assistant-ui conversations, active thread selection, and new thread creation.
  Last updated: 2026-08-08

- [ThreadRuntime](https://www.assistant-ui.com/docs/api-reference/runtimes/thread-runtime)
  Markdown: https://www.assistant-ui.com/docs/api-reference/runtimes/thread-runtime.md
  Description: ThreadRuntime state and actions for controlling assistant-ui messages, composers, suggestions, model context, and the full thread lifecycle.
  Last updated: 2026-08-08

- [Tools API Reference](https://www.assistant-ui.com/docs/api-reference/tools)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools.md
  Description: Tool definitions, React renderers, status helpers, and toolkits for exposing callable app capabilities to assistant-ui chat models.
  Last updated: 2026-08-08

- [Component Tools](https://www.assistant-ui.com/docs/api-reference/tools/component-tools)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools/component-tools.md
  Description: Register assistant tools from mounted React components, scoped to the lifetime of part of the UI tree.
  Last updated: 2026-08-08

- [Interactables](https://www.assistant-ui.com/docs/api-reference/tools/interactables)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools/interactables.md
  Description: Unstable interactables APIs for model-editable app and message state, including hooks, resources, toolkit helpers, and snapshot utilities.
  Last updated: 2026-08-08

- [Interactables (legacy)](https://www.assistant-ui.com/docs/api-reference/tools/interactables-legacy)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools/interactables-legacy.md
  Description: Deprecated legacy interactables APIs for registering model-editable components with per-instance update tools.
  Last updated: 2026-08-08

- [Tool Rendering](https://www.assistant-ui.com/docs/api-reference/tools/rendering)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools/rendering.md
  Description: Register React renderers for assistant-ui tool calls, tool results, and model data parts.
  Last updated: 2026-08-08

- [Tool Status](https://www.assistant-ui.com/docs/api-reference/tools/status)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools/status.md
  Description: Read tool arguments, execution status, and result state inside assistant-ui tool UI components.
  Last updated: 2026-08-08

- [Toolkits](https://www.assistant-ui.com/docs/api-reference/tools/toolkits)
  Markdown: https://www.assistant-ui.com/docs/api-reference/tools/toolkits.md
  Description: Define model-facing tools and compose them into named toolkits registered with an assistant-ui runtime scope.
  Last updated: 2026-08-08

- [Transport API Reference](https://www.assistant-ui.com/docs/api-reference/transport)
  Markdown: https://www.assistant-ui.com/docs/api-reference/transport.md
  Description: Transport commands, frame messages, and protocol types for synchronizing assistant-ui runtimes across process or iframe boundaries.
  Last updated: 2026-08-08

- [Assistant Transport](https://www.assistant-ui.com/docs/api-reference/transport/assistant-transport)
  Markdown: https://www.assistant-ui.com/docs/api-reference/transport/assistant-transport.md
  Description: Command, protocol, and transport types for connecting assistant-ui runtimes across execution boundaries.
  Last updated: 2026-08-08

- [Assistant Frame](https://www.assistant-ui.com/docs/api-reference/transport/frame)
  Markdown: https://www.assistant-ui.com/docs/api-reference/transport/frame.md
  Description: Frame bridge APIs and serialized message types for embedding assistant-ui runtimes in external contexts.
  Last updated: 2026-08-08

- [Utilities API Reference](https://www.assistant-ui.com/docs/api-reference/utilities)
  Markdown: https://www.assistant-ui.com/docs/api-reference/utilities.md
  Description: Utility exports for custom rendering, composition, and advanced assistant-ui behavior that does not fit a larger API family.
  Last updated: 2026-08-08

- [Utilities](https://www.assistant-ui.com/docs/api-reference/utilities/miscellaneous)
  Markdown: https://www.assistant-ui.com/docs/api-reference/utilities/miscellaneous.md
  Description: Miscellaneous @assistant-ui/react utilities for custom rendering, composition, and advanced assistant UI behavior.
  Last updated: 2026-08-08

- [Voice API Reference](https://www.assistant-ui.com/docs/api-reference/voice)
  Markdown: https://www.assistant-ui.com/docs/api-reference/voice.md
  Description: Realtime voice, speech synthesis, and dictation contracts for wiring spoken assistant flows into React chat UIs.
  Last updated: 2026-08-08

- [Voice Sessions](https://www.assistant-ui.com/docs/api-reference/voice/session)
  Markdown: https://www.assistant-ui.com/docs/api-reference/voice/session.md
  Description: Create and control realtime assistant-ui voice sessions, state, controls, and helpers.
  Last updated: 2026-08-08

- [Speech and Dictation](https://www.assistant-ui.com/docs/api-reference/voice/speech-dictation)
  Markdown: https://www.assistant-ui.com/docs/api-reference/voice/speech-dictation.md
  Description: Connect speech synthesis and dictation adapters to assistant-ui voice and composer workflows.
  Last updated: 2026-08-08

- [Architecture](https://www.assistant-ui.com/docs/architecture)
  Markdown: https://www.assistant-ui.com/docs/architecture.md
  Description: How components, runtimes, and cloud services fit together.
  Last updated: 2026-08-08

- [Radix UI and Base UI](https://www.assistant-ui.com/docs/base-ui)
  Markdown: https://www.assistant-ui.com/docs/base-ui.md
  Description: How the assistant-ui component registry serves Radix UI and Base UI flavored components for every shadcn style.
  Last updated: 2026-08-08

- [CLI](https://www.assistant-ui.com/docs/cli)
  Markdown: https://www.assistant-ui.com/docs/cli.md
  Description: Scaffold projects, add components, and manage updates from the command line.
  Last updated: 2026-08-08

- [Cloud Persistence](https://www.assistant-ui.com/docs/cloud)
  Markdown: https://www.assistant-ui.com/docs/cloud.md
  Description: Add managed thread persistence and chat history to your AI app in minutes — assistant-ui Cloud handles thread sync, search, and multi-tenant storage.
  Last updated: 2026-08-08

- [AI SDK](https://www.assistant-ui.com/docs/cloud/ai-sdk)
  Markdown: https://www.assistant-ui.com/docs/cloud/ai-sdk.md
  Description: Add cloud persistence to your existing AI SDK app with a single hook.
  Last updated: 2026-08-08

- [AI SDK + assistant-ui](https://www.assistant-ui.com/docs/cloud/ai-sdk-assistant-ui)
  Markdown: https://www.assistant-ui.com/docs/cloud/ai-sdk-assistant-ui.md
  Description: Integrate cloud persistence using assistant-ui runtime and pre-built components.
  Last updated: 2026-08-08

- [User Authorization](https://www.assistant-ui.com/docs/cloud/authorization)
  Markdown: https://www.assistant-ui.com/docs/cloud/authorization.md
  Description: Configure workspace auth tokens and integrate with auth providers.
  Last updated: 2026-08-08

- [LangGraph + assistant-ui](https://www.assistant-ui.com/docs/cloud/langgraph)
  Markdown: https://www.assistant-ui.com/docs/cloud/langgraph.md
  Description: Integrate cloud persistence and thread management with LangGraph Cloud.
  Last updated: 2026-08-08

- [Assistant Frame API](https://www.assistant-ui.com/docs/copilots/assistant-frame)
  Markdown: https://www.assistant-ui.com/docs/copilots/assistant-frame.md
  Description: Share model context across iframe boundaries
  Last updated: 2026-08-08

- [makeAssistantVisible](https://www.assistant-ui.com/docs/copilots/make-assistant-visible)
  Markdown: https://www.assistant-ui.com/docs/copilots/make-assistant-visible.md
  Description: Make React components visible and interactive to assistants via higher-order component wrapping.
  Last updated: 2026-08-08

- [Model Context](https://www.assistant-ui.com/docs/copilots/model-context)
  Markdown: https://www.assistant-ui.com/docs/copilots/model-context.md
  Description: Configure assistant behavior through system instructions, tools, and context providers.
  Last updated: 2026-08-08

- [Intelligent Components](https://www.assistant-ui.com/docs/copilots/motivation)
  Markdown: https://www.assistant-ui.com/docs/copilots/motivation.md
  Description: Add intelligence to React components through readable interfaces and assistant tools.
  Last updated: 2026-08-08

- [useAssistantInstructions](https://www.assistant-ui.com/docs/copilots/use-assistant-instructions)
  Markdown: https://www.assistant-ui.com/docs/copilots/use-assistant-instructions.md
  Description: React hook for setting system instructions to guide assistant behavior.
  Last updated: 2026-08-08

- [DevTools](https://www.assistant-ui.com/docs/devtools)
  Markdown: https://www.assistant-ui.com/docs/devtools.md
  Description: Inspect runtime state, context, and events in the browser.
  Last updated: 2026-08-08

- [Guides](https://www.assistant-ui.com/docs/guides)
  Markdown: https://www.assistant-ui.com/docs/guides.md
  Description: Practical recipes for building AI chat features in React with assistant-ui — attachments, branching, multi-agent, voice, slash commands, generative UI, and more.
  Last updated: 2026-08-08

- [File Attachments](https://www.assistant-ui.com/docs/guides/attachments)
  Markdown: https://www.assistant-ui.com/docs/guides/attachments.md
  Description: Let users attach images, PDFs, and other files to AI chat messages in React. Drag-drop, paste, and vision-model support, built into assistant-ui.
  Last updated: 2026-08-08

- [Message Branching](https://www.assistant-ui.com/docs/guides/branching)
  Markdown: https://www.assistant-ui.com/docs/guides/branching.md
  Description: Edit messages or regenerate AI responses, then switch between alternative replies. Branching navigation built into assistant-ui's React chat UI.
  Last updated: 2026-08-08

- [Chain of Thought UI](https://www.assistant-ui.com/docs/guides/chain-of-thought)
  Markdown: https://www.assistant-ui.com/docs/guides/chain-of-thought.md
  Description: Show AI reasoning steps and tool calls in a collapsible thinking accordion. Build chain-of-thought visualizations in React chat with assistant-ui.
  Last updated: 2026-08-08

- [ChatGPT Subscription](https://www.assistant-ui.com/docs/guides/chatgpt-subscription)
  Markdown: https://www.assistant-ui.com/docs/guides/chatgpt-subscription.md
  Description: Run your assistant-ui app on your ChatGPT Plus or Pro subscription via Codex OAuth. Local development without an OpenAI API key.
  Last updated: 2026-08-08

- [Assistant Context API](https://www.assistant-ui.com/docs/guides/context-api)
  Markdown: https://www.assistant-ui.com/docs/guides/context-api.md
  Description: Read and update assistant state to build custom React components in your chat UI — composable context API for thread, message, and runtime data via assistant-ui.
  Last updated: 2026-08-08

- [Speech-to-Text Dictation](https://www.assistant-ui.com/docs/guides/dictation)
  Markdown: https://www.assistant-ui.com/docs/guides/dictation.md
  Description: Add voice dictation to your AI chat composer with the Web Speech API or a custom adapter. Speech-to-text in React, integrated through assistant-ui.
  Last updated: 2026-08-08

- [Message Editing](https://www.assistant-ui.com/docs/guides/editing)
  Markdown: https://www.assistant-ui.com/docs/guides/editing.md
  Description: Let users edit their messages and regenerate AI responses with custom editor interfaces. Edit-and-resubmit patterns for React chat via assistant-ui.
  Last updated: 2026-08-08

- [Electron](https://www.assistant-ui.com/docs/guides/electron)
  Markdown: https://www.assistant-ui.com/docs/guides/electron.md
  Description: Run assistant-ui in an Electron renderer with a hosted backend or a secure, streaming preload and IPC bridge.
  Last updated: 2026-08-08

- [Headless Composer Input](https://www.assistant-ui.com/docs/guides/headless-composer-input)
  Markdown: https://www.assistant-ui.com/docs/guides/headless-composer-input.md
  Description: Build a custom composer input while keeping assistant-ui composer state and send gating.
  Last updated: 2026-08-08

- [Image Generation](https://www.assistant-ui.com/docs/guides/image-generation)
  Markdown: https://www.assistant-ui.com/docs/guides/image-generation.md
  Description: Generate images in your backend and render them inline in an assistant-ui thread.
  Last updated: 2026-08-08

- [Input History](https://www.assistant-ui.com/docs/guides/input-history)
  Markdown: https://www.assistant-ui.com/docs/guides/input-history.md
  Description: Terminal-style ArrowUp/ArrowDown recall of previously sent messages in the assistant-ui React composer.
  Last updated: 2026-08-08

- [LaTeX in Chat Messages](https://www.assistant-ui.com/docs/guides/latex)
  Markdown: https://www.assistant-ui.com/docs/guides/latex.md
  Description: Render LaTeX math expressions in AI chat messages with KaTeX — drop-in equation support for React chat UIs built on assistant-ui.
  Last updated: 2026-08-08

- [Mentions in Chat](https://www.assistant-ui.com/docs/guides/mentions)
  Markdown: https://www.assistant-ui.com/docs/guides/mentions.md
  Description: Let users @-mention tools or custom items in the AI chat composer to guide the LLM. Mention picker built into assistant-ui's React composer.
  Last updated: 2026-08-08

- [Message Timing & Token Stats](https://www.assistant-ui.com/docs/guides/message-timing)
  Markdown: https://www.assistant-ui.com/docs/guides/message-timing.md
  Description: Display stream metadata in AI chat — generation duration, tokens per second, and time to first token, rendered via assistant-ui's React components.
  Last updated: 2026-08-08

- [Quote Selected Text](https://www.assistant-ui.com/docs/guides/quoting)
  Markdown: https://www.assistant-ui.com/docs/guides/quoting.md
  Description: Let users select text from AI messages and quote it back into the composer. Full quoting flow with backend handling and programmatic API in assistant-ui.
  Last updated: 2026-08-08

- [Resumable Stream Deployment](https://www.assistant-ui.com/docs/guides/resumable-stream-deployment)
  Markdown: https://www.assistant-ui.com/docs/guides/resumable-stream-deployment.md
  Description: Production hardening for resumable streams. Authorization, serverless lifetimes, TTLs, key isolation, observability, resource limits, and incident response.
  Last updated: 2026-08-08

- [Custom Resumable Stream Stores](https://www.assistant-ui.com/docs/guides/resumable-stream-stores)
  Markdown: https://www.assistant-ui.com/docs/guides/resumable-stream-stores.md
  Description: Implement the ResumableStreamStore interface to back resumable streams with Postgres, Cloudflare Durable Objects, Upstash REST, InstantDB, or any other backend.
  Last updated: 2026-08-08

- [Resumable Streams](https://www.assistant-ui.com/docs/guides/resumable-streams)
  Markdown: https://www.assistant-ui.com/docs/guides/resumable-streams.md
  Description: Persist an in-flight LLM response on the server so the client can reload, lose its connection, or open a new tab and pick up the same stream.
  Last updated: 2026-08-08

- [Slash Commands](https://www.assistant-ui.com/docs/guides/slash-commands)
  Markdown: https://www.assistant-ui.com/docs/guides/slash-commands.md
  Description: Trigger predefined actions in your AI chat by typing / — slash command palette with popover, search, and action handlers in React via assistant-ui.
  Last updated: 2026-08-08

- [Text-to-Speech for Chat](https://www.assistant-ui.com/docs/guides/speech)
  Markdown: https://www.assistant-ui.com/docs/guides/speech.md
  Description: Read AI chat messages aloud with the Web Speech API or a custom TTS adapter. Speech synthesis for React chat UIs, integrated with assistant-ui.
  Last updated: 2026-08-08

- [Suggested Prompts](https://www.assistant-ui.com/docs/guides/suggestions)
  Markdown: https://www.assistant-ui.com/docs/guides/suggestions.md
  Description: Display suggested starter prompts in your AI chat to onboard users faster. Configurable suggestion components for React, built into assistant-ui.
  Last updated: 2026-08-08

- [Thread Virtualization](https://www.assistant-ui.com/docs/guides/virtualization)
  Markdown: https://www.assistant-ui.com/docs/guides/virtualization.md
  Description: Render very long threads with @tanstack/react-virtual, with ThreadPrimitive.Unstable_MessageById and ThreadPrimitive.MessageByIndex.
  Last updated: 2026-08-08

- [Realtime Voice Chat](https://www.assistant-ui.com/docs/guides/voice)
  Markdown: https://www.assistant-ui.com/docs/guides/voice.md
  Description: Build bidirectional voice conversations with AI in React. Realtime audio streaming, interruption handling, and visual state, integrated via assistant-ui.
  Last updated: 2026-08-08

- [Terminal AI Chat with Ink](https://www.assistant-ui.com/docs/ink)
  Markdown: https://www.assistant-ui.com/docs/ink.md
  Description: Build AI chat interfaces for the terminal in TypeScript with @assistant-ui/react-ink — streaming, tool calls, and keyboard navigation in CLI apps.
  Last updated: 2026-08-08

- [Adapters](https://www.assistant-ui.com/docs/ink/adapters)
  Markdown: https://www.assistant-ui.com/docs/ink/adapters.md
  Description: Attachment, title generation, and storage adapters for React Ink.
  Last updated: 2026-08-08

- [Custom Backend](https://www.assistant-ui.com/docs/ink/custom-backend)
  Markdown: https://www.assistant-ui.com/docs/ink/custom-backend.md
  Description: Connect your terminal app to your own backend API.
  Last updated: 2026-08-08

- [Hooks](https://www.assistant-ui.com/docs/ink/hooks)
  Markdown: https://www.assistant-ui.com/docs/ink/hooks.md
  Description: Reactive hooks for accessing runtime state in React Ink.
  Last updated: 2026-08-08

- [Migration from Web](https://www.assistant-ui.com/docs/ink/migration)
  Markdown: https://www.assistant-ui.com/docs/ink/migration.md
  Description: Migrate an existing @assistant-ui/react app to the terminal with React Ink.
  Last updated: 2026-08-08

- [Primitives](https://www.assistant-ui.com/docs/ink/primitives)
  Markdown: https://www.assistant-ui.com/docs/ink/primitives.md
  Description: Composable terminal components for building chat UIs with Ink.
  Last updated: 2026-08-08

- [Installation](https://www.assistant-ui.com/docs/installation)
  Markdown: https://www.assistant-ui.com/docs/installation.md
  Description: Get assistant-ui running in 5 minutes with npm and your first chat component.
  Last updated: 2026-08-08

- [Integrations](https://www.assistant-ui.com/docs/integrations)
  Markdown: https://www.assistant-ui.com/docs/integrations.md
  Description: Adapters for Vercel AI SDK, LangChain, LangGraph, Mastra, plus auth, persistence, observability, and tool services — drop into a React chat UI built with assistant-ui.
  Last updated: 2026-08-08

- [Custom attachment uploads](https://www.assistant-ui.com/docs/integrations/attachments/custom-adapter)
  Markdown: https://www.assistant-ui.com/docs/integrations/attachments/custom-adapter.md
  Description: Upload chat attachments to object storage with a presigned-URL AttachmentAdapter.
  Last updated: 2026-08-08

- [better-auth](https://www.assistant-ui.com/docs/integrations/auth/better-auth)
  Markdown: https://www.assistant-ui.com/docs/integrations/auth/better-auth.md
  Description: TypeScript-first auth with database-owned sessions; gate the chat route and scope threads to the signed-in user.
  Last updated: 2026-08-08

- [Clerk](https://www.assistant-ui.com/docs/integrations/auth/clerk)
  Markdown: https://www.assistant-ui.com/docs/integrations/auth/clerk.md
  Description: Gate the chat route and scope thread persistence to the signed-in user with Clerk.
  Last updated: 2026-08-08

- [Auth.js (next-auth)](https://www.assistant-ui.com/docs/integrations/auth/next-auth)
  Markdown: https://www.assistant-ui.com/docs/integrations/auth/next-auth.md
  Description: Gate the chat route and scope thread persistence to the signed-in user with Auth.js v5.
  Last updated: 2026-08-08

- [Vercel AI SDK Integration](https://www.assistant-ui.com/docs/integrations/frameworks/ai-sdk)
  Markdown: https://www.assistant-ui.com/docs/integrations/frameworks/ai-sdk.md
  Description: Wire the Vercel AI SDK into a React chat UI with assistant-ui — useChat, streaming, tools, attachments, multi-step agents, and persistence covered end-to-end.
  Last updated: 2026-08-08

- [Cloudflare Agents Integration](https://www.assistant-ui.com/docs/integrations/frameworks/cloudflare-agents/overview)
  Markdown: https://www.assistant-ui.com/docs/integrations/frameworks/cloudflare-agents/overview.md
  Description: Wire Cloudflare's stateful agent framework into a React chat UI with assistant-ui via the standard AI SDK runtime. WebSocket transport, server-side persistence, tool calling, all preserved.
  Last updated: 2026-08-08

- [Full-stack integration](https://www.assistant-ui.com/docs/integrations/frameworks/mastra/full-stack)
  Markdown: https://www.assistant-ui.com/docs/integrations/frameworks/mastra/full-stack.md
  Description: Run Mastra agents inside your Next.js API routes.
  Last updated: 2026-08-08

- [Mastra Integration](https://www.assistant-ui.com/docs/integrations/frameworks/mastra/overview)
  Markdown: https://www.assistant-ui.com/docs/integrations/frameworks/mastra/overview.md
  Description: Wire the Mastra TypeScript agent framework into a React chat UI with assistant-ui — full streaming, tool calling, multi-agent support, and thread management.
  Last updated: 2026-08-08

- [Separate server integration](https://www.assistant-ui.com/docs/integrations/frameworks/mastra/separate-server)
  Markdown: https://www.assistant-ui.com/docs/integrations/frameworks/mastra/separate-server.md
  Description: Run Mastra as a standalone server with assistant-ui as a separate frontend.
  Last updated: 2026-08-08

- [LLM Gateway Integrations](https://www.assistant-ui.com/docs/integrations/gateways)
  Markdown: https://www.assistant-ui.com/docs/integrations/gateways.md
  Description: Route AI chat traffic through OpenAI-compatible LLM gateways (OpenRouter, LiteLLM, Portkey, etc.) for cost, fallback, and BYOK in assistant-ui apps.
  Last updated: 2026-08-08

- [Helicone](https://www.assistant-ui.com/docs/integrations/observability/helicone)
  Markdown: https://www.assistant-ui.com/docs/integrations/observability/helicone.md
  Description: Log and monitor LLM calls by routing them through the Helicone proxy.
  Last updated: 2026-08-08

- [Langfuse](https://www.assistant-ui.com/docs/integrations/observability/langfuse)
  Markdown: https://www.assistant-ui.com/docs/integrations/observability/langfuse.md
  Description: Trace AI SDK calls into Langfuse via OpenTelemetry for tracing, evals, and prompt management.
  Last updated: 2026-08-08

- [LangSmith](https://www.assistant-ui.com/docs/integrations/observability/langsmith)
  Markdown: https://www.assistant-ui.com/docs/integrations/observability/langsmith.md
  Description: Trace AI SDK calls into LangSmith with the wrapAISDK helper.
  Last updated: 2026-08-08

- [Custom thread persistence](https://www.assistant-ui.com/docs/integrations/persistence/custom-adapter)
  Markdown: https://www.assistant-ui.com/docs/integrations/persistence/custom-adapter.md
  Description: Persist threads and messages to your own database with RemoteThreadListAdapter and ThreadHistoryAdapter.
  Last updated: 2026-08-08

- [Agent Skills](https://www.assistant-ui.com/docs/llm)
  Markdown: https://www.assistant-ui.com/docs/llm.md
  Description: Use AI tools to build with assistant-ui faster. AI-accessible documentation, Claude Code skills, and MCP integration.
  Last updated: 2026-08-08

- [Migration Guides](https://www.assistant-ui.com/docs/migrations)
  Markdown: https://www.assistant-ui.com/docs/migrations.md
  Description: Upgrade assistant-ui versions and migrate deprecated APIs and integrations.
  Last updated: 2026-08-08

- [Deprecation Policy](https://www.assistant-ui.com/docs/migrations/deprecation-policy)
  Markdown: https://www.assistant-ui.com/docs/migrations/deprecation-policy.md
  Description: Stability guarantees and deprecation timelines for assistant-ui features.
  Last updated: 2026-08-08

- [Using old React versions](https://www.assistant-ui.com/docs/migrations/react-compatibility)
  Markdown: https://www.assistant-ui.com/docs/migrations/react-compatibility.md
  Description: Compatibility notes for React 18 and 19.
  Last updated: 2026-08-08

- [Migrating to react-langgraph v0.7](https://www.assistant-ui.com/docs/migrations/react-langgraph-v0-7)
  Markdown: https://www.assistant-ui.com/docs/migrations/react-langgraph-v0-7.md
  Description: Guide to upgrading to the simplified LangGraph integration API.
  Last updated: 2026-08-08

- [Migrating Tools to Toolkits](https://www.assistant-ui.com/docs/migrations/toolkit-tools)
  Markdown: https://www.assistant-ui.com/docs/migrations/toolkit-tools.md
  Description: Move makeAssistantTool, useAssistantTool, makeAssistantToolUI, and useAssistantToolUI registrations to the toolkit API.
  Last updated: 2026-08-08

- [Migration to v0.11](https://www.assistant-ui.com/docs/migrations/v0-11)
  Markdown: https://www.assistant-ui.com/docs/migrations/v0-11.md
  Description: ContentPart renamed to MessagePart for better semantic clarity.
  Last updated: 2026-08-08

- [Migration to v0.12](https://www.assistant-ui.com/docs/migrations/v0-12)
  Markdown: https://www.assistant-ui.com/docs/migrations/v0-12.md
  Description: Unified state API replaces individual context hooks.
  Last updated: 2026-08-08

- [Migration to v0.14](https://www.assistant-ui.com/docs/migrations/v0-14)
  Markdown: https://www.assistant-ui.com/docs/migrations/v0-14.md
  Description: Drops APIs deprecated since v0.11/v0.12, and primitives migrate from components prop to children render functions.
  Last updated: 2026-08-08

- [Migration to v0.15](https://www.assistant-ui.com/docs/migrations/v0-15)
  Markdown: https://www.assistant-ui.com/docs/migrations/v0-15.md
  Description: Drops the v0.12-era legacy runtime hooks, the deprecated tools map, and the "mcp-app" group key. Scope accessors become properties.
  Last updated: 2026-08-08

- [Headless Chat Primitives](https://www.assistant-ui.com/docs/primitives)
  Markdown: https://www.assistant-ui.com/docs/primitives.md
  Description: Unstyled, accessible Radix-style building blocks for React AI chat interfaces — Thread, Composer, Message, and more, ready to compose with assistant-ui.
  Last updated: 2026-08-08

- [ActionBar](https://www.assistant-ui.com/docs/primitives/action-bar)
  Markdown: https://www.assistant-ui.com/docs/primitives/action-bar.md
  Description: Build message action buttons with auto-hide, copy state, and intelligent disabling.
  Last updated: 2026-08-08

- [AssistantModal](https://www.assistant-ui.com/docs/primitives/assistant-modal)
  Markdown: https://www.assistant-ui.com/docs/primitives/assistant-modal.md
  Description: A floating chat popover with a fixed-position trigger button that opens a chat panel.
  Last updated: 2026-08-08

- [Attachment](https://www.assistant-ui.com/docs/primitives/attachment)
  Markdown: https://www.assistant-ui.com/docs/primitives/attachment.md
  Description: File and image attachment rendering for the composer and messages.
  Last updated: 2026-08-08

- [BranchPicker](https://www.assistant-ui.com/docs/primitives/branch-picker)
  Markdown: https://www.assistant-ui.com/docs/primitives/branch-picker.md
  Description: Navigate between message branches, which are alternative responses the user can flip through.
  Last updated: 2026-08-08

- [ChainOfThought](https://www.assistant-ui.com/docs/primitives/chain-of-thought)
  Markdown: https://www.assistant-ui.com/docs/primitives/chain-of-thought.md
  Description: Collapsible accordion for grouping reasoning steps and tool calls.
  Last updated: 2026-08-08

- [Composer](https://www.assistant-ui.com/docs/primitives/composer)
  Markdown: https://www.assistant-ui.com/docs/primitives/composer.md
  Description: Build custom message input UIs with full control over layout and behavior.
  Last updated: 2026-08-08

- [Error](https://www.assistant-ui.com/docs/primitives/error)
  Markdown: https://www.assistant-ui.com/docs/primitives/error.md
  Description: Accessible error display for messages with automatic error text extraction.
  Last updated: 2026-08-08

- [Message](https://www.assistant-ui.com/docs/primitives/message)
  Markdown: https://www.assistant-ui.com/docs/primitives/message.md
  Description: Build custom message rendering with content parts, attachments, and hover state.
  Last updated: 2026-08-08

- [SelectionToolbar](https://www.assistant-ui.com/docs/primitives/selection-toolbar)
  Markdown: https://www.assistant-ui.com/docs/primitives/selection-toolbar.md
  Description: A floating toolbar that appears when text is selected within a message.
  Last updated: 2026-08-08

- [Suggestion](https://www.assistant-ui.com/docs/primitives/suggestion)
  Markdown: https://www.assistant-ui.com/docs/primitives/suggestion.md
  Description: Suggested prompts that users can click to quickly send or populate the composer.
  Last updated: 2026-08-08

- [Thread](https://www.assistant-ui.com/docs/primitives/thread)
  Markdown: https://www.assistant-ui.com/docs/primitives/thread.md
  Description: Build custom scrollable message containers with auto-scroll, empty states, and message rendering.
  Last updated: 2026-08-08

- [ThreadList](https://www.assistant-ui.com/docs/primitives/thread-list)
  Markdown: https://www.assistant-ui.com/docs/primitives/thread-list.md
  Description: Multi-thread management for listing, creating, switching, archiving, and deleting conversations.
  Last updated: 2026-08-08

- [React Native AI Chat](https://www.assistant-ui.com/docs/react-native)
  Markdown: https://www.assistant-ui.com/docs/react-native.md
  Description: Build AI chat for iOS and Android with @assistant-ui/react-native — streaming, tools, attachments, and platform-native components from the same primitives as the web SDK.
  Last updated: 2026-08-08

- [Adapters](https://www.assistant-ui.com/docs/react-native/adapters)
  Markdown: https://www.assistant-ui.com/docs/react-native/adapters.md
  Description: Persistence and title generation adapters for React Native.
  Last updated: 2026-08-08

- [Custom Backend](https://www.assistant-ui.com/docs/react-native/custom-backend)
  Markdown: https://www.assistant-ui.com/docs/react-native/custom-backend.md
  Description: Connect your React Native app to your own backend API.
  Last updated: 2026-08-08

- [Hooks](https://www.assistant-ui.com/docs/react-native/hooks)
  Markdown: https://www.assistant-ui.com/docs/react-native/hooks.md
  Description: Reactive hooks for accessing runtime state in React Native.
  Last updated: 2026-08-08

- [Migration from Web](https://www.assistant-ui.com/docs/react-native/migration)
  Markdown: https://www.assistant-ui.com/docs/react-native/migration.md
  Description: Migrate an existing @assistant-ui/react app to React Native.
  Last updated: 2026-08-08

- [Primitives](https://www.assistant-ui.com/docs/react-native/primitives)
  Markdown: https://www.assistant-ui.com/docs/react-native/primitives.md
  Description: Composable React Native components for building chat UIs.
  Last updated: 2026-08-08

- [RTL Support](https://www.assistant-ui.com/docs/rtl)
  Markdown: https://www.assistant-ui.com/docs/rtl.md
  Description: Use assistant-ui with right-to-left languages like Arabic, Hebrew, and Persian.
  Last updated: 2026-08-08

- [Client and hooks](https://www.assistant-ui.com/docs/runtimes/a2a/client-and-hooks)
  Markdown: https://www.assistant-ui.com/docs/runtimes/a2a/client-and-hooks.md
  Description: A2AClient, useA2ARuntime options, hooks, task states, artifacts, errors.
  Last updated: 2026-08-08

- [A2A Agent Runtime](https://www.assistant-ui.com/docs/runtimes/a2a/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/a2a/overview.md
  Description: Connect any A2A v1.0 protocol-compliant agent server to a React chat UI with assistant-ui — full streaming, tool calls, and message metadata supported.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/docs/runtimes/a2a/quickstart)
  Markdown: https://www.assistant-ui.com/docs/runtimes/a2a/quickstart.md
  Description: Minimal runtime and Thread setup against an A2A server.
  Last updated: 2026-08-08

- [Agent state](https://www.assistant-ui.com/docs/runtimes/ag-ui/agent-state)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ag-ui/agent-state.md
  Description: Read and optimistically update agent-owned state with useAgUiState and useAgUiSetState over AG-UI.
  Last updated: 2026-08-08

- [AG-UI Agent Runtime](https://www.assistant-ui.com/docs/runtimes/ag-ui/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ag-ui/overview.md
  Description: Wire AG-UI (Agent-User Interaction) protocol agents into a React chat UI with assistant-ui — bidirectional events, generative UI, and human-in-the-loop.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/docs/runtimes/ag-ui/quickstart)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ag-ui/quickstart.md
  Description: Minimal HttpAgent + useAgUiRuntime setup against an AG-UI server.
  Last updated: 2026-08-08

- [Runtime options](https://www.assistant-ui.com/docs/runtimes/ag-ui/runtime-options)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ag-ui/runtime-options.md
  Description: useAgUiRuntime options, adapters, supported events, thread list.
  Last updated: 2026-08-08

- [Vercel AI SDK Runtime](https://www.assistant-ui.com/docs/runtimes/ai-sdk/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ai-sdk/overview.md
  Description: Connect the Vercel AI SDK to a React chat UI via assistant-ui — useChat hooks, custom transports, frontend tools, attachments, multi-step agents, and token usage.
  Last updated: 2026-08-08

- [AI SDK v4 (legacy)](https://www.assistant-ui.com/docs/runtimes/ai-sdk/v4-legacy)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ai-sdk/v4-legacy.md
  Description: Reference for projects still on AI SDK v4. New projects should use v7.
  Last updated: 2026-08-08

- [AI SDK v5 (legacy)](https://www.assistant-ui.com/docs/runtimes/ai-sdk/v5-legacy)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ai-sdk/v5-legacy.md
  Description: Reference for projects still on AI SDK v5. New projects should use v7.
  Last updated: 2026-08-08

- [AI SDK v6 (legacy)](https://www.assistant-ui.com/docs/runtimes/ai-sdk/v6-legacy)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ai-sdk/v6-legacy.md
  Description: Reference for projects still on AI SDK v6. New projects should use v7.
  Last updated: 2026-08-08

- [AI SDK v7](https://www.assistant-ui.com/docs/runtimes/ai-sdk/v7)
  Markdown: https://www.assistant-ui.com/docs/runtimes/ai-sdk/v7.md
  Description: Integrate Vercel AI SDK v7 with assistant-ui for streaming chat.
  Last updated: 2026-08-08

- [Adapters](https://www.assistant-ui.com/docs/runtimes/concepts/adapters)
  Markdown: https://www.assistant-ui.com/docs/runtimes/concepts/adapters.md
  Description: Reusable extension points for attachments, speech, feedback, history, and suggestions.
  Last updated: 2026-08-08

- [Runtime architecture](https://www.assistant-ui.com/docs/runtimes/concepts/architecture)
  Markdown: https://www.assistant-ui.com/docs/runtimes/concepts/architecture.md
  Description: How core runtimes, protocol layers, and framework adapters fit together.
  Last updated: 2026-08-08

- [Stability](https://www.assistant-ui.com/docs/runtimes/concepts/stability)
  Markdown: https://www.assistant-ui.com/docs/runtimes/concepts/stability.md
  Description: What unstable_ means, when APIs become stable, and how to track changes.
  Last updated: 2026-08-08

- [Threads](https://www.assistant-ui.com/docs/runtimes/concepts/threads)
  Markdown: https://www.assistant-ui.com/docs/runtimes/concepts/threads.md
  Description: Single-thread, cloud, and custom-database thread management.
  Last updated: 2026-08-08

- [Assistant Transport](https://www.assistant-ui.com/docs/runtimes/custom/assistant-transport)
  Markdown: https://www.assistant-ui.com/docs/runtimes/custom/assistant-transport.md
  Description: Stream agent state to the frontend and handle user commands for custom agents.
  Last updated: 2026-08-08

- [Data Stream Protocol](https://www.assistant-ui.com/docs/runtimes/custom/data-stream)
  Markdown: https://www.assistant-ui.com/docs/runtimes/custom/data-stream.md
  Description: Standard message-streaming protocol on top of LocalRuntime.
  Last updated: 2026-08-08

- [ExternalStoreRuntime](https://www.assistant-ui.com/docs/runtimes/custom/external-store)
  Markdown: https://www.assistant-ui.com/docs/runtimes/custom/external-store.md
  Description: Bring your own redux, zustand, or state manager.
  Last updated: 2026-08-08

- [LocalRuntime](https://www.assistant-ui.com/docs/runtimes/custom/local-runtime)
  Markdown: https://www.assistant-ui.com/docs/runtimes/custom/local-runtime.md
  Description: Quickest path to a working chat. Handles state while you handle the API.
  Last updated: 2026-08-08

- [Custom Runtime](https://www.assistant-ui.com/docs/runtimes/custom/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/custom/overview.md
  Description: Build a React chat UI for any AI backend with assistant-ui — four runtime patterns covering local state, REST, custom protocols, and external runtimes.
  Last updated: 2026-08-08

- [Eve Runtime](https://www.assistant-ui.com/docs/runtimes/eve/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/eve/overview.md
  Description: Connect an Eve agent to assistant-ui with useEveAgentRuntime, eve/next, durable sessions, streaming messages, and human-in-the-loop approvals.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/docs/runtimes/eve/quickstart)
  Markdown: https://www.assistant-ui.com/docs/runtimes/eve/quickstart.md
  Description: Template, Eve CLI, and manual setup paths to a working Eve agent chat in assistant-ui.
  Last updated: 2026-08-08

- [API reference](https://www.assistant-ui.com/docs/runtimes/google-adk/api)
  Markdown: https://www.assistant-ui.com/docs/runtimes/google-adk/api.md
  Description: createAdkStream, server helpers, session adapter, threads, message editing.
  Last updated: 2026-08-08

- [Hooks](https://www.assistant-ui.com/docs/runtimes/google-adk/hooks)
  Markdown: https://www.assistant-ui.com/docs/runtimes/google-adk/hooks.md
  Description: Tool confirmations, auth, input requests, artifacts, escalation, metadata, structured events.
  Last updated: 2026-08-08

- [Google ADK Runtime](https://www.assistant-ui.com/docs/runtimes/google-adk/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/google-adk/overview.md
  Description: Connect Google's Agent Development Kit (ADK) to a React chat UI with assistant-ui — streaming, tool calls, and multi-agent orchestration supported.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/docs/runtimes/google-adk/quickstart)
  Markdown: https://www.assistant-ui.com/docs/runtimes/google-adk/quickstart.md
  Description: Minimal API route and client setup with createAdkApiRoute.
  Last updated: 2026-08-08

- [LangChain React Runtime](https://www.assistant-ui.com/docs/runtimes/langchain)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langchain.md
  Description: Use LangChain's useStream hook with a React chat UI through assistant-ui — a lighter LangGraph adapter that delegates streaming to @langchain/react.
  Last updated: 2026-08-08

- [Agent state](https://www.assistant-ui.com/docs/runtimes/langgraph/agent-state)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/agent-state.md
  Description: Read and optimistically update graph state with useLangGraphState and useLangGraphSetState in LangGraph.
  Last updated: 2026-08-08

- [LangGraph Generative UI](https://www.assistant-ui.com/docs/runtimes/langgraph/generative-ui)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/generative-ui.md
  Description: Render structured UI components emitted by LangGraph alongside assistant messages.
  Last updated: 2026-08-08

- [Interrupts and message editing](https://www.assistant-ui.com/docs/runtimes/langgraph/interrupts)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/interrupts.md
  Description: Interrupt persistence and checkpoint-based message editing.
  Last updated: 2026-08-08

- [LangGraph UI Runtime](https://www.assistant-ui.com/docs/runtimes/langgraph/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/overview.md
  Description: Build a chat UI for LangGraph agents in React with assistant-ui — streaming, subgraph events, UI messages, interrupts, and end-to-end cancellation supported.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/docs/runtimes/langgraph/quickstart)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/quickstart.md
  Description: From-template and manual setup paths to a working LangGraph chat.
  Last updated: 2026-08-08

- [Streaming](https://www.assistant-ui.com/docs/runtimes/langgraph/streaming)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/streaming.md
  Description: Event handlers, message accumulator, conversion, metadata, and generative UI.
  Last updated: 2026-08-08

- [Threads](https://www.assistant-ui.com/docs/runtimes/langgraph/threads)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/threads.md
  Description: Basic thread support, AssistantCloud, and custom thread list adapter.
  Last updated: 2026-08-08

- [Introduction](https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/introduction)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/introduction.md
  Description: Build a stockbroker assistant with LangGraph and assistant-ui.
  Last updated: 2026-08-08

- [Part 1: Setup frontend](https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/part-1)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/part-1.md
  Description: Create a Next.js project with the LangGraph assistant-ui template.
  Last updated: 2026-08-08

- [Part 2: Generative UI](https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/part-2)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/part-2.md
  Description: Display stock ticker information with generative UI components.
  Last updated: 2026-08-08

- [Part 3: Approval UI](https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/part-3)
  Markdown: https://www.assistant-ui.com/docs/runtimes/langgraph/tutorial/part-3.md
  Description: Add human-in-the-loop approval for tool calls.
  Last updated: 2026-08-08

- [Hooks](https://www.assistant-ui.com/docs/runtimes/opencode/hooks)
  Markdown: https://www.assistant-ui.com/docs/runtimes/opencode/hooks.md
  Description: Permissions, questions, session state, runtime extras.
  Last updated: 2026-08-08

- [OpenCode Runtime](https://www.assistant-ui.com/docs/runtimes/opencode/overview)
  Markdown: https://www.assistant-ui.com/docs/runtimes/opencode/overview.md
  Description: Build a React chat UI for OpenCode coding agents with assistant-ui — streaming, tool calls, file edits, and terminal output rendered in chat.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/docs/runtimes/opencode/quickstart)
  Markdown: https://www.assistant-ui.com/docs/runtimes/opencode/quickstart.md
  Description: Minimal useOpenCodeRuntime setup against a local OpenCode server.
  Last updated: 2026-08-08

- [Picking a runtime](https://www.assistant-ui.com/docs/runtimes/pick-a-runtime)
  Markdown: https://www.assistant-ui.com/docs/runtimes/pick-a-runtime.md
  Description: Decision guide for choosing the right runtime, by framework or by feature.
  Last updated: 2026-08-08

- [Tools](https://www.assistant-ui.com/docs/tools)
  Markdown: https://www.assistant-ui.com/docs/tools.md
  Description: Give the model callable capabilities with assistant-ui toolkits — define frontend, backend, human, and provider tools, render tool calls as interactive UI, and connect MCP servers.
  Last updated: 2026-08-08

- [A2UI over AG-UI](https://www.assistant-ui.com/docs/tools/a2ui)
  Markdown: https://www.assistant-ui.com/docs/tools/a2ui.md
  Description: Render A2UI surfaces from AG-UI activity snapshots as generative UI, using the community a2ui-surface convention.
  Last updated: 2026-08-08

- [Backend Tools](https://www.assistant-ui.com/docs/tools/backend)
  Markdown: https://www.assistant-ui.com/docs/tools/backend.md
  Description: Wire assistant-ui toolkits into your server with the AI SDK — AISDKToolkit, frontendTools, mixing client and server tools, and multi-modal results.
  Last updated: 2026-08-08

- [Defining Tools](https://www.assistant-ui.com/docs/tools/defining-tools)
  Markdown: https://www.assistant-ui.com/docs/tools/defining-tools.md
  Description: Define tools for your AI chat with assistant-ui toolkits and the "use generative" directive — frontend, backend, human, and provider tools with type safety and streaming.
  Last updated: 2026-08-08

- [Dynamic Tools](https://www.assistant-ui.com/docs/tools/dynamic-tools)
  Markdown: https://www.assistant-ui.com/docs/tools/dynamic-tools.md
  Description: Tools whose executor closes over React state — declare the contract with stubTool() in a "use generative" file and supply the executor with useAuiToolOverrides.
  Last updated: 2026-08-08

- [Generative UI (JSON spec)](https://www.assistant-ui.com/docs/tools/generative-ui)
  Markdown: https://www.assistant-ui.com/docs/tools/generative-ui.md
  Description: Render agent-described React UI from a JSON spec with a consumer-provided component allowlist.
  Last updated: 2026-08-08

- [Interactable Tool UIs](https://www.assistant-ui.com/docs/tools/interactables)
  Markdown: https://www.assistant-ui.com/docs/tools/interactables.md
  Description: Build stateful components and tool UIs that both the user and the model can read and edit. Render them beside the thread, or inside messages as versioned, editable surfaces like notepads and artifacts.
  Last updated: 2026-08-08

- [Interactables (legacy)](https://www.assistant-ui.com/docs/tools/interactables-legacy)
  Markdown: https://www.assistant-ui.com/docs/tools/interactables-legacy.md
  Description: Build persistent UI elements whose state the AI can read and update — copilot interactables in React with assistant-ui for forms, dashboards, and tools.
  Last updated: 2026-08-08

- [Model Context Protocol (MCP)](https://www.assistant-ui.com/docs/tools/mcp)
  Markdown: https://www.assistant-ui.com/docs/tools/mcp.md
  Description: Connect MCP servers as a tool catalog in your assistant-ui app.
  Last updated: 2026-08-08

- [MCP Apps](https://www.assistant-ui.com/docs/tools/mcp-apps)
  Markdown: https://www.assistant-ui.com/docs/tools/mcp-apps.md
  Description: Render MCP App UI resources inline in chat. Native renderer for the Model Context Protocol Apps spec — sandboxed iframes, JSON-RPC bridge, AI SDK integration.
  Last updated: 2026-08-08

- [Multi-Agent Chat UI](https://www.assistant-ui.com/docs/tools/multi-agent)
  Markdown: https://www.assistant-ui.com/docs/tools/multi-agent.md
  Description: Render sub-agent conversations and handoffs inside tool calls. Build supervisor and multi-agent patterns in a React chat UI with assistant-ui.
  Last updated: 2026-08-08

- [Tool UI](https://www.assistant-ui.com/docs/tools/tool-ui)
  Markdown: https://www.assistant-ui.com/docs/tools/tool-ui.md
  Description: Render AI tool calls as custom React components — show loading, result, and interactive states for each tool invocation in assistant-ui.
  Last updated: 2026-08-08

- [User-managed MCP servers](https://www.assistant-ui.com/docs/tools/user-managed-mcp)
  Markdown: https://www.assistant-ui.com/docs/tools/user-managed-mcp.md
  Description: Let end users add and authenticate MCP servers from the browser with @assistant-ui/react-mcp.
  Last updated: 2026-08-08

- [Accordion](https://www.assistant-ui.com/docs/ui/accordion)
  Markdown: https://www.assistant-ui.com/docs/ui/accordion.md
  Description: A vertically stacked set of interactive headings that reveal or hide content sections.
  Last updated: 2026-08-08

- [AssistantModal](https://www.assistant-ui.com/docs/ui/assistant-modal)
  Markdown: https://www.assistant-ui.com/docs/ui/assistant-modal.md
  Description: Floating chat bubble for support widgets and help desks.
  Last updated: 2026-08-08

- [AssistantSidebar](https://www.assistant-ui.com/docs/ui/assistant-sidebar)
  Markdown: https://www.assistant-ui.com/docs/ui/assistant-sidebar.md
  Description: Side panel chat for co-pilot experiences and inline assistance.
  Last updated: 2026-08-08

- [Attachment](https://www.assistant-ui.com/docs/ui/attachment)
  Markdown: https://www.assistant-ui.com/docs/ui/attachment.md
  Description: UI components for attaching and viewing files in messages.
  Last updated: 2026-08-08

- [Badge](https://www.assistant-ui.com/docs/ui/badge)
  Markdown: https://www.assistant-ui.com/docs/ui/badge.md
  Description: A small label component for displaying status, categories, or metadata.
  Last updated: 2026-08-08

- [Composer Trigger Popover](https://www.assistant-ui.com/docs/ui/composer-trigger-popover)
  Markdown: https://www.assistant-ui.com/docs/ui/composer-trigger-popover.md
  Description: Reusable picker UI for @ mentions, / slash commands, and any other character-triggered popover.
  Last updated: 2026-08-08

- [Context Display](https://www.assistant-ui.com/docs/ui/context-display)
  Markdown: https://www.assistant-ui.com/docs/ui/context-display.md
  Description: Visualize token usage relative to a model's context window — ring, bar, or text — with a detailed hover popover.
  Last updated: 2026-08-08

- [Diff Viewer](https://www.assistant-ui.com/docs/ui/diff-viewer)
  Markdown: https://www.assistant-ui.com/docs/ui/diff-viewer.md
  Description: Render code diffs with syntax highlighting for additions and deletions.
  Last updated: 2026-08-08

- [Directive Text](https://www.assistant-ui.com/docs/ui/directive-text)
  Markdown: https://www.assistant-ui.com/docs/ui/directive-text.md
  Description: Render mention directives as inline chips in user messages.
  Last updated: 2026-08-08

- [Dot Matrix](https://www.assistant-ui.com/docs/ui/dot-matrix)
  Markdown: https://www.assistant-ui.com/docs/ui/dot-matrix.md
  Description: Tiny 5x5 dot-matrix indicator with 20 state-specific blink patterns.
  Last updated: 2026-08-08

- [File](https://www.assistant-ui.com/docs/ui/file)
  Markdown: https://www.assistant-ui.com/docs/ui/file.md
  Description: Display file message parts with icon, name, size, and download button.
  Last updated: 2026-08-08

- [Follow-Up Suggestions](https://www.assistant-ui.com/docs/ui/follow-up-suggestions)
  Markdown: https://www.assistant-ui.com/docs/ui/follow-up-suggestions.md
  Description: Render runtime-generated follow-up prompt chips after an assistant response.
  Last updated: 2026-08-08

- [Image](https://www.assistant-ui.com/docs/ui/image)
  Markdown: https://www.assistant-ui.com/docs/ui/image.md
  Description: Display image message parts with preview, loading states, and fullscreen dialog.
  Last updated: 2026-08-08

- [Markdown](https://www.assistant-ui.com/docs/ui/markdown)
  Markdown: https://www.assistant-ui.com/docs/ui/markdown.md
  Description: Display rich text with headings, lists, links, and code blocks.
  Last updated: 2026-08-08

- [MCP Config Dialog](https://www.assistant-ui.com/docs/ui/mcp-config)
  Markdown: https://www.assistant-ui.com/docs/ui/mcp-config.md
  Description: Drop-in shadcn dialog that lists MCP connectors and custom servers, with inline OAuth/bearer auth controls and an add form.
  Last updated: 2026-08-08

- [Mermaid Diagrams](https://www.assistant-ui.com/docs/ui/mermaid)
  Markdown: https://www.assistant-ui.com/docs/ui/mermaid.md
  Description: Render Mermaid diagrams in chat messages with streaming support.
  Last updated: 2026-08-08

- [Message Timing](https://www.assistant-ui.com/docs/ui/message-timing)
  Markdown: https://www.assistant-ui.com/docs/ui/message-timing.md
  Description: Display streaming performance stats — TTFT, total time, tok/s, and chunk count — as a badge with hover popover.
  Last updated: 2026-08-08

- [Model Selector](https://www.assistant-ui.com/docs/ui/model-selector)
  Markdown: https://www.assistant-ui.com/docs/ui/model-selector.md
  Description: Composable model picker with reasoning effort levels, search, and runtime integration.
  Last updated: 2026-08-08

- [Number Roll](https://www.assistant-ui.com/docs/ui/number-roll)
  Markdown: https://www.assistant-ui.com/docs/ui/number-roll.md
  Description: Animated number that rolls digits odometer-style when the value changes.
  Last updated: 2026-08-08

- [Message Part Grouping](https://www.assistant-ui.com/docs/ui/part-grouping)
  Markdown: https://www.assistant-ui.com/docs/ui/part-grouping.md
  Description: Organize message parts into custom groups with flexible grouping functions.
  Last updated: 2026-08-08

- [Quote](https://www.assistant-ui.com/docs/ui/quote)
  Markdown: https://www.assistant-ui.com/docs/ui/quote.md
  Description: Let users select and quote text from messages with a floating toolbar, composer preview, and inline quote display.
  Last updated: 2026-08-08

- [Reasoning](https://www.assistant-ui.com/docs/ui/reasoning)
  Markdown: https://www.assistant-ui.com/docs/ui/reasoning.md
  Description: Collapsible UI for displaying AI reasoning and thinking messages.
  Last updated: 2026-08-08

- [Custom Scrollbar](https://www.assistant-ui.com/docs/ui/scrollbar)
  Markdown: https://www.assistant-ui.com/docs/ui/scrollbar.md
  Description: Replace the default scrollbar with a custom Radix UI scroll area.
  Last updated: 2026-08-08

- [Select](https://www.assistant-ui.com/docs/ui/select)
  Markdown: https://www.assistant-ui.com/docs/ui/select.md
  Description: A dropdown select component with composable sub-components.
  Last updated: 2026-08-08

- [Sources](https://www.assistant-ui.com/docs/ui/sources)
  Markdown: https://www.assistant-ui.com/docs/ui/sources.md
  Description: Display URL sources with favicon, title, and external link.
  Last updated: 2026-08-08

- [Streamdown Markdown Renderer](https://www.assistant-ui.com/docs/ui/streamdown)
  Markdown: https://www.assistant-ui.com/docs/ui/streamdown.md
  Description: Stream markdown into a React chat UI with syntax highlighting, math, and Mermaid diagrams. Powered by Vercel Streamdown, integrated for assistant-ui.
  Last updated: 2026-08-08

- [Syntax Highlighting](https://www.assistant-ui.com/docs/ui/syntax-highlighting)
  Markdown: https://www.assistant-ui.com/docs/ui/syntax-highlighting.md
  Description: Code block syntax highlighting with react-shiki or react-syntax-highlighter.
  Last updated: 2026-08-08

- [Tabs](https://www.assistant-ui.com/docs/ui/tabs)
  Markdown: https://www.assistant-ui.com/docs/ui/tabs.md
  Description: A multi-variant tabs component for organizing content into switchable panels.
  Last updated: 2026-08-08

- [Thread Component](https://www.assistant-ui.com/docs/ui/thread)
  Markdown: https://www.assistant-ui.com/docs/ui/thread.md
  Description: Stream-ready React chat container with message list, composer, auto-scroll, and accessibility built in. Drop into any AI chat UI built with assistant-ui.
  Last updated: 2026-08-08

- [Thread List Component](https://www.assistant-ui.com/docs/ui/thread-list)
  Markdown: https://www.assistant-ui.com/docs/ui/thread-list.md
  Description: Sidebar or dropdown component for switching between AI chat conversations. Persistent thread state, search, and active selection — built for assistant-ui apps.
  Last updated: 2026-08-08

- [ToolFallback](https://www.assistant-ui.com/docs/ui/tool-fallback)
  Markdown: https://www.assistant-ui.com/docs/ui/tool-fallback.md
  Description: Default UI component for tools without dedicated custom renderers.
  Last updated: 2026-08-08

- [ToolGroup](https://www.assistant-ui.com/docs/ui/tool-group)
  Markdown: https://www.assistant-ui.com/docs/ui/tool-group.md
  Description: Wrapper for consecutive tool calls with collapsible and styled options.
  Last updated: 2026-08-08

- [Voice](https://www.assistant-ui.com/docs/ui/voice)
  Markdown: https://www.assistant-ui.com/docs/ui/voice.md
  Description: Realtime voice session controls with connect, mute, and status indicator.
  Last updated: 2026-08-08

- [heat-graph](https://www.assistant-ui.com/docs/utilities/heat-graph)
  Markdown: https://www.assistant-ui.com/docs/utilities/heat-graph.md
  Description: Headless, composable activity heatmap components for React.
  Last updated: 2026-08-08

- [react-o11y](https://www.assistant-ui.com/docs/utilities/react-o11y)
  Markdown: https://www.assistant-ui.com/docs/utilities/react-o11y.md
  Description: Headless primitives for visualizing observability spans as collapsible trace trees, waterfalls, and agent and LLM call timelines.
  Last updated: 2026-08-08

- [tw-shimmer](https://www.assistant-ui.com/docs/utilities/tw-shimmer)
  Markdown: https://www.assistant-ui.com/docs/utilities/tw-shimmer.md
  Description: Tailwind CSS v4 plugin for shimmer effects.
  Last updated: 2026-08-08

## Tap documentation

- [Introduction](https://www.assistant-ui.com/tap/docs/overview/introduction)
  Markdown: https://www.assistant-ui.com/tap/docs/overview/introduction.md
  Description: State management based on React Hooks.
  Last updated: 2026-08-08

- [Motivation](https://www.assistant-ui.com/tap/docs/overview/motivation)
  Markdown: https://www.assistant-ui.com/tap/docs/overview/motivation.md
  Description: Composable configuration and state built on React's lifecycle.
  Last updated: 2026-08-08

- [API Reference](https://www.assistant-ui.com/tap/docs/store/api-reference)
  Markdown: https://www.assistant-ui.com/tap/docs/store/api-reference.md
  Description: All exports from @assistant-ui/store.
  Last updated: 2026-08-08

- [Child Scopes](https://www.assistant-ui.com/tap/docs/store/child-scopes)
  Markdown: https://www.assistant-ui.com/tap/docs/store/child-scopes.md
  Description: Derive scopes from parent data with Derived, useClientResource, and useClientLookup.
  Last updated: 2026-08-08

- [Events](https://www.assistant-ui.com/tap/docs/store/events)
  Markdown: https://www.assistant-ui.com/tap/docs/store/events.md
  Description: Emit and subscribe to typed events.
  Last updated: 2026-08-08

- [Meta](https://www.assistant-ui.com/tap/docs/store/meta)
  Markdown: https://www.assistant-ui.com/tap/docs/store/meta.md
  Description: Track scope origin with source and query.
  Last updated: 2026-08-08

- [Methods](https://www.assistant-ui.com/tap/docs/store/methods)
  Markdown: https://www.assistant-ui.com/tap/docs/store/methods.md
  Description: Access scope methods with useAui.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/tap/docs/store/quickstart)
  Markdown: https://www.assistant-ui.com/tap/docs/store/quickstart.md
  Description: Install Store and connect your first Tap resource to React.
  Last updated: 2026-08-08

- [Rendering Lists](https://www.assistant-ui.com/tap/docs/store/rendering-lists)
  Markdown: https://www.assistant-ui.com/tap/docs/store/rendering-lists.md
  Description: How to efficiently render lists of items from store state.
  Last updated: 2026-08-08

- [Scopes](https://www.assistant-ui.com/tap/docs/store/scopes)
  Markdown: https://www.assistant-ui.com/tap/docs/store/scopes.md
  Description: Named, independent units of state in your store.
  Last updated: 2026-08-08

- [Sibling Scopes](https://www.assistant-ui.com/tap/docs/store/sibling-scopes)
  Markdown: https://www.assistant-ui.com/tap/docs/store/sibling-scopes.md
  Description: Scopes that reference each other at the same level.
  Last updated: 2026-08-08

- [State](https://www.assistant-ui.com/tap/docs/store/state)
  Markdown: https://www.assistant-ui.com/tap/docs/store/state.md
  Description: Subscribe to state changes with useAuiState.
  Last updated: 2026-08-08

- [Why Store](https://www.assistant-ui.com/tap/docs/store/why-store)
  Markdown: https://www.assistant-ui.com/tap/docs/store/why-store.md
  Description: The problem Store solves.
  Last updated: 2026-08-08

- [API Reference](https://www.assistant-ui.com/tap/docs/tap/api-reference)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/api-reference.md
  Description: Public API exported from @assistant-ui/tap.
  Last updated: 2026-08-08

- [Composition](https://www.assistant-ui.com/tap/docs/tap/composition)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/composition.md
  Description: Combine Resources into reusable state trees.
  Last updated: 2026-08-08

- [Context](https://www.assistant-ui.com/tap/docs/tap/context)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/context.md
  Description: Pass values through resource boundaries without prop drilling.
  Last updated: 2026-08-08

- [How tap differs from React](https://www.assistant-ui.com/tap/docs/tap/differences-from-react)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/differences-from-react.md
  Description: The handful of behaviors that aren't quite React.
  Last updated: 2026-08-08

- [Lifecycle](https://www.assistant-ui.com/tap/docs/tap/lifecycle)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/lifecycle.md
  Description: How resources render, mount, update, and unmount.
  Last updated: 2026-08-08

- [Outside React](https://www.assistant-ui.com/tap/docs/tap/outside-react)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/outside-react.md
  Description: Run resources standalone, with no React tree.
  Last updated: 2026-08-08

- [Quickstart](https://www.assistant-ui.com/tap/docs/tap/quickstart)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/quickstart.md
  Description: Install tap and build your first Resource.
  Last updated: 2026-08-08

- [Resources](https://www.assistant-ui.com/tap/docs/tap/resources)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/resources.md
  Description: Package stateful behavior into reusable, configurable values.
  Last updated: 2026-08-08

- [Trees & Re-renders](https://www.assistant-ui.com/tap/docs/tap/trees-and-rerenders)
  Markdown: https://www.assistant-ui.com/tap/docs/tap/trees-and-rerenders.md
  Description: How resource trees re-render and where scheduling boundaries form.
  Last updated: 2026-08-08

## Examples

- [Examples](https://www.assistant-ui.com/examples)
  Markdown: https://www.assistant-ui.com/examples.md
  Description: Production-ready examples of AI chat in React — ChatGPT clones, copilots, generative UI, artifacts, multimodal, and more, all built with assistant-ui.
  Last updated: 2026-08-08

- [AI SDK Chat Persistence](https://www.assistant-ui.com/examples/ai-sdk)
  Markdown: https://www.assistant-ui.com/examples/ai-sdk.md
  Description: Vercel AI SDK chat with thread persistence — open-source React example combining the AI SDK and assistant-ui for streaming, thread management, and message history.
  Last updated: 2026-08-08

- [Claude Artifacts Example](https://www.assistant-ui.com/examples/artifacts)
  Markdown: https://www.assistant-ui.com/examples/artifacts.md
  Description: Open-source Claude Artifacts implementation in React — generate websites and components in a side panel from chat messages, built on assistant-ui.
  Last updated: 2026-08-08

- [ChatGPT Clone Example](https://www.assistant-ui.com/examples/chatgpt)
  Markdown: https://www.assistant-ui.com/examples/chatgpt.md
  Description: Open-source ChatGPT clone built in React with assistant-ui — centered welcome composer, high-contrast user bubbles, tooltipped controls, and full assistant action bar.
  Last updated: 2026-08-08

- [Claude Clone](https://www.assistant-ui.com/examples/claude)
  Markdown: https://www.assistant-ui.com/examples/claude.md
  Description: Open-source Claude clone in React — warm cream theme, serif typography, hover-only action bars, and a clean minimal-shadow composer styled after claude.ai.
  Last updated: 2026-08-08

- [Expo React Native AI Chat](https://www.assistant-ui.com/examples/expo)
  Markdown: https://www.assistant-ui.com/examples/expo.md
  Description: Native iOS and Android AI chat app with Expo — drawer navigation, thread persistence, and the assistant-ui React Native components for mobile.
  Last updated: 2026-08-08

- [Form-Filling AI Copilot](https://www.assistant-ui.com/examples/form-demo)
  Markdown: https://www.assistant-ui.com/examples/form-demo.md
  Description: Open-source AI copilot that fills forms for users — sidebar UI, field-aware tool calls, and a working React example built with assistant-ui.
  Last updated: 2026-08-08

- [Gemini Clone](https://www.assistant-ui.com/examples/gemini)
  Markdown: https://www.assistant-ui.com/examples/gemini.md
  Description: Open-source Gemini clone in React with a centered greeting over an ambient glow, a single-row pill composer, avatar-free assistant replies, and disabled, ready, and stop send states.
  Last updated: 2026-08-08

- [Generative UI Example (Tool UI)](https://www.assistant-ui.com/examples/generative-ui)
  Markdown: https://www.assistant-ui.com/examples/generative-ui.md
  Description: Live demo of toolkit Tool UI patterns — charts, date pickers, contact forms, and maps. For the GenerativeUI JSON-spec primitive, see /gui-chat and /primitive in the same example app.
  Last updated: 2026-08-08

- [Grok Clone](https://www.assistant-ui.com/examples/grok)
  Markdown: https://www.assistant-ui.com/examples/grok.md
  Description: Open-source Grok clone in React — pill composer with paperclip, animated Mic↔Send, functional model picker dropdown, and message timing tooltip.
  Last updated: 2026-08-08

- [Mem0 Memory Chat](https://www.assistant-ui.com/examples/mem0)
  Markdown: https://www.assistant-ui.com/examples/mem0.md
  Description: AI chat with persistent memory powered by Mem0 — remembers user preferences, facts, and history across sessions. Open-source React example built on assistant-ui.
  Last updated: 2026-08-08

- [Floating Modal Chat](https://www.assistant-ui.com/examples/modal)
  Markdown: https://www.assistant-ui.com/examples/modal.md
  Description: Embeddable AI assistant in a floating button modal — drop into any React app for in-product copilots or support chat, built on assistant-ui.
  Last updated: 2026-08-08

- [Perplexity Clone](https://www.assistant-ui.com/examples/perplexity)
  Markdown: https://www.assistant-ui.com/examples/perplexity.md
  Description: Open-source Perplexity-style chat in React — theme-aware composer, functional Search and Model dropdowns, four-state primary action, and a Search-icon assistant avatar.
  Last updated: 2026-08-08

- [LangGraph Stockbroker Demo](https://www.assistant-ui.com/examples/stockbroker)
  Markdown: https://www.assistant-ui.com/examples/stockbroker.md
  Description: Human-in-the-loop AI stockbroker built on LangGraph and assistant-ui — interrupt handling, tool approval, and an interactive React chat UI.
  Last updated: 2026-08-08
