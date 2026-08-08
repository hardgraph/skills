# API Reference
URL: /docs/api-reference/overview

Complete assistant-ui React API reference for building AI chat UIs with primitives, hooks, runtimes, adapters, tools, transport, voice, and integrations.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Start Here

The React API reference is organized by how you build with assistant-ui:

- [Tools](/docs/api-reference/tools) —

  Define toolkits, install tool resources, and render tool or data parts.

- [Model Context](/docs/api-reference/model-context/context) —

  Provide instructions, merge model context, and register inline renderers for model-facing state.

- [Transport](/docs/api-reference/transport/assistant-transport) —

  Assistant transport, frame provider, and serialized frame protocol APIs for cross-boundary runtimes.

- [External Store](/docs/api-reference/external-store/runtime) —

  Runtime and message conversion APIs for apps that own their message store.

- [Voice](/docs/api-reference/voice/session) —

  Realtime voice sessions, speech synthesis adapters, and dictation adapters.

- [Primitives](/docs/api-reference/primitives/thread) —

  Composable React components such as `ThreadPrimitive`, `MessagePrimitive`, and `ComposerPrimitive`.

- [Hooks](/docs/api-reference/hooks/state) —

  Reactive state hooks, runtime hooks, model context hooks, and utility hooks.

- [Adapters](/docs/api-reference/adapters/model) —

  Runtime extension points for attachments, persistence, feedback, speech, dictation, and suggestions.

- [Integrations](/docs/api-reference/integrations/react-ai-sdk) —

  Package-level APIs for integrations such as `@assistant-ui/react-ai-sdk` and `@assistant-ui/cloud-ai-sdk`.

- [Runtime State](/docs/api-reference/runtimes/assistant-runtime) —

  Runtime action and state shapes used by `useAui` and `useAuiState`.

The lower sections on this page show how the core React contexts nest and where the primary primitives, hooks, and runtime objects are available. Feature-first APIs such as tools, model context, transport, external store, and voice now live in their own generated sections.

## Highest Level Context Providers

- name

  AssistantRuntimeProvider

- providedContexts

  - - name

      Assistant Context

    - color

      \#4a86e8

  - - name

      Thread Context

    - color

      \#45a049

  - - name

      Thread Composer Context

    - color

      \#ff9933

    - link

      \#composer-context

- docsLink

  /docs/api-reference/context-providers/assistant-runtime-provider

- tooltip

  Provides the highest level context for the assistant-ui

- props

  runtime={runtime}

* name

  ThreadPrimitive

* docsLink

  /docs/api-reference/primitives/thread

* tooltip

  Thread primitive components

- name

  ThreadListPrimitive

- docsLink

  /docs/api-reference/primitives/thread-list

- tooltip

  Thread list primitive components

* name

  TextMessagePartProvider

* providedContexts

  - - name

      Text MessagePart Context

    - color

      \#268bd2

    - link

      \#messagepart-context

* docsLink

  /docs/api-reference/context-providers/scoped-providers

* tooltip

  Provides context for text message parts

* props

  text={text}

- name

  MessagePartPrimitive

- docsLink

  /docs/api-reference/primitives/message-part

- tooltip

  Message part primitive components

* color

  \#4a86e8

## Assistant Context

The context available to components inside `<AssistantRuntimeProvider />`. You usually wrap your entire application in this context.

### AssistantRuntime

Programmatically access the assistant's state and actions via `useAui()`.

### Instructions

Add system prompt instructions

- [`useAssistantInstructions`](/docs/api-reference/model-context/context)

### Tool UI

Register toolkits and tool UIs

- [`Toolkit`](/docs/api-reference/tools/toolkits#toolkit)
- [`Tools`](/docs/api-reference/tools/toolkits#tools)

Programmatically access the list of registered tool UIs via `useAuiState((s) => s.tools)` and `useAui().tools()`.

### ThreadListPrimitive

Shows a list of threads and allows the user to switch between them.

- name

  ThreadListPrimitive.Root

- docsLink

  /docs/api-reference/primitives/thread-list

- tooltip

  Root component for the thread list

* name

  ThreadListPrimitive.New

* docsLink

  /docs/api-reference/primitives/thread-list

* tooltip

  Component for creating a new thread

- name

  ThreadListPrimitive.Items

- providedContexts

  - - name

      ThreadListItem Context

    - color

      \#c678dd

- docsLink

  /docs/api-reference/primitives/thread-list

- tooltip

  Container for thread list items, provides context for individual items

- props

  components={...}

* color

  \#45a049

## Thread Context

The context for a single thread. Currently always corresponds to the runtime's main thread.

### ThreadRuntime

Programmatically access the thread's state and actions.

- State: `useAuiState((s) => s.thread)`
- Actions: `useAui().thread()`
- Composer: `useAuiState((s) => s.composer)`

### ModelContext

- `useAui().thread().getModelContext()`

### ThreadViewportStore

- [`useThreadViewport`](/docs/api-reference/hooks/primitives)
- [`useThreadViewportStore`](/docs/api-reference/hooks/primitives)

### ThreadPrimitive

A conversation thread.

- name

  ThreadPrimitive.Root

- docsLink

  /docs/api-reference/primitives/thread

- tooltip

  Root component for a thread

* name

  ThreadPrimitive.Viewport

* docsLink

  /docs/api-reference/primitives/thread

* tooltip

  Viewport for the thread content

- name

  ThreadPrimitive.Messages

- providedContexts

  - - name

      Message Context

    - color

      \#bb2244

  - - name

      Edit Composer Context

    - color

      \#ff9933

    - link

      \#composer-context

- docsLink

  /docs/api-reference/primitives/thread

- tooltip

  Container for thread messages, provides context for messages and edit composer

* name

  MessagePrimitive

* docsLink

  /docs/api-reference/primitives/message

* tooltip

  Message primitive components

- name

  ThreadPrimitive.ScrollToBottom

- docsLink

  /docs/api-reference/primitives/thread

- tooltip

  Scrolls to the bottom of the thread

* name

  AuiIf

* docsLink

  /docs/api-reference/primitives/assistant-if

* tooltip

  Conditional rendering based on assistant state

- name

  ThreadPrimitive.Suggestion

- docsLink

  /docs/api-reference/primitives/thread

- tooltip

  Displays suggestions in the thread

* name

  ComposerPrimitive

* docsLink

  /docs/api-reference/primitives/composer

* tooltip

  Composer primitive components

### AssistantModalPrimitive

A floating modal that usually appears in the lower right corner of the screen. Common for support use cases.

- name

  AssistantModalPrimitive.Root

- docsLink

  /docs/api-reference/primitives/assistant-modal

- tooltip

  Root component for the assistant modal

* name

  AssistantModalPrimitive.Trigger

* docsLink

  /docs/api-reference/primitives/assistant-modal

* tooltip

  Trigger to open the assistant modal

- name

  AssistantModalPrimitive.Anchor

- docsLink

  /docs/api-reference/primitives/assistant-modal

- tooltip

  Anchor point for the assistant modal

* name

  AssistantModalPrimitive.Content

* docsLink

  /docs/api-reference/primitives/assistant-modal

* tooltip

  Content of the assistant modal

- color

  \#ff9933

## Composer Context

Manages the state and actions for the message composer

### ComposerRuntime

- State: `useAuiState((s) => s.composer)`
- Actions: `useAui().composer()`

### ComposerPrimitive

- name

  ComposerPrimitive.Root

- docsLink

  /docs/api-reference/primitives/composer

- tooltip

  Root component for the composer

* name

  ComposerPrimitive.Input

* docsLink

  /docs/api-reference/primitives/composer

* tooltip

  Input field for composing messages

- name

  ComposerPrimitive.Send

- docsLink

  /docs/api-reference/primitives/composer

- tooltip

  Button to send the composed message

* name

  ComposerPrimitive.Cancel

* docsLink

  /docs/api-reference/primitives/composer

* tooltip

  Button to cancel composing

- name

  ComposerPrimitive.Attachments

- providedContexts

  - - name

      Attachment Context

    - color

      \#FFB6C1

- docsLink

  /docs/api-reference/primitives/composer

- tooltip

  Manages attachments in the composer

* name

  ComposerPrimitive.AddAttachment

* docsLink

  /docs/api-reference/primitives/composer

* tooltip

  Button to add an attachment

- color

  \#bb2244

## Message Context

Manages the state and actions for individual messages

### MessageRuntime

- State: `useAuiState((s) => s.message)`
- Edit Composer: `useAuiState((s) => s.message.composer)`
- Actions: `useAui().message()`
- Utilities: `useAuiState((s) => s.message.isCopied)` / `useAuiState((s) => s.message.isHovering)`

### MessagePrimitive

- name

  MessagePrimitive.Root

- docsLink

  /docs/api-reference/primitives/message

- tooltip

  Root component for a message

* name

  MessagePrimitive.Parts

* providedContexts

  - - name

      MessagePart Context

    - color

      \#268bd2

* docsLink

  /docs/api-reference/primitives/message

* tooltip

  Displays the parts of the message

- name

  MessagePrimitive.Attachments

- providedContexts

  - - name

      Attachment Context

    - color

      \#FFB6C1

- docsLink

  /docs/api-reference/primitives/message

- tooltip

  Displays attachments in the message

* name

  AuiIf

* docsLink

  /docs/api-reference/primitives/assistant-if

* tooltip

  Conditional rendering based on assistant state

### ActionBarPrimitive

- name

  ActionBarPrimitive.Root

- docsLink

  /docs/api-reference/primitives/action-bar

- tooltip

  Root component for the action bar

* name

  ActionBarPrimitive.Copy

* docsLink

  /docs/api-reference/primitives/action-bar

* tooltip

  Copies the message content

- name

  ActionBarPrimitive.Edit

- docsLink

  /docs/api-reference/primitives/action-bar

- tooltip

  Edits the message

* name

  ActionBarPrimitive.Reload

* docsLink

  /docs/api-reference/primitives/action-bar

* tooltip

  Reloads the message

- name

  ActionBarPrimitive.Speak

- docsLink

  /docs/api-reference/primitives/action-bar

- tooltip

  Speaks the message content

* name

  ActionBarPrimitive.StopSpeaking

* docsLink

  /docs/api-reference/primitives/action-bar

* tooltip

  Stops speaking the message

- name

  ActionBarPrimitive.FeedbackPositive

- docsLink

  /docs/api-reference/primitives/action-bar

- tooltip

  Provides positive feedback

* name

  ActionBarPrimitive.FeedbackNegative

* docsLink

  /docs/api-reference/primitives/action-bar

* tooltip

  Provides negative feedback

### BranchPickerPrimitive

- name

  BranchPickerPrimitive.Root

- docsLink

  /docs/api-reference/primitives/branch-picker

- tooltip

  Root component for the branch picker

* name

  BranchPickerPrimitive.Previous

* docsLink

  /docs/api-reference/primitives/branch-picker

* tooltip

  Navigates to the previous branch

- name

  BranchPickerPrimitive.Number

- docsLink

  /docs/api-reference/primitives/branch-picker

- tooltip

  Displays the current branch number

* name

  BranchPickerPrimitive.Next

* docsLink

  /docs/api-reference/primitives/branch-picker

* tooltip

  Navigates to the next branch

- color

  \#268bd2

## MessagePart Context

Manages the state and actions for message parts within messages

### MessagePartRuntime

- State: `useAuiState((s) => s.part)`
- Actions: `useAui().part()`

### MessagePartPrimitive

- name

  MessagePartPrimitive.Text

- docsLink

  /docs/api-reference/primitives/message-part

- tooltip

  Represents a text part of the message content

### MarkdownText

- name

  MarkdownText

- docsLink

  /docs/api-reference/primitives/message-part

- tooltip

  Renders markdown text in the message

* color

  \#FFB6C1

## Attachment Context

Manages the state and actions for attachments in messages and composer

### AttachmentRuntime

- State: `useAuiState((s) => s.attachment)`
- Actions: `useAui().attachment()`

### AttachmentPrimitive

- name

  AttachmentPrimitive.Root

- docsLink

  /docs/api-reference/primitives/attachment

- tooltip

  Root component for an attachment

* name

  AttachmentPrimitive.Name

* docsLink

  /docs/api-reference/primitives/attachment

* tooltip

  Displays the name of the attachment

- name

  AttachmentPrimitive.Remove

- docsLink

  /docs/api-reference/primitives/attachment

- tooltip

  Removes the attachment

* name

  AttachmentPrimitive.unstable\_Thumb

* docsLink

  /docs/api-reference/primitives/attachment

* tooltip

  Displays a thumbnail of the attachment

- color

  \#c678dd

## ThreadListItem Context

Manages the state and actions for individual thread list items

### ThreadListItemRuntime

- State: `useAuiState((s) => s.threadListItem)`
- Actions: `useAui().threadListItem()`

### ThreadListItem

- name

  ThreadListItemPrimitive.Root

- docsLink

  /docs/api-reference/primitives/thread-list-item

- tooltip

  Root component for a thread list item

* name

  ThreadListItemPrimitive.Trigger

* docsLink

  /docs/api-reference/primitives/thread-list-item

* tooltip

  Trigger for thread list item actions

- name

  ThreadListItemPrimitive.Title

- docsLink

  /docs/api-reference/primitives/thread-list-item

- tooltip

  Displays the title of the thread

* name

  ThreadListItemPrimitive.Archive

* docsLink

  /docs/api-reference/primitives/thread-list-item

* tooltip

  Archives the thread

- name

  ThreadListItemPrimitive.Unarchive

- docsLink

  /docs/api-reference/primitives/thread-list-item

- tooltip

  Unarchives the thread

* name

  ThreadListItemPrimitive.Delete

* docsLink

  /docs/api-reference/primitives/thread-list-item

* tooltip

  Deletes the thread

## Utilities

- [`useThreadViewportAutoScroll`](/docs/api-reference/hooks/primitives)
- [`useInlineRender`](/docs/api-reference/model-context/context)