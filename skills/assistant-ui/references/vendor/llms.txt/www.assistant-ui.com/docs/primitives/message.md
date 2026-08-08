# Message
URL: /docs/primitives/message

Build custom message rendering with content parts, attachments, and hover state.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

The Message primitive handles individual message rendering: content parts, attachments, quotes, hover state, and error display. It's the building block inside each message bubble, resolving text, images, tool calls, and more through a parts pipeline.

Choose one:

**Preview**

\[interactive preview omitted]

**Code**

```
import {
  MessagePrimitive,
  MessagePartPrimitive,
} from "@assistant-ui/react";

function UserMessage() {
  return (
    <MessagePrimitive.Root className="flex justify-end">
      <div className="max-w-[80%] rounded-2xl bg-primary px-4 py-2.5 text-sm text-primary-foreground">
        <MessagePrimitive.Parts>
          {({ part }) => {
            if (part.type === "text") return <UserText />;
            return null;
          }}
        </MessagePrimitive.Parts>
      </div>
    </MessagePrimitive.Root>
  );
}

function AssistantMessage() {
  return (
    <MessagePrimitive.Root className="flex justify-start gap-3">
      <div className="flex size-8 items-center justify-center rounded-full bg-primary/10 text-xs font-medium text-primary">
        AI
      </div>
      <div className="max-w-[80%] rounded-2xl bg-muted px-4 py-2.5 text-sm">
        <MessagePrimitive.Parts>
          {({ part }) => {
            if (part.type === "text") return <AssistantText />;
            return part.toolUI ?? null;
          }}
        </MessagePrimitive.Parts>
      </div>
    </MessagePrimitive.Root>
  );
}

function UserText() {
  return (
    <p>
      <MessagePartPrimitive.Text />
    </p>
  );
}

function AssistantText() {
  return (
    <p className="leading-relaxed">
      <MessagePartPrimitive.Text />
    </p>
  );
}
```

## Quick Start

A minimal message with parts rendering:

```
import { MessagePrimitive } from "@assistant-ui/react";

<MessagePrimitive.Root>
  <MessagePrimitive.Parts />
</MessagePrimitive.Root>
```

`Root` renders a `<div>` that provides message context and tracks hover state. `Parts` iterates over the message's content parts and renders each one. Without custom components, parts render with sensible defaults: `Text` renders a `<p>` with `white-space: pre-line` and a streaming indicator, `Image` renders via `MessagePartPrimitive.Image`, and tool calls render nothing unless a tool UI is registered globally or inline. Reasoning, source, file, and audio parts render nothing by default.

> [!info]
>
> Runtime setup: primitives require runtime context. Wrap your UI in `AssistantRuntimeProvider` with a runtime (for example `useLocalRuntime(...)`). See [Pick a Runtime](/docs/runtimes/pick-a-runtime).

## Core Concepts

### Parts Pipeline

`MessagePrimitive.Parts` now prefers a children render function. It gives you the current enriched part state directly, so you can branch inline and return exactly the UI you want:

```
<MessagePrimitive.Parts>
  {({ part }) => {
    if (part.type === "text") return <MyTextRenderer />;
    if (part.type === "image") return <MyImageRenderer />;
    if (part.type === "tool-call")
      return part.toolUI ?? <GenericToolUI {...part} />;
    return null;
  }}
</MessagePrimitive.Parts>
```

> [!info]
>
> For most new code, prefer `MessagePrimitive.Parts` with a `children` render function. When you need adjacent grouping, use `MessagePrimitive.GroupedParts`.

### Part Types

A message part is one of three kinds, and which kind a new capability belongs to is decided by the list below rather than case by case.

| Kind             | Parts                                               | Grows by                                                                      |
| ---------------- | --------------------------------------------------- | ----------------------------------------------------------------------------- |
| Modality         | `text`, `image`, `file`                             | Never. `file` carries every non-image binary modality through its `mimeType`. |
| Provider channel | `reasoning`, `source`, `tool-call`, `generative-ui` | Never. These mirror channels a model already emits.                           |
| Extensibility    | `data`                                              | Freely, through `name`. This is the growth path for anything app-level.       |

`file` is the media carrier. Audio, video, PDFs and everything else are `{ type: "file", mimeType: "audio/mpeg" }` rather than part types of their own, so one renderer and one converter branch handle them all. The payload can be inline base64 or a URL; `sourceType` additionally allows an opaque storage id, which the LangChain-family runtimes honor and other adapters ignore.

Anything that is not a modality the model consumes or a channel it emits is a `data` part, routed by `name`:

```
<MessagePrimitive.Parts
  components={{
    data: {
      by_name: { citation: MyCitation },
      Fallback: MyDataFallback,
    },
  }}
/>
```

> [!warn]
>
> `Unstable_AudioMessagePart` and the `Unstable_Audio` slot are deprecated. The audio part cannot carry a filename, has no way to declare how its payload goes on the wire (no `sourceType`, so no storage id), and exists only on user messages, so a model that returns audio has no way to express it. Send audio as a `file` part with an `audio/*` mime type instead, and give it a filename.
>
> Adapter coverage for the file path is still uneven, so check your adapter's converter before relying on file parts. Known so far: `react-pi` and the assistant-transport runtime have no file part on their user-content surface at all, so one is dropped; `react-data-stream` carries the payload but not the filename. Existing audio parts keep working; they will not gain fields.

### Tool Resolution

Tool call parts resolve in this order:

1. **`tools.Override`**: if provided inline through the deprecated `components` prop, handles **all** tool calls
2. **Globally registered tools**: tools registered via `Tools({ toolkit })`
3. **`tools.by_name[toolName]`**: per-`MessagePrimitive.Parts` inline overrides from the deprecated `components` prop
4. **`tools.Fallback`**: catch-all for unmatched tool calls from the deprecated `components` prop
5. **`part.toolUI`**: the resolved tool UI exposed directly in the children render function

In the children API, tool and data parts expose resolved UI helpers directly:

```
<MessagePrimitive.Parts>
  {({ part }) => {
    if (part.type === "tool-call")
      return part.toolUI ?? <ToolFallback {...part} />;

    if (part.type === "data")
      return part.dataRendererUI ?? null;

    return null;
  }}
</MessagePrimitive.Parts>
```

Returning `null` still allows registered tool UIs and data renderer UIs to render automatically. Return `<></>` if you want to suppress them entirely.

### Components Prop (Deprecated)

`components` is deprecated. This section only documents it so older code is still understandable:

- `ToolGroup` wraps consecutive tool-call parts
- `ReasoningGroup` wraps consecutive reasoning parts
- `components.ChainOfThought` takes over all reasoning and tool-call rendering (mutually exclusive with `ToolGroup`, `ReasoningGroup`, `tools`, and `Reasoning`). This legacy path is deprecated; use `MessagePrimitive.GroupedParts` for grouped Chain of Thought in new code.
- `data.by_name` and `data.Fallback` let you route custom data part types
- `Quote` renders quoted message references from metadata
- `Empty` is available for edge rendering paths
- `Unstable_Audio` is deprecated; render `audio/*` from the `File` slot instead

```
<MessagePrimitive.Parts
  components={{
    Text: () => (
      <p className="whitespace-pre-wrap">
        <MessagePartPrimitive.Text />
      </p>
    ),
    Image: () => <MessagePartPrimitive.Image className="max-w-sm rounded-xl" />,
    File: () => <div className="rounded-md border px-2 py-1 text-xs">File part</div>,
    tools: {
      by_name: {
        get_weather: () => <div>Weather tool</div>,
      },
      Fallback: ({ toolName }) => <div>Unknown tool: {toolName}</div>,
    },
    data: {
      by_name: {
        "my-event": ({ data }) => <pre>{JSON.stringify(data, null, 2)}</pre>,
      },
      Fallback: ({ name }) => <div>Unknown data event: {name}</div>,
    },
    ToolGroup: ({ children }) => (
      <div className="space-y-2 rounded-lg border p-2">{children}</div>
    ),
    ReasoningGroup: ({ children }) => (
      <details className="rounded-lg border p-2">
        <summary>Reasoning</summary>
        {children}
      </details>
    ),
    Empty: () => <span className="text-muted-foreground">...</span>,
    Unstable_Audio: () => null,
  }}
/>
```

For new code, use the `children` render function or `GroupedParts` instead.

### Hover State

`MessagePrimitive.Root` automatically tracks mouse enter/leave events. This hover state is consumed by `ActionBarPrimitive` to implement auto-hide behavior, with no extra wiring needed.

### MessagePartPrimitive

Inside your custom part components, use these sub-primitives to access the actual content:

- **`MessagePartPrimitive.Text`**: renders the text content of a text part
- **`MessagePartPrimitive.Image`**: renders the image of an image part
- **`MessagePartPrimitive.InProgress`**: renders only while the part is still streaming

```
function MyText() {
  return (
    <p className="whitespace-pre-wrap">
      <MessagePartPrimitive.Text />
      <MessagePartPrimitive.InProgress>
        <span className="animate-pulse">▊</span>
      </MessagePartPrimitive.InProgress>
    </p>
  );
}
```

## Parts

### Root

Container for a single message. Renders a `<div>` element unless `asChild` is set.

```
<MessagePrimitive.Root className="flex flex-col gap-2">
  <MessagePrimitive.Quote>
    {({ text }) => <blockquote className="mb-2 border-l pl-3 italic">{text}</blockquote>}
  </MessagePrimitive.Quote>
  <MessagePrimitive.Parts />
</MessagePrimitive.Root>
```

### Parts

Renders each content part with type-based component resolution.

```
<MessagePrimitive.Parts>
  {({ part }) => {
    if (part.type === "text") return <MyTextRenderer />;
    if (part.type === "image") return <MyImageRenderer />;
    if (part.type === "tool-call")
      return part.toolUI ?? <GenericToolUI {...part} />;
    return null;
  }}
</MessagePrimitive.Parts>
```

- `components?`: `StandardComponents | ChainOfThoughtComponents` — Component configuration for rendering different types of message content. Use either \`Reasoning\`/\`tools\`/\`ToolGroup\`/\`ReasoningGroup\` for standard rendering, or \`ChainOfThought\` to group all reasoning and tool-call parts into a single collapsible component. These two modes are mutually exclusive.
- `unstable_showEmptyOnNonTextEnd`: `boolean` (default `true`) — When enabled, shows the Empty component if the last part in the message is anything other than Text or Reasoning.
- `children?`: `(value: { part: EnrichedPartState; }) => ReactNode` — Render function called for each part. Receives the enriched part state.

### GroupedParts

Groups adjacent message parts into a nested tree. Use `groupBy` to map each part to a group-key path, then switch on `part.type` in the render function. Group cases render `children`; leaf cases render their own UI. Prefer the `groupPartByType` helper for the common `part.type → path` case.

```
import { MessagePrimitive, groupPartByType } from "@assistant-ui/react";

<MessagePrimitive.GroupedParts
  groupBy={groupPartByType({
    reasoning: ["group-chainOfThought", "group-reasoning"],
    "tool-call": ["group-chainOfThought", "group-tool"],
  })}
>
  {({ part, children }) => {
    switch (part.type) {
      case "group-chainOfThought":
        return <div>{children}</div>;
      case "group-reasoning":
        return <ReasoningRoot>{children}</ReasoningRoot>;
      case "group-tool":
        return <ToolGroupRoot>{children}</ToolGroupRoot>;
      case "text":
        return <MarkdownText />;
      case "reasoning":
        return <Reasoning {...part} />;
      case "tool-call":
        return part.toolUI ?? <ToolFallback {...part} />;
      case "data":
        return part.dataRendererUI;
      default:
        return null;
    }
  }}
</MessagePrimitive.GroupedParts>
```

- `groupBy`: `(part: PartState, context: GroupByContext) => readonly TKey[] | null` — Maps each part to a group-key path. Adjacent parts that share a prefix coalesce into the same group. Return \`\[]\` (or \`null\`) to leave a part ungrouped. Group keys must start with \`"group-"\` so the renderer's \`switch (part.type)\` can tell groups apart from real part types. \*\*Prefer [groupPartByType](/docs/api-reference/primitives/message#grouppartbytype)\*\* for the common case of mapping by \`part.type\` — it ships a stable memo fingerprint so the tree survives unrelated re-renders. Use an inline function only when the helper isn't expressive enough (e.g. branching on \`part.toolName\` or part metadata). The second argument is a GroupByContext carrying the tool-UI registry, for grouping that depends on it (e.g. standalone tool calls).
- `indicator`: `IndicatorMode` (default `"no-text"`) — Controls emission of the synthetic IndicatorPart — a trailing \`{ part: { type: "indicator", status } }\` render call you handle with \`case "indicator"\` to show loading/status UI.
- `children`: `(info: RenderInfo<TKey>) => ReactNode` — Render function called once per group node, once per leaf part, and (when the \`indicator\` condition is met) once for the trailing IndicatorPart. Switch on \`part.type\`: \`"group-…"\` cases wrap \`children\`; real part types (\`"text"\`, \`"tool-call"\`, …) render the part directly; \`"indicator"\` renders status/loading UI. Leaf parts receive the same [EnrichedPartState](/docs/api-reference/runtimes/message-part-runtime#enrichedpartstate) that \`\<MessagePrimitive.Parts>\` would produce (\`toolUI\`, \`addResult\`, \`resume\`, \`respondToApproval\`, \`dataRendererUI\`).

### Content

Legacy alias for `Parts`.

```
<MessagePrimitive.Content>
  {({ part }) => {
    if (part.type === "text") return <MyTextRenderer />;
    return null;
  }}
</MessagePrimitive.Content>
```

### PartByIndex

Renders a single part at a specific index.

```
<MessagePrimitive.PartByIndex
  index={0}
  components={{ Text: MyTextRenderer }}
/>
```

### Attachments

Renders all user message attachments.

```
<MessagePrimitive.Attachments>
  {({ attachment }) => {
    if (attachment.type === "image") {
      const imageSrc = attachment.content?.find((part) => part.type === "image")?.image;
      if (!imageSrc) return null;
      return <img src={imageSrc} alt={attachment.name} className="max-w-xs rounded-lg" />;
    }

    if (attachment.type === "document") {
      return (
        <div className="rounded-lg border p-2 text-sm">
          {attachment.name}
        </div>
      );
    }

    return null;
  }}
</MessagePrimitive.Attachments>
```

- `components?`: `MessageAttachmentsComponentConfig` (deprecated: Use the children render function instead.)
- `children?`: `(value: { attachment: CompleteAttachment; }) => ReactNode` — Render function called for each attachment. Receives the attachment.

### AttachmentByIndex

Renders a single attachment at the specified index within the current message.

```
<MessagePrimitive.AttachmentByIndex
  index={0}
  components={{ Attachment: MyAttachment }}
/>
```

- `index`: `number`
- `components?`: `MessageAttachmentsComponentConfig`

### Error

Renders children only when the message has an error.

```
<MessagePrimitive.Error>
  <ErrorPrimitive.Root className="mt-2 rounded-md border border-destructive/20 bg-destructive/5 p-3">
    <ErrorPrimitive.Message />
  </ErrorPrimitive.Root>
</MessagePrimitive.Error>
```

### Quote

Renders quote metadata when the current message includes a quote. Place it above `MessagePrimitive.Parts`.

```
<MessagePrimitive.Quote>
  {({ text, messageId }) => (
    <blockquote className="mb-2 border-l pl-3 italic" data-message-id={messageId}>
      {text}
    </blockquote>
  )}
</MessagePrimitive.Quote>
```

### Unstable\_PartsGrouped

Groups parts by a custom grouping function *(unstable; use `GroupedParts` for adjacent grouping)*.

```
<MessagePrimitive.Unstable_PartsGrouped
  groupingFunction={myGroupFn}
  components={{ Text: MyText, Group: MyGroupWrapper }}
/>
```

- `groupingFunction`: `GroupingFunction` — Function that takes an array of message parts and returns an array of groups. Each group contains a key (for identification) and an array of indices.

- `components?`: `{ Empty?: EmptyMessagePartComponent | undefined; Text?: TextMessagePartComponent | undefined; Reasoning?: ReasoningMessagePartComponent | undefined; Source?: SourceMessagePartComponent | undefined; Image?: ImageMessagePartComponent | undefined; File?: FileMessagePartComponent | undefined; Unstable_Audio?: Unstable_AudioMessagePartComponent | undefined; data?: { by_name?: Record<string, DataMessagePartComponent | undefined> | undefined; ... } | undefined; tools?: { by_name?: Record<string, ToolCallMessagePartComponent | undefined> | undefined; ... } | { Override: ComponentType<ToolCallMessagePartProps>; } | undefined; Group?: ComponentType<PropsWithChildren<{ groupKey: string | undefined; indices: number[]; }>>; }` — Component configuration for rendering different types of message content. You can provide custom components for each content type (text, image, file, etc.) and configure tool rendering behavior. If not provided, default components will be used.

  - `Empty?`: `EmptyMessagePartComponent` — Component for rendering empty messages
  - `Text?`: `TextMessagePartComponent` — Component for rendering text content
  - `Reasoning?`: `ReasoningMessagePartComponent` — Component for rendering reasoning content (typically hidden)
  - `Source?`: `SourceMessagePartComponent` — Component for rendering source content
  - `Image?`: `ImageMessagePartComponent` — Component for rendering image content
  - `File?`: `FileMessagePartComponent` — Component for rendering file content
  - `Unstable_Audio?`: `Unstable_AudioMessagePartComponent` (deprecated: Render audio through the \`File\` slot instead, branching on an \`audio/\*\` mime type.) — Component for rendering audio content.
  - `data?`: `{ by_name?: Record<string, DataMessagePartComponent | undefined> | undefined; ... }` — Configuration for data part rendering
  - `tools?`: `{ by_name?: Record<string, ToolCallMessagePartComponent | undefined> | undefined; ... } | { Override: ComponentType<ToolCallMessagePartProps>; }` — Configuration for tool call rendering
  - `Group?`: `ComponentType<PropsWithChildren<{ groupKey: string | undefined; indices: number[]; }>>` — Component for rendering grouped message parts. When provided, this component will automatically wrap message parts that share the same group key as determined by the groupingFunction. The component receives: - \`groupKey\`: The group key (or undefined for ungrouped parts) - \`indices\`: Array of indices for the parts in this group - \`children\`: The rendered message part components

### Unstable\_PartsGroupedByParentId

Groups parts by parent ID *(unstable, deprecated; use `Unstable_PartsGrouped`)*.

```
<MessagePrimitive.Unstable_PartsGroupedByParentId
  components={{ Text: MyText, Group: MyGroupWrapper }}
/>
```

### If (deprecated)

> [!warn]
>
> Deprecated. Use [`AuiIf`](/docs/api-reference/primitives/assistant-if) instead.

```
// Before (deprecated)
<MessagePrimitive.If user>...</MessagePrimitive.If>
<MessagePrimitive.If assistant>...</MessagePrimitive.If>

// After
<AuiIf condition={(s) => s.message.role === "user"}>...</AuiIf>
<AuiIf condition={(s) => s.message.role === "assistant"}>...</AuiIf>
```

## Patterns

### Custom Text Rendering

```
function MarkdownText() {
  return (
    <div className="prose prose-sm">
      <MessagePartPrimitive.Text />
    </div>
  );
}

<MessagePrimitive.Parts>
  {({ part }) => {
    if (part.type === "text") return <MarkdownText />;
    return null;
  }}
</MessagePrimitive.Parts>
```

### Tool UI with by\_name

```
<MessagePrimitive.Parts
  components={{
    Text: MyText,
    tools: {
      by_name: {
        get_weather: ({ result }) => (
          <div className="rounded-lg border p-3">
            <p className="font-medium">Weather</p>
            <p>{result?.temperature}°F, {result?.condition}</p>
          </div>
        ),
      },
      Fallback: ({ toolName, status }) => (
        <div className="text-muted-foreground text-sm">
          {status.type === "running" ? `Running ${toolName}...` : `${toolName} completed`}
        </div>
      ),
    },
  }}
/>
```

### Error Display

```
<MessagePrimitive.Root>
  <MessagePrimitive.Parts />
  <MessagePrimitive.Error>
    <div className="mt-2 rounded-md bg-destructive/10 p-2 text-sm text-destructive">
      Something went wrong. Please try again.
    </div>
  </MessagePrimitive.Error>
</MessagePrimitive.Root>
```

### Error Display with ErrorPrimitive

For more control over error rendering, `ErrorPrimitive` provides a dedicated component that auto-reads the error string from the message status:

```
import { ErrorPrimitive, MessagePrimitive } from "@assistant-ui/react";

<MessagePrimitive.Root>
  <MessagePrimitive.Parts />
  <ErrorPrimitive.Root className="mt-2 rounded-md bg-destructive/10 p-2 text-sm text-destructive" role="alert">
    <ErrorPrimitive.Message />
  </ErrorPrimitive.Root>
</MessagePrimitive.Root>
```

`ErrorPrimitive.Root` renders a `<div>` container with `role="alert"` and `ErrorPrimitive.Message` renders a `<span>` that displays the error text. `Root` always renders. Only `Message` conditionally returns `null` when there is no error. Wrap in `<MessagePrimitive.Error>` if you want the entire block to be conditional. See the [ErrorPrimitive API Reference](/docs/api-reference/primitives/error) for full details.

### Render After Stream Completes

To render content only once the assistant message has finished streaming (a follow-up card, a feedback prompt, a generated component that should not flicker through partial states), gate it with [`AuiIf`](/docs/api-reference/primitives/assistant-if) on `s.message.status`:

```
import { MessagePrimitive, AuiIf } from "@assistant-ui/react";

<MessagePrimitive.Root>
  <MessagePrimitive.Parts />

  <AuiIf
    condition={(s) =>
      s.message.role === "assistant" &&
      s.message.status?.type === "complete"
    }
  >
    <FollowUpCard />
  </AuiIf>
</MessagePrimitive.Root>;
```

`s.message.status` is a discriminated union of `running | requires-action | complete | incomplete`, defined only on assistant messages. The `role === "assistant"` guard keeps the predicate type-safe. For tool-call-driven generative UI that defers rendering inside the part itself, see [Deferred Rendering](/docs/tools/tool-ui#deferred-rendering) in the Generative UI guide.

### Legacy and Unstable APIs

- `MessagePrimitive.Unstable_PartsGrouped` and `MessagePrimitive.Unstable_PartsGroupedByParentId` are unstable APIs for non-adjacent custom grouping.
- `Unstable_PartsGroupedByParentId` is deprecated in favor of `Unstable_PartsGrouped`.

### Role-Based Styling

`MessagePrimitive.Root` sets `data-message-id` automatically but does not set a `data-role` attribute. Style by role in your message components:

```
// In your ThreadPrimitive.Messages children render function:
function UserMessage() {
  return (
    <MessagePrimitive.Root data-role="user" className="flex justify-end">
      <MessagePrimitive.Parts />
    </MessagePrimitive.Root>
  );
}

function AssistantMessage() {
  return (
    <MessagePrimitive.Root data-role="assistant" className="flex justify-start">
      <MessagePrimitive.Parts />
    </MessagePrimitive.Root>
  );
}
```

### Attachments

```
<MessagePrimitive.Root>
  <MessagePrimitive.Attachments>
    {({ attachment }) => {
      if (attachment.type === "image") {
        const imageSrc = attachment.content?.find((part) => part.type === "image")?.image;
        if (!imageSrc) return null;
        return <img src={imageSrc} alt={attachment.name} className="max-w-xs rounded-lg" />;
      }

      if (attachment.type === "document") {
        return (
          <div className="flex items-center gap-2 rounded-lg border p-2 text-sm">
            📄 {attachment.name}
          </div>
        );
      }

      return null;
    }}
  </MessagePrimitive.Attachments>
  <MessagePrimitive.Parts />
</MessagePrimitive.Root>
```

## Relationship to Components

The shadcn [Thread](/docs/ui/thread) component renders user and assistant messages built from these primitives. The pre-built `AssistantMessage` and `UserMessage` components handle text rendering, tool UIs, error display, and action bars, all using `MessagePrimitive` under the hood.

Messages are commonly paired with [ActionBar](/docs/primitives/action-bar) for copy/reload/edit actions and [BranchPicker](/docs/primitives/branch-picker) for navigating between alternative responses.

## API Reference

For full prop details on every part, see the [MessagePrimitive API Reference](/docs/api-reference/primitives/message).

Related:

- [MessagePartPrimitive API Reference](/docs/api-reference/primitives/message-part)
- [ActionBarPrimitive API Reference](/docs/api-reference/primitives/action-bar)
- [BranchPickerPrimitive API Reference](/docs/api-reference/primitives/branch-picker)