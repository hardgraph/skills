# Reasoning
URL: /docs/ui/reasoning

Collapsible UI for displaying AI reasoning and thinking messages.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

## Getting Started

1. ### Add `reasoning`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/reasoning
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/reasoning.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react class-variance-authority tw-shimmer @assistant-ui/react-markdown remark-gfm
   ```

   ```bash
   npx shadcn@latest add collapsible tooltip button
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/reasoning.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/reasoning.tsx)
   - [components/assistant-ui/markdown-text.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/markdown-text.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/reasoning.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/reasoning.tsx \
     -o components/assistant-ui/markdown-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/markdown-text.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx
   ```

   This adds a `/components/assistant-ui/reasoning.tsx` file to your project.

2. ### Use in your application

   Previously, reasoning parts were rendered via `components.Reasoning` and grouped via `components.ReasoningGroup` on `MessagePrimitive.Parts`. Both are deprecated; `MessagePrimitive.GroupedParts` is the supported replacement, and the highlighted lines below show the new pieces.

   Render reasoning parts through `MessagePrimitive.GroupedParts`. Group consecutive reasoning parts with `"group-reasoning"`, then compose `ReasoningRoot`, `ReasoningTrigger`, `ReasoningContent`, and `ReasoningText` around the grouped children.

   While reasoning is streaming, `part.status.type === "running"`. Pass that to `streaming` so the accordion auto-opens during streaming with a bottom-pinned live preview of the newest tokens, and permanently defers to the first manual toggle. `streaming` only holds the disclosure open; when it ends the accordion returns to `defaultOpen`, which is `false` by default and therefore collapses. Pass `defaultOpen` to keep reasoning expanded once the run finishes.

   ```
   import { MessagePrimitive, groupPartByType } from "@assistant-ui/react";
   import { MarkdownText } from "@/components/assistant-ui/markdown-text";
   import { ToolFallback } from "@/components/assistant-ui/tool-fallback";
   import {
     Reasoning,
     ReasoningContent,
     ReasoningRoot,
     ReasoningText,
     ReasoningTrigger,
   } from "@/components/assistant-ui/reasoning";

   const AssistantMessage: FC = () => {
     return (
       <MessagePrimitive.Root className="...">
         <div className="...">
           <MessagePrimitive.GroupedParts
             groupBy={groupPartByType({
               reasoning: ["group-reasoning"],
             })}
           >
             {({ part, children }) => {
               switch (part.type) {
                 case "group-reasoning": {
                   const running = part.status.type === "running";
                   return ( 
                     <ReasoningRoot streaming={running}>
                       <ReasoningTrigger active={running} />
                       <ReasoningContent aria-busy={running}>
                         <ReasoningText>{children}</ReasoningText>
                       </ReasoningContent>
                     </ReasoningRoot>
                   );
                 }
                 case "text":
                   return <MarkdownText />;
                 case "reasoning":
                   return <Reasoning {...part} />;
                 case "tool-call":
                   return part.toolUI ?? <ToolFallback {...part} />;
                 default:
                   return null;
               }
             }}
           </MessagePrimitive.GroupedParts>
         </div>
         <AssistantActionBar />
         <BranchPicker className="..." />
       </MessagePrimitive.Root>
     );
   };
   ```

   `GroupedParts` calls your render function for both group nodes and leaf parts. The `case "group-reasoning"` branch renders the collapsible shell and must render `{children}`; that is where the individual reasoning parts get placed. The `case "reasoning"` branch renders each individual reasoning part inside that shell and must not render `children`. Removing either case breaks rendering.

## How It Works

The component consists of two parts:

1. `Reasoning`: Renders individual reasoning message part content (with markdown support)
2. Composable group pieces (`ReasoningRoot`, `ReasoningTrigger`, `ReasoningContent`, `ReasoningText`): Wrap grouped reasoning children in a collapsible container

Consecutive reasoning parts are grouped by `MessagePrimitive.GroupedParts`. Use the composable API below to control the grouped layout.

> When using the composable API, `ReasoningText` is a plain container. Add `<MarkdownText />` for markdown rendering.

## Variants

Use the `variant` prop on `ReasoningRoot` to change the visual style:

```
<ReasoningRoot variant="outline">...</ReasoningRoot>
<ReasoningRoot variant="ghost">...</ReasoningRoot>
<ReasoningRoot variant="muted">...</ReasoningRoot>
```

| Variant   | Description              |
| --------- | ------------------------ |
| `outline` | Rounded border (default) |
| `ghost`   | No additional styling    |
| `muted`   | Muted background         |

## Legacy ReasoningGroup

`ReasoningGroup` is kept for existing code that still uses the deprecated `components.ReasoningGroup` prop on `MessagePrimitive.Parts`. New code should use `MessagePrimitive.GroupedParts` and compose the root/trigger/content pieces directly.

\[interactive preview omitted]

```
import { ReasoningGroup } from "@/components/assistant-ui/reasoning";

const ReasoningGroupImpl: ReasoningGroupComponent = ({
  children,
  startIndex,
  endIndex,
}) => {
  const isReasoningStreaming = useAuiState((s) => {
    if (s.message.status?.type !== "running") return false;
    const lastIndex = s.message.parts.length - 1;
    if (lastIndex < 0) return false;
    const lastType = s.message.parts[lastIndex]?.type;
    if (lastType !== "reasoning") return false;
    return lastIndex >= startIndex && lastIndex <= endIndex;
  });

  return (
    <ReasoningRoot streaming={isReasoningStreaming}>
      <ReasoningTrigger active={isReasoningStreaming} />
      <ReasoningContent aria-busy={isReasoningStreaming}>
        <ReasoningText>{children}</ReasoningText>
      </ReasoningContent>
    </ReasoningRoot>
  );
};
```

## API Reference

### Composable API

All sub-components are exported for custom layouts:

| Component          | Description                            |
| ------------------ | -------------------------------------- |
| `ReasoningRoot`    | Collapsible container with scroll lock |
| `ReasoningTrigger` | Button with icon, label, and shimmer   |
| `ReasoningContent` | Animated collapsible content wrapper   |
| `ReasoningText`    | Text wrapper with slide/fade animation |
| `ReasoningFade`    | Gradient fade overlay at bottom        |

```
import {
  ReasoningRoot,
  ReasoningTrigger,
  ReasoningContent,
  ReasoningText,
  ReasoningFade,
} from "@/components/assistant-ui/reasoning";

<ReasoningRoot variant="muted">
  <ReasoningTrigger active={isStreaming} />
  <ReasoningContent>
    <ReasoningText>{children}</ReasoningText>
  </ReasoningContent>
</ReasoningRoot>
```

## Related Components

- [ToolGroup](/docs/ui/tool-group) - Similar grouping pattern for tool calls
- [PartGrouping](/docs/ui/part-grouping) - Advanced grouping options for message parts