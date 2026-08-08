# Thread
URL: /docs/primitives/thread

Build custom scrollable message containers with auto-scroll, empty states, and message rendering.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

The Thread primitive is the scrollable message container, and the backbone of any chat interface. It handles viewport management, auto-scrolling, empty states, message rendering, and suggestions. You provide the layout and styling.

Choose one:

**Preview**

\[interactive preview omitted]

**Code**

```
import {
  AuiIf,
  ComposerPrimitive,
  ThreadPrimitive,
  MessagePrimitive,
} from "@assistant-ui/react";
import { ArrowUpIcon } from "lucide-react";

function MinimalThread() {
  return (
    <ThreadPrimitive.Root className="flex h-full flex-col">
      <ThreadPrimitive.Viewport className="flex flex-1 flex-col gap-3 overflow-y-auto p-3">
        <AuiIf condition={(s) => s.thread.isEmpty}>
          <p>Welcome! Ask a question to get started.</p>
        </AuiIf>

        <ThreadPrimitive.Messages>
          {({ message }) => {
            if (message.role === "user") return <UserMessage />;
            return <AssistantMessage />;
          }}
        </ThreadPrimitive.Messages>

        <ThreadPrimitive.ViewportFooter className="sticky bottom-0 pt-2">
          <ComposerPrimitive.Root className="flex w-full flex-col rounded-3xl border bg-muted">
            <ComposerPrimitive.Input
              placeholder="Ask anything..."
              className="min-h-10 w-full resize-none bg-transparent px-5 pt-3.5 pb-2.5 text-sm focus:outline-none"
              rows={1}
            />
            <div className="flex items-center justify-end px-2.5 pb-2.5">
              <ComposerPrimitive.Send className="flex size-8 items-center justify-center rounded-full bg-primary text-primary-foreground disabled:opacity-30">
                <ArrowUpIcon className="size-4" />
              </ComposerPrimitive.Send>
            </div>
          </ComposerPrimitive.Root>
        </ThreadPrimitive.ViewportFooter>
      </ThreadPrimitive.Viewport>
    </ThreadPrimitive.Root>
  );
}

function UserMessage() {
  return (
    <MessagePrimitive.Root className="flex justify-end">
      <div className="max-w-[80%] rounded-2xl bg-primary px-4 py-2.5 text-sm text-primary-foreground">
        <MessagePrimitive.Parts />
      </div>
    </MessagePrimitive.Root>
  );
}

function AssistantMessage() {
  return (
    <MessagePrimitive.Root className="flex justify-start">
      <div className="max-w-[80%] rounded-2xl bg-muted px-4 py-2.5 text-sm">
        <MessagePrimitive.Parts />
      </div>
    </MessagePrimitive.Root>
  );
}
```

## Quick Start

Minimal example:

```
import { ThreadPrimitive } from "@assistant-ui/react";

<ThreadPrimitive.Root>
  <ThreadPrimitive.Viewport>
    <ThreadPrimitive.Messages>
      {() => <MyMessage />}
    </ThreadPrimitive.Messages>
  </ThreadPrimitive.Viewport>
</ThreadPrimitive.Root>
```

`Root` renders a `<div>`, `Viewport` renders a scrollable `<div>`, and `Messages` iterates over the thread's messages. Add your own styles and components; the primitive handles the rest.

> [!info]
>
> Runtime setup: primitives require runtime context. Wrap your UI in `AssistantRuntimeProvider` with a runtime (for example `useLocalRuntime(...)`). See [Pick a Runtime](/docs/runtimes/pick-a-runtime).

## Core Concepts

### Viewport & Auto-Scroll

`Viewport` is the scrollable container. It auto-scrolls to the bottom as new content streams in, but only if the user hasn't scrolled up manually. Set `autoScroll={false}` to disable this entirely.

```
<ThreadPrimitive.Viewport autoScroll={true}>
  {/* messages */}
</ThreadPrimitive.Viewport>
```

### Turn Anchor

By default, new messages appear at the bottom and scroll down. With `turnAnchor="top"`, the user's message anchors to the top of the viewport. This creates the modern reading experience where you see the question at the top and the response flowing below it.

```
<ThreadPrimitive.Viewport turnAnchor="top">
  {/* messages */}
</ThreadPrimitive.Viewport>
```

When `turnAnchor="top"` is set, `MessagePrimitive.Root` automatically registers the latest assistant message as the top-anchor target and the preceding user message as the anchor — no additional component is required. The viewport itself manages a stable reserve element that provides the missing scroll range while the assistant response grows. This is the behavior used by the [Thread](/docs/ui/thread) component by default.

Use `topAnchorMessageClamp` to control how much of a long user message remains visible when `turnAnchor="top"`. Messages up to `tallerThan` stay fully visible. For messages taller than that, `visibleHeight` controls how much of the message's bottom edge remains visible above the assistant response:

```
<ThreadPrimitive.Viewport
  turnAnchor="top"
  topAnchorMessageClamp={{ tallerThan: "10em", visibleHeight: "6em" }}
>
  {/* messages */}
</ThreadPrimitive.Viewport>
```

### Viewport Scroll Options

`ThreadPrimitive.Viewport` has three event-specific scroll controls:

- `scrollToBottomOnRunStart` (default `true`): scrolls when `thread.runStart` fires
- `scrollToBottomOnInitialize` (default `true`): scrolls when `thread.initialize` fires
- `scrollToBottomOnThreadSwitch` (default `true`): scrolls when `threadListItem.switchedTo` fires

These work alongside `autoScroll`. If `autoScroll` is omitted, it defaults to `true` for `turnAnchor="bottom"` and `false` for `turnAnchor="top"`.

```
<ThreadPrimitive.Viewport
  turnAnchor="top"
  autoScroll={false}
  scrollToBottomOnRunStart={true}
  scrollToBottomOnInitialize={false}
  scrollToBottomOnThreadSwitch={true}
>
  <ThreadPrimitive.Messages>
    {({ message }) => {
      if (message.role === "user") return <UserMessage />;
      return <AssistantMessage />;
    }}
  </ThreadPrimitive.Messages>
</ThreadPrimitive.Viewport>
```

### ViewportFooter

`ViewportFooter` sticks to the bottom of the viewport and registers its height so the auto-scroll system accounts for it. This is where you place your composer:

```
<ThreadPrimitive.Viewport>
  <ThreadPrimitive.Messages>
    {() => <MyMessage />}
  </ThreadPrimitive.Messages>
  <ThreadPrimitive.ViewportFooter className="sticky bottom-0">
    <MyComposer />
  </ThreadPrimitive.ViewportFooter>
</ThreadPrimitive.Viewport>
```

### Empty State

> [!warn]
>
> `ThreadPrimitive.Empty` is deprecated. Use [`AuiIf`](/docs/api-reference/primitives/assistant-if) instead.

```
<AuiIf condition={(s) => s.thread.isEmpty}>
  <div className="flex flex-col items-center gap-2 text-center">
    <h2>Welcome!</h2>
    <p>How can I help you today?</p>
  </div>
</AuiIf>
```

### Messages Iterator

`Messages` now prefers a children render function. It gives you the current message state so you can branch inline:

```
<ThreadPrimitive.Messages>
  {({ message }) => {
    if (message.composer.isEditing) return <MyEditComposer />;
    if (message.role === "user") return <MyUserMessage />;
    return <MyAssistantMessage />;
  }}
</ThreadPrimitive.Messages>
```

`components` is deprecated. Use the `children` render function instead.

### Suggestions Iterator

`Suggestions` follows the same pattern. Prefer the children render function when rendering custom suggestion UIs:

```
<ThreadPrimitive.Suggestions>
  {() => <MySuggestionButton />}
</ThreadPrimitive.Suggestions>
```

## Parts

### Root

Top-level container for a thread layout. Renders a `<div>` element unless `asChild` is set.

```
<ThreadPrimitive.Root className="flex h-full flex-col">
  <ThreadPrimitive.Viewport>
    <ThreadPrimitive.Messages>
      {() => <MyMessage />}
    </ThreadPrimitive.Messages>
  </ThreadPrimitive.Viewport>
</ThreadPrimitive.Root>
```

### Viewport

The scrollable area with auto-scroll behavior. Renders a `<div>` element unless `asChild` is set.

```
<ThreadPrimitive.Viewport
  turnAnchor="top"
  autoScroll={false}
  scrollToBottomOnRunStart={true}
>
  <ThreadPrimitive.Messages>
    {({ message }) => {
      if (message.role === "user") return <UserMessage />;
      return <AssistantMessage />;
    }}
  </ThreadPrimitive.Messages>
</ThreadPrimitive.Viewport>
```

- `render?`: `ReactElement<unknown, string | JSXElementConstructor<any>>`

- `autoScroll?`: `boolean` — Whether to automatically scroll to the bottom when new messages are added. When enabled, the viewport will automatically scroll to show the latest content. Default false if \`turnAnchor\` is "top", otherwise defaults to true.

- `turnAnchor?`: `"top" | "bottom"` — Controls scroll anchoring behavior for new messages. - "bottom" (default): Messages anchor at the bottom, classic chat behavior. - "top": New user messages anchor at the top of the viewport for a focused reading experience.

- `topAnchorMessageClamp`: `{ tallerThan?: string; visibleHeight?: string; }` (default `{ tallerThan: "10em", visibleHeight: "6em" }`) — Clamps tall user messages so the assistant response stays in view.

  - `tallerThan`: `string` (default `"10em"`) — Clamp messages taller than this. Supports \`px\`, \`em\`, and \`rem\`.
  - `visibleHeight`: `string` (default `"6em"`) — Visible portion of clamped messages. Supports \`px\`, \`em\`, and \`rem\`.

- `scrollToBottomOnRunStart?`: `boolean` — Whether to scroll to bottom when a new run starts. Defaults to true.

- `scrollToBottomOnInitialize?`: `boolean` — Whether to scroll to bottom when thread history is first loaded. Defaults to true.

- `scrollToBottomOnThreadSwitch?`: `boolean` — Whether to scroll to bottom when switching to a different thread. Defaults to true.

### ViewportFooter

Footer container that registers its height with the viewport scroll system. Renders a `<div>` element unless `asChild` is set.

```
<ThreadPrimitive.ViewportFooter className="sticky bottom-0 pt-2">
  <ComposerPrimitive.Root>...</ComposerPrimitive.Root>
</ThreadPrimitive.ViewportFooter>
```

### ViewportProvider

Provides viewport context without rendering a scrollable element. Use this when you have a custom scroll container.

```
<ThreadPrimitive.ViewportProvider>
  <div className="flex-1 overflow-y-auto">
    <ThreadPrimitive.Messages>
      {() => <MyMessage />}
    </ThreadPrimitive.Messages>
    <ThreadPrimitive.ViewportFooter>
      <MyComposer />
    </ThreadPrimitive.ViewportFooter>
  </div>
</ThreadPrimitive.ViewportProvider>
```

### ViewportSlack

> [!warn]
>
> `ThreadPrimitive.ViewportSlack` has been removed from the public API. Top-anchor target registration is handled automatically by `MessagePrimitive.Root` when `turnAnchor="top"`. Remove `ViewportSlack` from your tree; if you customized `fillClampThreshold` or `fillClampOffset` on `ViewportSlack` or `MessagePrimitive.Root`, replace those props with `topAnchorMessageClamp` on `ThreadPrimitive.Viewport`.

### Messages

Renders a component for each message in the thread, resolved by role and edit state.

```
<ThreadPrimitive.Messages>
  {({ message }) => {
    if (message.composer.isEditing) return <MyEditComposer />;
    if (message.role === "user") return <MyUserMessage />;
    return <MyAssistantMessage />;
  }}
</ThreadPrimitive.Messages>
```

- `components?`: `MessagesComponentConfig` (deprecated: Use the children render function instead.)
- `children?`: `(value: { message: MessageState; }) => ReactNode` — Render function called for each message. Receives the message.

### MessageByIndex

Renders a single message at a specific index in the thread.

```
<ThreadPrimitive.MessageByIndex
  index={0}
  components={{ Message: MyMessage }}
/>
```

- `index`: `number`
- `components`: `MessagesComponentConfig`

### Unstable\_MessageById

Renders a single message by id with the same `components` surface as `MessageByIndex`. Pair it with `unstable_useThreadMessageIds` for virtualized or custom message lists that should stay attached to messages across reordering and windowing. Unknown ids render `null`.

```
const messageIds = unstable_useThreadMessageIds();

messageIds.map((messageId) => (
  <ThreadPrimitive.Unstable_MessageById
    key={messageId}
    messageId={messageId}
    components={{ Message: MyMessage }}
  />
));
```

> [!warn]
>
> `unstable_useThreadMessageIds` and `ThreadPrimitive.Unstable_MessageById` are experimental and may change in any release.

### ScrollToBottom

Scrolls the viewport to the bottom. Automatically disabled when already at the bottom. Renders a `<button>` element unless `asChild` is set.

```
<ThreadPrimitive.ScrollToBottom className="rounded-full bg-background p-2 shadow-md">
  <ArrowDownIcon />
</ThreadPrimitive.ScrollToBottom>
```

- `behavior?`: `ScrollBehavior`

### Suggestions

Renders suggestion prompts via a component.

```
<ThreadPrimitive.Suggestions>
  {({ suggestion }) => <MySuggestionButton prompt={suggestion.prompt} />}
</ThreadPrimitive.Suggestions>
```

- `components?`: `SuggestionsComponentConfig` (deprecated: Use the children render function instead.)
- `children?`: `(value: { suggestion: SuggestionState; }) => ReactNode` — Render function called for each suggestion. Receives the suggestion.

### SuggestionByIndex

Renders a single suggestion at a specific index.

```
<ThreadPrimitive.SuggestionByIndex
  index={0}
  components={{ Suggestion: MySuggestion }}
/>
```

- `index`: `number`
- `components`: `SuggestionsComponentConfig`

### Suggestion

Self-contained suggestion button. Renders a `<button>` element unless `asChild` is set. *(Legacy -- prefer `Suggestions` iterator.)*

```
<ThreadPrimitive.Suggestion prompt="Write a blog post" send />
```

- `prompt`: `string` — The suggestion prompt.
- `send?`: `boolean` — When true, automatically sends the message. When false, replaces or appends the composer text with the suggestion - depending on the value of \`clearComposer\`.
- `clearComposer`: `boolean` (default `true`) — Whether to clear the composer after sending. A send queued while a run is in progress never clears the composer. When send is set to false, determines if composer text is replaced with suggestion (true, default), or if it's appended to the composer text (false).
- `autoSend?`: `boolean` (deprecated: Use \`send\` instead.)
- `method?`: `"replace"` (deprecated: Use \`clearComposer\` instead.)

### Empty

> [!warn]
>
> Deprecated. Use [`AuiIf`](/docs/api-reference/primitives/assistant-if) with `s.thread.isEmpty` instead.

Legacy helper that only renders its children when the thread is empty.

```
<ThreadPrimitive.Empty>
  <div className="text-center text-muted-foreground">
    No messages yet.
  </div>
</ThreadPrimitive.Empty>
```

### If (deprecated)

> [!warn]
>
> Deprecated. Use [`AuiIf`](/docs/api-reference/primitives/assistant-if) instead.

```
// Before (deprecated)
<ThreadPrimitive.If empty>...</ThreadPrimitive.If>
<ThreadPrimitive.If running>...</ThreadPrimitive.If>
<ThreadPrimitive.If disabled>...</ThreadPrimitive.If>

// After
<AuiIf condition={(s) => s.thread.isEmpty}>...</AuiIf>
<AuiIf condition={(s) => s.thread.isRunning}>...</AuiIf>
<AuiIf condition={(s) => s.thread.isDisabled}>...</AuiIf>
```

## Patterns

### Welcome Screen with Suggestions

```
<AuiIf condition={(s) => s.thread.isEmpty}>
  <div className="flex flex-col items-center gap-4 text-center">
    <h2>What can I help with?</h2>
    <div className="grid grid-cols-2 gap-2">
      <ThreadPrimitive.Suggestions>
        {() => <MySuggestionButton />}
      </ThreadPrimitive.Suggestions>
    </div>
  </div>
</AuiIf>
```

### Scroll-to-Bottom Button

```
<ThreadPrimitive.ScrollToBottom className="fixed bottom-24 right-4 rounded-full bg-background p-2 shadow-md">
  <ArrowDownIcon />
</ThreadPrimitive.ScrollToBottom>
```

The button is automatically disabled when the viewport is already scrolled to the bottom.

### Turn Anchor Top Layout

```
<ThreadPrimitive.Root className="flex h-full flex-col">
  <ThreadPrimitive.Viewport turnAnchor="top" className="flex-1 overflow-y-auto">
    <ThreadPrimitive.Messages>
      {({ message }) => {
        if (message.role === "user") return <UserMessage />;
        return <AssistantMessage />;
      }}
    </ThreadPrimitive.Messages>
    <ThreadPrimitive.ViewportFooter className="sticky bottom-0">
      <MyComposer />
    </ThreadPrimitive.ViewportFooter>
  </ThreadPrimitive.Viewport>
</ThreadPrimitive.Root>
```

### Custom Message Components

```
<ThreadPrimitive.Messages>
  {({ message }) => {
    if (message.role === "user") {
      return (
        <MessagePrimitive.Root>
          <div className="ml-auto rounded-xl bg-blue-500 p-3 text-white">
            <MessagePrimitive.Parts />
          </div>
        </MessagePrimitive.Root>
      );
    }

    return (
      <MessagePrimitive.Root>
        <div className="rounded-xl bg-gray-100 p-3">
          <MessagePrimitive.Parts />
        </div>
      </MessagePrimitive.Root>
    );
  }}
</ThreadPrimitive.Messages>
```

## Relationship to Components

The [Thread](/docs/ui/thread) component is a full chat interface built from these primitives with Tailwind styling. Start there for a default implementation. Reach for `ThreadPrimitive` when you need a custom layout, different scroll behavior, or a non-standard thread structure.

## API Reference

For full prop details on every part, see the [ThreadPrimitive API Reference](/docs/api-reference/primitives/thread).

Related:

- [MessagePrimitive API Reference](/docs/api-reference/primitives/message)
- [ComposerPrimitive API Reference](/docs/api-reference/primitives/composer)
- [ThreadListPrimitive API Reference](/docs/api-reference/primitives/thread-list)