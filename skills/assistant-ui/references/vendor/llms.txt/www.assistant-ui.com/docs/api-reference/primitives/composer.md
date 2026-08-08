# ComposerPrimitive
URL: /docs/api-reference/primitives/composer

Composable input primitives for assistant-ui prompts, send controls, cancellation, attachments, and composer state.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [Composer](/docs/primitives/composer).

## Anatomy

```
import { ComposerPrimitive } from "@assistant-ui/react";

// creating a new message
const Composer = () => (
  <ComposerPrimitive.Root>
    <ComposerPrimitive.AttachmentDropzone>
      <ComposerPrimitive.Quote>
        <ComposerPrimitive.QuoteText />
        <ComposerPrimitive.QuoteDismiss />
      </ComposerPrimitive.Quote>
      <ComposerPrimitive.Attachments />
      <ComposerPrimitive.AddAttachment />
      <ComposerPrimitive.Input />
      <ComposerPrimitive.Send />
    </ComposerPrimitive.AttachmentDropzone>
  </ComposerPrimitive.Root>
);

// editing an existing message
const EditComposer = () => (
  <ComposerPrimitive.Root>
    <ComposerPrimitive.Input />
    <ComposerPrimitive.Send />
    <ComposerPrimitive.Cancel />
  </ComposerPrimitive.Root>
);

// with voice input (dictation)
const ComposerWithDictation = () => (
  <ComposerPrimitive.Root>
    <ComposerPrimitive.Input />
    <AuiIf condition={(s) => s.composer.dictation == null}>
      <ComposerPrimitive.Dictate />
    </AuiIf>
    <AuiIf condition={(s) => s.composer.dictation != null}>
      <ComposerPrimitive.StopDictation />
    </AuiIf>
    <ComposerPrimitive.Send />
  </ComposerPrimitive.Root>
);
```

## API Reference

### Root

The root form container for message composition. This component provides a form wrapper that handles message submission when the form is submitted (e.g., via Enter key or submit button). It automatically prevents the default form submission and triggers the composer's send functionality. Clicking blank form space focuses the composer input (the first textarea or contenteditable inside the form). Text selection cannot start from blank areas; descendants that need native mousedown defaults can call \`stopPropagation()\` in their own \`onMouseDown\`.

This primitive renders a `<form>` element unless `asChild` is set.

```
<ComposerPrimitive.Root>
  <ComposerPrimitive.Input placeholder="Type your message..." />
  <ComposerPrimitive.Send>Send</ComposerPrimitive.Send>
</ComposerPrimitive.Root>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `compact`: `boolean` (default `false`) — Opt the composer into compact mode. While the input holds at most a single line of text and there are no attachments, quote, queued messages, or active dictation, the root renders a \`data-compact\` attribute so styles can collapse the composer into a single row. It expands automatically when any of those conditions stop holding.

### Input

A text input component for composing messages. This component provides a rich text input experience with automatic resizing, keyboard shortcuts, file paste support, and intelligent focus management. It integrates with the composer context to manage message state and submission. When rendered inside \`Unstable\_TriggerPopoverRoot\` and a popover is open, the underlying \`\<textarea>\` automatically receives \`aria-controls\`, \`aria-expanded\`, \`aria-haspopup\`, and \`aria-activedescendant\` for the combobox relationship. These computed attributes override user-provided values for those four ARIA props while the popover is open.

This primitive renders a `<textarea>` element unless `asChild` is set.

```
// Ctrl/Cmd+Enter to submit (plain Enter inserts newline)
<ComposerPrimitive.Input
  placeholder="Type your message..."
  submitMode="ctrlEnter"
/>

// Insert a newline on Enter on touch-primary devices.
<ComposerPrimitive.Input
  placeholder="Type your message..."
  unstable_insertNewlineOnTouchEnter
/>

// Old API (deprecated, still supported)
<ComposerPrimitive.Input
  placeholder="Type your message..."
  submitOnEnter={true}
/>
```

- `asChild`: `boolean` (default `false`) — Whether to render as a child component using Slot. When true, the component will merge its props with its child.
- `render?`: `ReactElement` — A React element to use as the input container, with props merged in.
- `cancelOnEscape`: `boolean` (default `true`) — Whether to cancel message composition when Escape is pressed.
- `unstable_focusOnRunStart`: `boolean` (default `true`) — Whether to automatically focus the input when a new run starts.
- `unstable_focusOnScrollToBottom`: `boolean` (default `true`) — Whether to automatically focus the input when scrolling to bottom.
- `unstable_focusOnThreadSwitched`: `boolean` (default `true`) — Whether to automatically focus the input when switching threads.
- `unstable_insertNewlineOnTouchEnter`: `boolean` (default `false`) — Whether plain Enter on a touch-primary device should insert a newline instead of submitting, detected via \`(pointer: coarse) and (not (any-pointer: fine))\`. Only takes effect when \`submitMode\` resolves to \`"enter"\`.
- `addAttachmentOnPaste`: `boolean` (default `true`) — Whether to automatically add pasted files as attachments.
- `submitMode`: `"enter" | "ctrlEnter" | "none"` (default `"enter"`) — Controls how the Enter key submits messages. - "enter": Plain Enter submits (Shift+Enter for newline) - "ctrlEnter": Ctrl/Cmd+Enter submits (plain Enter for newline) - "none": Keyboard submission disabled
- `submitOnEnter`: `boolean` (default `true`) (deprecated: Use \`submitMode\` instead) — Whether to submit the message when Enter is pressed (without Shift).

#### Keyboard Shortcuts

Default (`submitMode="enter"`):

| Key             | Description            |
| --------------- | ---------------------- |
| `Enter`         | Sends the message.     |
| `Shift + Enter` | Inserts a newline.     |
| `Escape`        | Sends a cancel action. |

With `submitMode="ctrlEnter"`:

| Key                | Description            |
| ------------------ | ---------------------- |
| `Ctrl/Cmd + Enter` | Sends the message.     |
| `Enter`            | Inserts a newline.     |
| `Escape`           | Sends a cancel action. |

With `submitMode="none"`:

| Key      | Description                                 |
| -------- | ------------------------------------------- |
| `Enter`  | Inserts a newline (no keyboard submission). |
| `Escape` | Sends a cancel action.                      |

#### Touch-primary devices

Pass `unstable_insertNewlineOnTouchEnter` to make plain Enter insert a newline on phones and tablets without a hardware keyboard, detected via the `(pointer: coarse) and (not (any-pointer: fine))` media query. Messages then dispatch only via the explicit Send button, matching the chat-input convention used by WhatsApp, Slack, Discord, iMessage, ChatGPT, and Claude.ai.

```
<ComposerPrimitive.Input
  placeholder="Ask anything..."
  unstable_insertNewlineOnTouchEnter
/>
```

Only takes effect when `submitMode` resolves to `"enter"` (the default); `"ctrlEnter"` and `"none"` are unchanged, so a tablet paired with a hardware keyboard can still submit via Cmd/Ctrl+Enter.

### Send

A button component that sends the current message in the composer. This component automatically handles the send functionality and is disabled when sending is not available (e.g., when the thread is running, the composer is empty, or not in editing mode).

This primitive renders a `<button>` element unless `asChild` is set.

```
<ComposerPrimitive.Send>
  Send Message
</ComposerPrimitive.Send>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Cancel

A button component that cancels the current message composition. This component automatically handles the cancel functionality and is disabled when canceling is not available.

This primitive renders a `<button>` element unless `asChild` is set.

```
<ComposerPrimitive.Cancel>
  Cancel
</ComposerPrimitive.Cancel>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### AddAttachment

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `multiple?`: `boolean` — allow selecting multiple files

### Attachments

- `components?`: `ComposerAttachmentsComponentConfig` (deprecated: Use the children render function instead.)

  - `Image?`: `ComponentType`
  - `Document?`: `ComponentType`
  - `File?`: `ComponentType`
  - `Attachment?`: `ComponentType`

- `children?`: `(value: { attachment: Attachment }) => ReactNode` — Render function called for each attachment. Receives the attachment.

### AttachmentByIndex

Renders a single attachment at the specified index within the composer.

- `index`: `number`

- `components?`: `ComposerAttachmentsComponentConfig`

  - `Image?`: `ComponentType`
  - `Document?`: `ComponentType`
  - `File?`: `ComponentType`
  - `Attachment?`: `ComponentType`

### AttachmentDropzone

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `disabled?`: `boolean`

| Data attribute    | Values                                                   |
| ----------------- | -------------------------------------------------------- |
| `[data-dragging]` | Present while a file is being dragged over the dropzone. |

### Dictate

A button that starts dictation to convert voice to text. Requires a DictationAdapter to be configured in the runtime.

This primitive renders a `<button>` element unless `asChild` is set.

```
<ComposerPrimitive.Dictate>
  <MicIcon />
</ComposerPrimitive.Dictate>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### StopDictation

A button that stops the current dictation session. Only rendered when dictation is active.

This primitive renders a `<button>` element unless `asChild` is set.

```
<ComposerPrimitive.StopDictation>
  <StopIcon />
</ComposerPrimitive.StopDictation>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### DictationTranscript

Renders the current interim (partial) transcript while dictation is active. This component displays real-time feedback of what the user is saying before the transcription is finalized and committed to the composer input.

This primitive renders a `<span>` element unless `asChild` is set.

```
<ComposerPrimitive.If dictation>
  <div className="dictation-preview">
    <ComposerPrimitive.DictationTranscript />
  </div>
</ComposerPrimitive.If>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### If

> [!warn]
>
> **Deprecated.** Use \`\<AuiIf condition={(s) => s.composer...} />\` instead.

- `editing?`: `boolean` — Whether the composer is in editing mode
- `dictation?`: `boolean` — Whether dictation is currently active

### Quote

Renders a container for the quoted text preview in the composer. Only renders when a quote is set.

This primitive renders a `<div>` element unless `asChild` is set.

```
<ComposerPrimitive.Quote>
  <ComposerPrimitive.QuoteText />
  <ComposerPrimitive.QuoteDismiss>×</ComposerPrimitive.QuoteDismiss>
</ComposerPrimitive.Quote>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### QuoteText

Renders the quoted text content.

This primitive renders a `<span>` element unless `asChild` is set.

```
<ComposerPrimitive.QuoteText />
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### QuoteDismiss

A button that clears the current quote from the composer.

This primitive renders a `<button>` element unless `asChild` is set.

```
<ComposerPrimitive.QuoteDismiss>×</ComposerPrimitive.QuoteDismiss>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Queue

Renders all queue items in the composer.

```
<ComposerPrimitive.Queue>
  {({ queueItem }) => (
    <div>
      <QueueItemPrimitive.Text />
      <QueueItemPrimitive.Steer>Run Now</QueueItemPrimitive.Steer>
    </div>
  )}
</ComposerPrimitive.Queue>
```

- `children`: `(value: { queueItem: QueueItemState }) => ReactNode` — Render function called for each queue item. Receives the queue item state.

### Unstable\_TriggerPopover

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.

- `render?`: `ReactElement`

- `char`: `string` — The character(s) that activate this trigger (e.g. \`"@"\`, \`"/"\`). Also serves as the trigger identity within the root.

- `adapter?`: `Unstable_TriggerAdapter` — Adapter providing categories and items.

  - `categories`: `() => readonly Unstable_TriggerCategory[]` — Return the top-level categories for the trigger popover.
  - `categoryItems`: `(categoryId: string) => readonly Unstable_TriggerItem[]` — Return items within a category.
  - `search?`: `(query: string) => readonly Unstable_TriggerItem[]` — Global search across all categories (optional).

- `isLoading`: `boolean` (default `false`) — Whether the adapter is resolving items, surfaced to the popover scope for async sources.

| Data attribute | Values                                     |
| -------------- | ------------------------------------------ |
| `[data-state]` | `"open"` when the trigger popover is open. |

### Unstable\_TriggerPopoverRoot

Provider that groups one or more \`TriggerPopover\` declarations. Each trigger is identified by its \`char\` (unique within the root). Behavior is contributed by a child \`TriggerPopover.Directive\` or \`TriggerPopover.Action\`.

```
<ComposerPrimitive.Unstable_TriggerPopoverRoot>
  <ComposerPrimitive.Unstable_TriggerPopover char="@" adapter={mentionAdapter}>
    <ComposerPrimitive.Unstable_TriggerPopover.Directive formatter={formatter} />
    ...
  </ComposerPrimitive.Unstable_TriggerPopover>

  <ComposerPrimitive.Unstable_TriggerPopover char="/" adapter={slashAdapter}>
    <ComposerPrimitive.Unstable_TriggerPopover.Action onExecute={handler} />
    ...
  </ComposerPrimitive.Unstable_TriggerPopover>

  <ComposerPrimitive.Root>
    <ComposerPrimitive.Input />
  </ComposerPrimitive.Root>
</ComposerPrimitive.Unstable_TriggerPopoverRoot>
```

- `children?`: `ReactNode`

### Unstable\_TriggerPopoverCategories

Renders the top-level category list via a render function. Only renders when no category is active and search mode is off.

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `children`: `(categories: readonly Unstable_TriggerCategory[]) => ReactNode`

### Unstable\_TriggerPopoverCategoryItem

A button that selects a category and triggers drill-down navigation. Automatically receives \`data-highlighted\` when keyboard-navigated.

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `categoryId`: `string`

| Data attribute       | Values                             |
| -------------------- | ---------------------------------- |
| `[data-highlighted]` | Present when keyboard-highlighted. |

### Unstable\_TriggerPopoverItems

Renders the list of items within a category or search results via a render function. Only renders when a category is active or search mode is on.

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `children`: `(items: readonly Unstable_TriggerItem[]) => ReactNode`

### Unstable\_TriggerPopoverItem

A button that selects a trigger item. Automatically receives \`data-highlighted\` when keyboard-navigated.

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.

- `render?`: `ReactElement`

- `item`: `Unstable_TriggerItem`

  - `id`: `string`
  - `type`: `string`
  - `label`: `string`
  - `description?`: `string`
  - `metadata?`: `ReadonlyJSONObject`

- `index?`: `number`

| Data attribute       | Values                             |
| -------------------- | ---------------------------------- |
| `[data-highlighted]` | Present when keyboard-highlighted. |

### Unstable\_TriggerPopoverBack

A button that navigates back from category items to the category list. Only renders when a category is active (drill-down view).

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Unstable\_RegisteredTrigger