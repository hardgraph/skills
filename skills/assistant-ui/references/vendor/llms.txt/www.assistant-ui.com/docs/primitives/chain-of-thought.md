# ChainOfThought
URL: /docs/primitives/chain-of-thought

Collapsible accordion for grouping reasoning steps and tool calls.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

The ChainOfThought primitive is the legacy accordion API for grouped reasoning and tool-call parts. Reasoning models emit reasoning tokens and tool calls before producing a final answer.

> [!warn]
>
> For new grouped reasoning/tool-call UI, use `MessagePrimitive.GroupedParts`. `ChainOfThoughtPrimitive` and `components.ChainOfThought` remain available for maintaining existing code.

Choose one:

**Preview**

\[interactive preview omitted]

**Code**

```
import { MessagePrimitive, groupPartByType } from "@assistant-ui/react";

function AssistantMessage() {
  return (
    <MessagePrimitive.Root>
      <MessagePrimitive.GroupedParts
        groupBy={groupPartByType({
          reasoning: ["group-chainOfThought", "group-reasoning"],
          "tool-call": ["group-chainOfThought", "group-tool"],
        })}
      >
        {({ part, children }) => {
          switch (part.type) {
            case "group-chainOfThought":
              return <ThinkingAccordion>{children}</ThinkingAccordion>;
            case "group-reasoning":
              return <ReasoningGroup>{children}</ReasoningGroup>;
            case "group-tool":
              return <ToolGroup>{children}</ToolGroup>;
            case "text":
              return <MyText />;
            case "reasoning":
              return <MyReasoning {...part} />;
            case "tool-call":
              return part.toolUI ?? <MyToolFallback {...part} />;
            default:
              return null;
          }
        }}
      </MessagePrimitive.GroupedParts>
    </MessagePrimitive.Root>
  );
}
```

## Recommended: GroupedParts

Group reasoning and tool-call parts directly in your assistant message:

```
import { MessagePrimitive, groupPartByType } from "@assistant-ui/react";

<MessagePrimitive.Root>
  <MessagePrimitive.GroupedParts
    groupBy={groupPartByType({
      reasoning: ["group-chainOfThought", "group-reasoning"],
      "tool-call": ["group-chainOfThought", "group-tool"],
    })}
  >
    {({ part, children }) => {
      switch (part.type) {
        case "group-chainOfThought":
          return <ThinkingAccordion>{children}</ThinkingAccordion>;
        case "group-reasoning":
          return <ReasoningGroup>{children}</ReasoningGroup>;
        case "group-tool":
          return <ToolGroup>{children}</ToolGroup>;
        case "text":
          return <MyText />;
        case "reasoning":
          return <MyReasoning {...part} />;
        case "tool-call":
          return part.toolUI ?? <MyToolFallback {...part} />;
        default:
          return null;
      }
    }}
  </MessagePrimitive.GroupedParts>
</MessagePrimitive.Root>
```

### GroupedParts API Reference

- `groupBy`: `(part: PartState, context: GroupByContext) => readonly TKey[] | null` — Maps each part to a group-key path. Adjacent parts that share a prefix coalesce into the same group. Return \`\[]\` (or \`null\`) to leave a part ungrouped. Group keys must start with \`"group-"\` so the renderer's \`switch (part.type)\` can tell groups apart from real part types. \*\*Prefer [groupPartByType](/docs/api-reference/primitives/message#grouppartbytype)\*\* for the common case of mapping by \`part.type\` — it ships a stable memo fingerprint so the tree survives unrelated re-renders. Use an inline function only when the helper isn't expressive enough (e.g. branching on \`part.toolName\` or part metadata). The second argument is a GroupByContext carrying the tool-UI registry, for grouping that depends on it (e.g. standalone tool calls).
- `indicator`: `IndicatorMode` (default `"no-text"`) — Controls emission of the synthetic IndicatorPart — a trailing \`{ part: { type: "indicator", status } }\` render call you handle with \`case "indicator"\` to show loading/status UI.
- `children`: `(info: RenderInfo<TKey>) => ReactNode` — Render function called once per group node, once per leaf part, and (when the \`indicator\` condition is met) once for the trailing IndicatorPart. Switch on \`part.type\`: \`"group-…"\` cases wrap \`children\`; real part types (\`"text"\`, \`"tool-call"\`, …) render the part directly; \`"indicator"\` renders status/loading UI. Leaf parts receive the same [EnrichedPartState](/docs/api-reference/runtimes/message-part-runtime#enrichedpartstate) that \`\<MessagePrimitive.Parts>\` would produce (\`toolUI\`, \`addResult\`, \`resume\`, \`respondToApproval\`, \`dataRendererUI\`).

## Legacy: ChainOfThoughtPrimitive

### Quick Start

Render your normal message parts with `MessagePrimitive.Parts`, then place a `ChainOfThought` component alongside them inside the same `MessagePrimitive.Root` only when maintaining older code that already uses the ChainOfThought primitive.

> [!info]
>
> Runtime setup: primitives require runtime context. Wrap your UI in `AssistantRuntimeProvider` with a runtime (for example `useLocalRuntime(...)`). See [Pick a Runtime](/docs/runtimes/pick-a-runtime).

### Concepts

#### How Grouping Works

`ChainOfThoughtPrimitive.Parts` reads the current message's grouped reasoning and tool-call context from the legacy `components.ChainOfThought` path. New code should use `MessagePrimitive.GroupedParts` instead.

#### Collapsed State

The accordion starts collapsed by default. `AccordionTrigger` toggles between collapsed and expanded. Use `AuiIf` to conditionally render parts based on the collapsed state:

```
import { AuiIf, ChainOfThoughtPrimitive } from "@assistant-ui/react";

<ChainOfThoughtPrimitive.Root>
  <ChainOfThoughtPrimitive.AccordionTrigger>
    Thinking
  </ChainOfThoughtPrimitive.AccordionTrigger>
  <AuiIf condition={(s) => !s.chainOfThought.collapsed}>
    <ChainOfThoughtPrimitive.Parts
      components={{ Reasoning }}
    />
  </AuiIf>
</ChainOfThoughtPrimitive.Root>
```

#### Chevron Indicators

Use `AuiIf` to show directional icons that reflect the current state:

```
import { AuiIf, ChainOfThoughtPrimitive } from "@assistant-ui/react";
import { ChevronDownIcon, ChevronRightIcon } from "lucide-react";

<ChainOfThoughtPrimitive.AccordionTrigger className="flex w-full cursor-pointer items-center gap-2 px-4 py-2 text-sm">
  <AuiIf condition={(s) => s.chainOfThought.collapsed}>
    <ChevronRightIcon className="size-4" />
  </AuiIf>
  <AuiIf condition={(s) => !s.chainOfThought.collapsed}>
    <ChevronDownIcon className="size-4" />
  </AuiIf>
  Thinking
</ChainOfThoughtPrimitive.AccordionTrigger>
```

#### Parts Components

`ChainOfThoughtPrimitive.Parts` accepts a deprecated `components` prop to control how each part type renders:

```
<ChainOfThoughtPrimitive.Parts
  components={{
    Reasoning: ({ text }) => (
      <p className="whitespace-pre-wrap px-4 py-2 text-muted-foreground text-sm italic">
        {text}
      </p>
    ),
    tools: {
      Fallback: ({ toolName, status }) => (
        <div className="px-4 py-2 text-sm">
          {status.type === "running" ? `Running ${toolName}...` : `${toolName} completed`}
        </div>
      ),
    },
    Layout: ({ children }) => (
      <div className="border-t">{children}</div>
    ),
  }}
/>
```

| Prop                        | Type                               | Description                       |
| --------------------------- | ---------------------------------- | --------------------------------- |
| `components.Reasoning`      | `FC<{ text: string }>`             | Renders reasoning parts           |
| `components.tools.Fallback` | `ToolCallMessagePartComponent`     | Fallback for tool-call parts      |
| `components.Layout`         | `ComponentType<PropsWithChildren>` | Wrapper around each rendered part |

### Parts API Reference

#### Root

Container for the chain-of-thought disclosure UI. Renders a `<div>` element unless `asChild` is set.

```
<ChainOfThoughtPrimitive.Root className="rounded-lg border">
  ...
</ChainOfThoughtPrimitive.Root>
```

#### AccordionTrigger

Trigger that toggles the collapsed state. Renders a `<button>` element unless `asChild` is set.

```
<ChainOfThoughtPrimitive.AccordionTrigger className="flex w-full items-center justify-between px-4 py-2 text-sm">
  Thinking
</ChainOfThoughtPrimitive.AccordionTrigger>
```

#### Parts

Renders reasoning and tool-call parts. This component does not track collapsed state internally, so control visibility with `AuiIf` as shown in the patterns below.

```
<ChainOfThoughtPrimitive.Parts
  components={{
    Reasoning: ({ text }) => (
      <p className="whitespace-pre-wrap px-4 py-2 text-muted-foreground text-sm italic">
        {text}
      </p>
    ),
    tools: {
      Fallback: ({ toolName, status }) => (
        <div className="px-4 py-2 text-sm">
          {status.type === "running" ? `Running ${toolName}...` : `${toolName} completed`}
        </div>
      ),
    },
  }}
/>
```

- `components?`: `ChainOfThoughtPartsComponentConfig` (deprecated: Use the children render function instead.)
- `children?`: `(value: { part: PartState; }) => ReactNode` — Render function called for each part. Receives the part.

### Patterns

#### Minimal Accordion

```
function ChainOfThought() {
  return (
    <ChainOfThoughtPrimitive.Root className="my-2 rounded-lg border">
      <ChainOfThoughtPrimitive.AccordionTrigger className="flex w-full cursor-pointer items-center gap-2 px-4 py-2 font-medium text-sm hover:bg-muted/50">
        Thinking
      </ChainOfThoughtPrimitive.AccordionTrigger>
      <AuiIf condition={(s) => !s.chainOfThought.collapsed}>
        <ChainOfThoughtPrimitive.Parts
          components={{
            Reasoning: ({ text }) => (
              <p className="whitespace-pre-wrap px-4 py-2 text-muted-foreground text-sm italic">
                {text}
              </p>
            ),
          }}
        />
      </AuiIf>
    </ChainOfThoughtPrimitive.Root>
  );
}
```

#### With Tool Calls

```
function ChainOfThought() {
  return (
    <ChainOfThoughtPrimitive.Root className="my-2 rounded-lg border">
      <ChainOfThoughtPrimitive.AccordionTrigger className="flex w-full cursor-pointer items-center gap-2 px-4 py-2 font-medium text-sm hover:bg-muted/50">
        Thinking
      </ChainOfThoughtPrimitive.AccordionTrigger>
      <AuiIf condition={(s) => !s.chainOfThought.collapsed}>
        <ChainOfThoughtPrimitive.Parts
          components={{
            Reasoning: ({ text }) => (
              <p className="whitespace-pre-wrap px-4 py-2 text-muted-foreground text-sm italic">
                {text}
              </p>
            ),
            tools: {
              Fallback: ({ toolName, status }) => (
                <div className="flex items-center gap-2 px-4 py-2 text-sm">
                  <span className="font-medium">{toolName}</span>
                  <span className="text-muted-foreground">
                    {status.type === "running" ? "running..." : "done"}
                  </span>
                </div>
              ),
            },
            Layout: ({ children }) => (
              <div className="border-t">{children}</div>
            ),
          }}
        />
      </AuiIf>
    </ChainOfThoughtPrimitive.Root>
  );
}
```

### Relationship to Components

The [Chain of Thought guide](/docs/guides/chain-of-thought) covers end-to-end setup with `MessagePrimitive.GroupedParts`, including backend configuration with reasoning models. See the complete [with-chain-of-thought example](https://github.com/assistant-ui/assistant-ui/tree/main/examples/with-chain-of-thought) for a full working implementation.

### API Reference

For the complete guide including backend configuration, see [Chain of Thought](/docs/guides/chain-of-thought). For prop details, see the [ChainOfThoughtPrimitive source](https://github.com/assistant-ui/assistant-ui/tree/main/packages/react/src/primitives/chainOfThought).

Related:

- [Chain of Thought Guide](/docs/guides/chain-of-thought)
- [MessagePrimitive](/docs/primitives/message)