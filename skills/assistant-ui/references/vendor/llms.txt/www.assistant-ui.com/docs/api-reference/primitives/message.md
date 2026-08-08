# MessagePrimitive
URL: /docs/api-reference/primitives/message

Message primitives for rendering assistant and user turns, message parts, attachments, actions, editing, and branch controls.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [Message](/docs/primitives/message).

## Anatomy

```
import { MessagePrimitive } from "@assistant-ui/react";

const UserMessage = () => (
  <MessagePrimitive.Root>
    User: <MessagePrimitive.Parts />
    <BranchPicker />
    <ActionBar />
  </MessagePrimitive.Root>
);

const AssistantMessage = () => (
  <MessagePrimitive.Root>
    Assistant: <MessagePrimitive.Parts />
    <BranchPicker />
    <ActionBar />
  </MessagePrimitive.Root>
);
```

## API Reference

### Root

The root container component for a message. This component provides the foundational wrapper for message content and handles hover state management for the message. It automatically tracks when the user is hovering over the message, which can be used by child components like action bars. When \`turnAnchor="top"\` is set on the viewport, this component automatically registers itself as the top-anchor user message (when it's the previous user message) or as the top-anchor target (when it's the streaming assistant response). No additional component is required.

This primitive renders a `<div>` element unless `asChild` is set.

```
<MessagePrimitive.Root>
  <MessagePrimitive.Content />
  <ActionBarPrimitive.Root>
    <ActionBarPrimitive.Copy />
    <ActionBarPrimitive.Edit />
  </ActionBarPrimitive.Root>
</MessagePrimitive.Root>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Parts

Renders the parts of a message with web-specific default components.

- `components?`: `StandardComponents | ChainOfThoughtComponents` — Component configuration for rendering different types of message content. Use either \`Reasoning\`/\`tools\`/\`ToolGroup\`/\`ReasoningGroup\` for standard rendering, or \`ChainOfThought\` to group all reasoning and tool-call parts into a single collapsible component. These two modes are mutually exclusive.

  - `Empty?`: `EmptyMessagePartComponent` — Component for rendering empty messages

  - `Text?`: `TextMessagePartComponent` — Component for rendering text content

  - `Source?`: `SourceMessagePartComponent` — Component for rendering source content

  - `Image?`: `ImageMessagePartComponent` — Component for rendering image content

  - `File?`: `FileMessagePartComponent` — Component for rendering file content

  - `Unstable_Audio?`: `Unstable_AudioMessagePartComponent` (deprecated: Render audio through the \`File\` slot instead, branching on an \`audio/\*\` mime type.) — Component for rendering audio content.

  - `data?`: `DataConfig` — Configuration for data part rendering

    - `by_name?`: `Record<string, DataMessagePartComponent | undefined>` — Map data event names to specific components
    - `Fallback?`: `DataMessagePartComponent` — Fallback component for unmatched data events

  - `Quote?`: `QuoteMessagePartComponent` — Component for rendering a quoted message reference (from metadata, not parts)

  - `generativeUI?`: `MessagePrimitivePartsProps["components"]["generativeUI"]` — Configuration for generative-ui part rendering. \`components\` is the consumer-provided allowlist of React components the agent's JSON spec is permitted to render. Any name not present in the registry is rejected with a typed \`GenerativeUIRenderError\` — this is the security boundary in the same-realm rendering path.

    - `components`: `GenerativeUIComponentRegistry` — The component allowlist (the security boundary).
    - `Fallback?`: `ComponentType<{ component: string; props?: unknown }>` — Optional fallback for unknown component names.

  - `Reasoning?`: `ReasoningMessagePartComponent` — Component for rendering reasoning content (typically hidden)

  - `tools?`: `ToolsConfig` — Configuration for tool call rendering

    - `by_name?`: `Record<string, ToolCallMessagePartComponent | undefined>` — Map of tool names to their specific components
    - `Fallback?`: `ComponentType<ToolCallMessagePartProps>` — Fallback component for unregistered tools
    - `Override`: `ComponentType<ToolCallMessagePartProps>` — Override component that handles all tool calls

  - `ToolGroup?`: `ComponentType< PropsWithChildren<{ startIndex: number; endIndex: number }> >` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a custom \`groupBy\` instead.) — Component for rendering grouped consecutive tool calls.

  - `ReasoningGroup?`: `ReasoningGroupComponent` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a custom \`groupBy\` instead.) — Component for rendering grouped reasoning parts.

  - `ChainOfThought`: `ComponentType` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a \`groupBy\` that returns \`\["group-thought", ...]\` for reasoning and tool-call parts. See \`@assistant-ui/ui\` for a worked example.)

- `unstable_showEmptyOnNonTextEnd`: `boolean` (default `true`) — When enabled, shows the Empty component if the last part in the message is anything other than Text or Reasoning.

- `children?`: `(value: { part: EnrichedPartState }) => ReactNode` — Render function called for each part. Receives the enriched part state.

### PartByIndex

Renders a single message part at the specified index.

- `index`: `number`

- `components?`: `MessagePrimitiveParts.Props["components"]`

  - `Empty?`: `EmptyMessagePartComponent` — Component for rendering empty messages

  - `Text?`: `TextMessagePartComponent` — Component for rendering text content

  - `Source?`: `SourceMessagePartComponent` — Component for rendering source content

  - `Image?`: `ImageMessagePartComponent` — Component for rendering image content

  - `File?`: `FileMessagePartComponent` — Component for rendering file content

  - `Unstable_Audio?`: `Unstable_AudioMessagePartComponent` (deprecated: Render audio through the \`File\` slot instead, branching on an \`audio/\*\` mime type.) — Component for rendering audio content.

  - `data?`: `DataConfig` — Configuration for data part rendering

    - `by_name?`: `Record<string, DataMessagePartComponent | undefined>` — Map data event names to specific components
    - `Fallback?`: `DataMessagePartComponent` — Fallback component for unmatched data events

  - `Quote?`: `QuoteMessagePartComponent` — Component for rendering a quoted message reference (from metadata, not parts)

  - `generativeUI?`: `MessagePrimitivePartByIndexProps["components"]["generativeUI"]` — Configuration for generative-ui part rendering. \`components\` is the consumer-provided allowlist of React components the agent's JSON spec is permitted to render. Any name not present in the registry is rejected with a typed \`GenerativeUIRenderError\` — this is the security boundary in the same-realm rendering path.

    - `components`: `GenerativeUIComponentRegistry` — The component allowlist (the security boundary).
    - `Fallback?`: `ComponentType<{ component: string; props?: unknown }>` — Optional fallback for unknown component names.

  - `Reasoning?`: `ReasoningMessagePartComponent` — Component for rendering reasoning content (typically hidden)

  - `tools?`: `ToolsConfig` — Configuration for tool call rendering

    - `by_name?`: `Record<string, ToolCallMessagePartComponent | undefined>` — Map of tool names to their specific components
    - `Fallback?`: `ComponentType<ToolCallMessagePartProps>` — Fallback component for unregistered tools
    - `Override`: `ComponentType<ToolCallMessagePartProps>` — Override component that handles all tool calls

  - `ToolGroup?`: `ComponentType< PropsWithChildren<{ startIndex: number; endIndex: number }> >` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a custom \`groupBy\` instead.) — Component for rendering grouped consecutive tool calls.

  - `ReasoningGroup?`: `ReasoningGroupComponent` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a custom \`groupBy\` instead.) — Component for rendering grouped reasoning parts.

  - `ChainOfThought`: `ComponentType` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a \`groupBy\` that returns \`\["group-thought", ...]\` for reasoning and tool-call parts. See \`@assistant-ui/ui\` for a worked example.)

### Content

- `components?`: `StandardComponents | ChainOfThoughtComponents` — Component configuration for rendering different types of message content. Use either \`Reasoning\`/\`tools\`/\`ToolGroup\`/\`ReasoningGroup\` for standard rendering, or \`ChainOfThought\` to group all reasoning and tool-call parts into a single collapsible component. These two modes are mutually exclusive.

  - `Empty?`: `EmptyMessagePartComponent` — Component for rendering empty messages

  - `Text?`: `TextMessagePartComponent` — Component for rendering text content

  - `Source?`: `SourceMessagePartComponent` — Component for rendering source content

  - `Image?`: `ImageMessagePartComponent` — Component for rendering image content

  - `File?`: `FileMessagePartComponent` — Component for rendering file content

  - `Unstable_Audio?`: `Unstable_AudioMessagePartComponent` (deprecated: Render audio through the \`File\` slot instead, branching on an \`audio/\*\` mime type.) — Component for rendering audio content.

  - `data?`: `DataConfig` — Configuration for data part rendering

    - `by_name?`: `Record<string, DataMessagePartComponent | undefined>` — Map data event names to specific components
    - `Fallback?`: `DataMessagePartComponent` — Fallback component for unmatched data events

  - `Quote?`: `QuoteMessagePartComponent` — Component for rendering a quoted message reference (from metadata, not parts)

  - `generativeUI?`: `MessagePrimitiveContentProps["components"]["generativeUI"]` — Configuration for generative-ui part rendering. \`components\` is the consumer-provided allowlist of React components the agent's JSON spec is permitted to render. Any name not present in the registry is rejected with a typed \`GenerativeUIRenderError\` — this is the security boundary in the same-realm rendering path.

    - `components`: `GenerativeUIComponentRegistry` — The component allowlist (the security boundary).
    - `Fallback?`: `ComponentType<{ component: string; props?: unknown }>` — Optional fallback for unknown component names.

  - `Reasoning?`: `ReasoningMessagePartComponent` — Component for rendering reasoning content (typically hidden)

  - `tools?`: `ToolsConfig` — Configuration for tool call rendering

    - `by_name?`: `Record<string, ToolCallMessagePartComponent | undefined>` — Map of tool names to their specific components
    - `Fallback?`: `ComponentType<ToolCallMessagePartProps>` — Fallback component for unregistered tools
    - `Override`: `ComponentType<ToolCallMessagePartProps>` — Override component that handles all tool calls

  - `ToolGroup?`: `ComponentType< PropsWithChildren<{ startIndex: number; endIndex: number }> >` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a custom \`groupBy\` instead.) — Component for rendering grouped consecutive tool calls.

  - `ReasoningGroup?`: `ReasoningGroupComponent` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a custom \`groupBy\` instead.) — Component for rendering grouped reasoning parts.

  - `ChainOfThought`: `ComponentType` (deprecated: Use \`\<MessagePrimitive.GroupedParts>\` with a \`groupBy\` that returns \`\["group-thought", ...]\` for reasoning and tool-call parts. See \`@assistant-ui/ui\` for a worked example.)

- `unstable_showEmptyOnNonTextEnd`: `boolean` (default `true`) — When enabled, shows the Empty component if the last part in the message is anything other than Text or Reasoning.

- `children?`: `(value: { part: EnrichedPartState }) => ReactNode` — Render function called for each part. Receives the enriched part state.

### If

> [!warn]
>
> **Deprecated.** Use \`\<AuiIf condition={(s) => s.message...} />\` instead.

- `system?`: `boolean`
- `user?`: `boolean`
- `assistant?`: `boolean`
- `speaking?`: `boolean`
- `hasBranches?`: `boolean`
- `copied?`: `boolean`
- `lastOrHover?`: `boolean`
- `last?`: `boolean`
- `hasAttachments?`: `boolean`
- `hasContent?`: `boolean`
- `submittedFeedback?`: `"positive" | "negative" | null`

### Attachments

- `components?`: `MessageAttachmentsComponentConfig` (deprecated: Use the children render function instead.)

  - `Image?`: `ComponentType`
  - `Document?`: `ComponentType`
  - `File?`: `ComponentType`
  - `Attachment?`: `ComponentType`

- `children?`: `(value: { attachment: CompleteAttachment }) => ReactNode` — Render function called for each attachment. Receives the attachment.

### AttachmentByIndex

Renders a single attachment at the specified index within the current message.

- `index`: `number`

- `components?`: `MessageAttachmentsComponentConfig`

  - `Image?`: `ComponentType`
  - `Document?`: `ComponentType`
  - `File?`: `ComponentType`
  - `Attachment?`: `ComponentType`

### Quote

- `children`: `(value: QuoteInfo) => ReactNode` — Render function called when a quote is present. Receives quote info.

### Error

### GroupedParts

Groups adjacent message parts into a tree of coalesced runs and renders each node — group or part — through a single \`children\` function. The render function receives \`{ part, children }\` where \`part.type\` is either a \`"group-…"\` literal (for a group, \`children\` is the recursively-rendered subtree) or a real part type (\`"text"\`, \`"tool-call"\`, …) for a leaf (\`children\` is a sentinel that throws if rendered — use \`part.type\` to distinguish).

```
<MessagePrimitive.GroupedParts
  groupBy={groupPartByType({
    reasoning: ["group-thought", "group-reasoning"],
    "tool-call": ["group-thought", "group-tool"],
  })}
>
  {({ part, children }) => {
    switch (part.type) {
      case "group-thought":   return <Thought>{children}</Thought>;
      case "group-reasoning": return <Reasoning>{children}</Reasoning>;
      case "group-tool":      return <ToolStack>{children}</ToolStack>;
      case "text":            return <MarkdownText />;
      case "tool-call":       return part.toolUI ?? <ToolFallback {...part} />;
      case "indicator":       return <LoadingDots />;
      default:                return null;
    }
  }}
</MessagePrimitive.GroupedParts>
```

- `groupBy`: `( part: PartState, context: GroupByContext, ) => readonly TKey[] | null` — Maps each part to a group-key path. Adjacent parts that share a prefix coalesce into the same group. Return \`\[]\` (or \`null\`) to leave a part ungrouped. Group keys must start with \`"group-"\` so the renderer's \`switch (part.type)\` can tell groups apart from real part types. \*\*Prefer [groupPartByType](/docs/api-reference/primitives/message#grouppartbytype)\*\* for the common case of mapping by \`part.type\` — it ships a stable memo fingerprint so the tree survives unrelated re-renders. Use an inline function only when the helper isn't expressive enough (e.g. branching on \`part.toolName\` or part metadata). The second argument is a GroupByContext carrying the tool-UI registry, for grouping that depends on it (e.g. standalone tool calls).
- `indicator`: `IndicatorMode` (default `"no-text"`) — Controls emission of the synthetic IndicatorPart — a trailing \`{ part: { type: "indicator", status } }\` render call you handle with \`case "indicator"\` to show loading/status UI.
- `children`: `(info: RenderInfo<TKey>) => ReactNode` — Render function called once per group node, once per leaf part, and (when the \`indicator\` condition is met) once for the trailing IndicatorPart. Switch on \`part.type\`: \`"group-…"\` cases wrap \`children\`; real part types (\`"text"\`, \`"tool-call"\`, …) render the part directly; \`"indicator"\` renders status/loading UI. Leaf parts receive the same [EnrichedPartState](/docs/api-reference/runtimes/message-part-runtime#enrichedpartstate) that \`\<MessagePrimitive.Parts>\` would produce (\`toolUI\`, \`addResult\`, \`resume\`, \`respondToApproval\`, \`dataRendererUI\`).

### GenerativeUI

Renders a generative-ui message part using a consumer-provided allowlist. The agent emits a \`generative-ui\` message part containing a JSON spec (see \[GenerativeUISpec]\(/docs/api-reference/generative-ui/spec#generativeuispec)). This primitive walks the spec and resolves each \`component\` name against the allowlist. Names not in the allowlist throw \[GenerativeUIRenderError]\(/docs/api-reference/generative-ui/rendering#generativeuirendererror) unless a \`Fallback\` is provided. Stream-friendly: a partial spec renders progressively as it is filled in.

```
<MessagePrimitive.GenerativeUI
  components={{ Card: MyCard, Button: MyButton }}
/>
```

- `components`: `GenerativeUIComponentRegistry` — The component allowlist. Keys are the names referenced in the spec (e.g. \`"Card"\`, \`"Button"\`), values are the React components. This is the security boundary — any name not in the allowlist is rejected with [GenerativeUIRenderError](/docs/api-reference/generative-ui/rendering#generativeuirendererror).
- `spec?`: `GenerativeUISpec` — Optional override spec. If omitted, the primitive reads the \`generative-ui\` part from the surrounding \`MessagePartProvider\` / \`PartByIndexProvider\` context.
  - `root`: `GenerativeUINode | readonly GenerativeUINode[]` — Root node(s) to render.
- `Fallback?`: `ComponentType<{ component: string; props?: unknown }>` — Optional fallback for unknown component names.

### Unstable\_PartsGrouped

> [!warn]
>
> **Deprecated.** Prefer \`\<MessagePrimitive.GroupedParts>\` for adjacent grouping — it dispatches all rendering through one \`switch (part.type)\` and supports nested group paths. Keep this primitive only for non-adjacent clustering (e.g., gathering parts with the same parent-id across the message).

Renders the parts of a message grouped by a custom grouping function. This component allows you to group message parts based on any criteria you define. The grouping function receives all message parts and returns an array of groups, where each group has a key and an array of part indices.

```
// Group by parent ID (default behavior)
<MessagePrimitive.Unstable_PartsGrouped
  components={{
    Text: ({ text }) => <p className="message-text">{text}</p>,
    Image: ({ image }) => <img src={image} alt="Message image" />,
    Group: ({ groupKey, indices, children }) => {
      if (!groupKey) return <>{children}</>;
      return (
        <div className="parent-group border rounded p-4">
          <h4>Parent ID: {groupKey}</h4>
          {children}
        </div>
      );
    }
  }}
/>
```

- `groupingFunction`: `GroupingFunction` — Function that takes an array of message parts and returns an array of groups. Each group contains a key (for identification) and an array of indices.

- `components?`: `MessagePrimitiveUnstable_PartsGroupedProps["components"]` — Component configuration for rendering different types of message content. You can provide custom components for each content type (text, image, file, etc.) and configure tool rendering behavior. If not provided, default components will be used.

  - `Empty?`: `EmptyMessagePartComponent` — Component for rendering empty messages

  - `Text?`: `TextMessagePartComponent` — Component for rendering text content

  - `Reasoning?`: `ReasoningMessagePartComponent` — Component for rendering reasoning content (typically hidden)

  - `Source?`: `SourceMessagePartComponent` — Component for rendering source content

  - `Image?`: `ImageMessagePartComponent` — Component for rendering image content

  - `File?`: `FileMessagePartComponent` — Component for rendering file content

  - `Unstable_Audio?`: `Unstable_AudioMessagePartComponent` (deprecated: Render audio through the \`File\` slot instead, branching on an \`audio/\*\` mime type.) — Component for rendering audio content.

  - `data?`: `MessagePrimitiveUnstable_PartsGroupedProps["components"]["data"]` — Configuration for data part rendering

    - `by_name?`: `Record<string, DataMessagePartComponent | undefined>` — Map data event names to specific components
    - `Fallback?`: `DataMessagePartComponent` — Fallback component for unmatched data events

  - `tools?`: `MessagePrimitiveUnstable_PartsGroupedProps["components"]["tools"]` — Configuration for tool call rendering

    - `by_name?`: `Record<string, ToolCallMessagePartComponent | undefined>` — Map of tool names to their specific components
    - `Fallback?`: `ComponentType<ToolCallMessagePartProps>` — Fallback component for unregistered tools
    - `Override`: `ComponentType<ToolCallMessagePartProps>` — Override component that handles all tool calls

  - `Group?`: `ComponentType< PropsWithChildren<{ groupKey: string | undefined; indices: number[]; }> >` — Component for rendering grouped message parts. When provided, this component will automatically wrap message parts that share the same group key as determined by the groupingFunction. The component receives: - \`groupKey\`: The group key (or undefined for ungrouped parts) - \`indices\`: Array of indices for the parts in this group - \`children\`: The rendered message part components

### Unstable\_PartsGroupedByParentId

> [!warn]
>
> **Deprecated.** Use MessagePrimitive.Unstable\_PartsGrouped instead for more flexibility

Renders the parts of a message grouped by their parent ID. This is a convenience wrapper around Unstable\_PartsGrouped with parent ID grouping.

- `components?`: `MessagePrimitiveUnstable_PartsGroupedByParentIdProps props["components"]` — Component configuration for rendering different types of message content. You can provide custom components for each content type (text, image, file, etc.) and configure tool rendering behavior. If not provided, default components will be used.

  - `Empty?`: `EmptyMessagePartComponent` — Component for rendering empty messages

  - `Text?`: `TextMessagePartComponent` — Component for rendering text content

  - `Reasoning?`: `ReasoningMessagePartComponent` — Component for rendering reasoning content (typically hidden)

  - `Source?`: `SourceMessagePartComponent` — Component for rendering source content

  - `Image?`: `ImageMessagePartComponent` — Component for rendering image content

  - `File?`: `FileMessagePartComponent` — Component for rendering file content

  - `Unstable_Audio?`: `Unstable_AudioMessagePartComponent` (deprecated: Render audio through the \`File\` slot instead, branching on an \`audio/\*\` mime type.) — Component for rendering audio content.

  - `data?`: `MessagePrimitiveUnstable_PartsGroupedByParentIdProps props["components"]["data"]` — Configuration for data part rendering

    - `by_name?`: `Record<string, DataMessagePartComponent | undefined>` — Map data event names to specific components
    - `Fallback?`: `DataMessagePartComponent` — Fallback component for unmatched data events

  - `tools?`: `MessagePrimitiveUnstable_PartsGroupedByParentIdProps props["components"]["tools"]` — Configuration for tool call rendering

    - `by_name?`: `Record<string, ToolCallMessagePartComponent | undefined>` — Map of tool names to their specific components
    - `Fallback?`: `ComponentType<ToolCallMessagePartProps>` — Fallback component for unregistered tools
    - `Override`: `ComponentType<ToolCallMessagePartProps>` — Override component that handles all tool calls

  - `Group?`: `ComponentType< PropsWithChildren<{ groupKey: string | undefined; indices: number[]; }> >` — Component for rendering grouped message parts. When provided, this component will automatically wrap message parts that share the same group key as determined by the groupingFunction. The component receives: - \`groupKey\`: The group key (or undefined for ungrouped parts) - \`indices\`: Array of indices for the parts in this group - \`children\`: The rendered message part components

### groupPartByType

Build a `groupBy` from a `part.type → group-key path` lookup. Parts whose type isn't in the map are left ungrouped. The returned function carries a stable GROUPBY\_MEMO\_KEY fingerprint so `<MessagePrimitive.GroupedParts>` can memoize its tree across renders.

The synthetic `"standalone-tool-call"` key matches tool calls that should render outside the chain-of-thought grouping. MCP-app calls are detected from the part alone; the registry-driven cases (human tools, the generative-UI tool, `display: "standalone"` opt-ins) are resolved from the GroupByContext that `<MessagePrimitive.GroupedParts>` passes to the `groupBy` function — the helper needs nothing threaded into it.

```
<MessagePrimitive.GroupedParts
  groupBy={groupPartByType({
    reasoning: ["group-thought", "group-reasoning"],
    "tool-call": ["group-thought", "group-tool"],
    "standalone-tool-call": [],
  })}
>
  {({ part, children }) => { ... }}
</MessagePrimitive.GroupedParts>
```

```
const groupPartByType: <TKey extends `group-${string}`>(map: Partial<Readonly<Record<GroupPartType, readonly TKey[]>>>) => ((part: PartState, context?: GroupByContext) => readonly TKey[]);
```