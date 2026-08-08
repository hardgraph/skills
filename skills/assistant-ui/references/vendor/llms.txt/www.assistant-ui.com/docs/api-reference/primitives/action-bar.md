# ActionBarPrimitive
URL: /docs/api-reference/primitives/action-bar

Composable message action controls for copy, edit, reload, speech, and feedback in assistant-ui chat interfaces.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [ActionBar](/docs/primitives/action-bar).

## Anatomy

```
import { ActionBarPrimitive } from "@assistant-ui/react";

const UserMessageBar = () => (
  <ActionBarPrimitive.Root>
    <ActionBarPrimitive.Edit />
    <ActionBarPrimitive.Copy />
  </ActionBarPrimitive.Root>
);

const AssistantMessageBar = () => (
  <ActionBarPrimitive.Root>
    <ActionBarPrimitive.Reload />
    <ActionBarPrimitive.Copy />
  </ActionBarPrimitive.Root>
);
```

## API Reference

### Root

The root container for action bar components. This component provides intelligent visibility and floating behavior for action bars, automatically hiding and showing based on message state, hover status, and configuration. It supports floating mode for better UX when space is limited.

This primitive renders a `<div>` element unless `asChild` is set.

```
<ActionBarPrimitive.Root
  hideWhenRunning={true}
  autohide="not-last"
  autohideFloat="single-branch"
>
  <ActionBarPrimitive.Copy />
  <ActionBarPrimitive.Edit />
  <ActionBarPrimitive.Reload />
</ActionBarPrimitive.Root>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `hideWhenRunning`: `boolean` (default `false`) — Whether to hide the action bar when the thread is running.
- `autohide`: `"always" | "not-last" | "never"` (default `"never"`) — Controls when the action bar should automatically hide. - "always": Always hide unless hovered - "not-last": Hide unless this is the last message - "never": Never auto-hide
- `autohideFloat`: `"always" | "single-branch" | "never"` (default `"never"`) — Controls floating behavior when auto-hidden. - "always": Always float when hidden - "single-branch": Float only for single-branch messages - "never": Never float

| Data attribute    | Values                |
| ----------------- | --------------------- |
| `[data-floating]` | Present when floating |

### Copy

A button component that copies message content to the clipboard. This component automatically handles copying message text to the clipboard and provides visual feedback through the data-copied attribute. It's disabled when there's no copyable content available.

This primitive renders a `<button>` element unless `asChild` is set.

```
<ActionBarPrimitive.Copy copiedDuration={2000}>
  Copy Message
</ActionBarPrimitive.Copy>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `copiedDuration?`: `number`

| Data attribute  | Values                                        |
| --------------- | --------------------------------------------- |
| `[data-copied]` | Present when the message was recently copied. |

### Reload

A button component that reloads/regenerates the current assistant message. This component automatically handles reloading the current assistant message and is disabled when reloading is not available (e.g., thread is running, disabled, or message is not from assistant).

This primitive renders a `<button>` element unless `asChild` is set.

```
<ActionBarPrimitive.Reload>
  Reload Message
</ActionBarPrimitive.Reload>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Edit

A button component that starts editing the current message. This component automatically handles starting the edit mode for the current message and is disabled when editing is not available (e.g., already in editing mode).

This primitive renders a `<button>` element unless `asChild` is set.

```
<ActionBarPrimitive.Edit>
  Edit Message
</ActionBarPrimitive.Edit>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Speak

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### StopSpeaking

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### FeedbackPositive

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

| Data attribute     | Values                                        |
| ------------------ | --------------------------------------------- |
| `[data-submitted]` | Present when positive feedback was submitted. |

### FeedbackNegative

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

| Data attribute     | Values                                        |
| ------------------ | --------------------------------------------- |
| `[data-submitted]` | Present when negative feedback was submitted. |

### ExportMarkdown

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `filename?`: `string`
- `onExport?`: `((content: string) => void | Promise<void>)`