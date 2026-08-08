# ToolFallback
URL: /docs/ui/tool-fallback

Default UI component for tools without dedicated custom renderers.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

## Getting Started

1. ### Add `tool-fallback`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/tool-fallback
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/tool-fallback.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react tw-shimmer
   ```

   ```bash
   npx shadcn@latest add button collapsible
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/tool-fallback.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/tool-fallback.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx
   ```

   This adds a `/components/assistant-ui/tool-fallback.tsx` file to your project, which you can adjust as needed.

2. ### Use it in your application

   Pass the `ToolFallback` component to the `MessagePrimitive.Parts` component

   ```
   import { ToolFallback } from "@/components/assistant-ui/tool-fallback";

   const AssistantMessage = () => {
     return (
       <MessagePrimitive.Root>
         <MessagePrimitive.Parts>
           {({ part }) => {
             if (part.type === "tool-call") return <ToolFallback {...part} />;
             return null;
           }}
         </MessagePrimitive.Parts>
       </MessagePrimitive.Root>
     );
   };
   ```

## Examples

### Streaming Demo

Interactive demo showing the full tool call lifecycle: running → complete.

\[interactive preview omitted]

### Running State

Shows a loading spinner and shimmer animation while the tool is executing.

\[interactive preview omitted]

### Cancelled State

Shows a muted appearance when a tool call was cancelled.

\[interactive preview omitted]

### Approval State

Shows the default Allow / Deny buttons rendered when a tool call enters `requires-action`. A tool call whose approval gate is already decided renders no buttons. The block auto-expands so the decision is visible without a click. Edit your project's copy of `tool-fallback.tsx` to add trust escalation, edit-args, custom labels, or analytics hooks — the shadcn philosophy is you own the file.

\[interactive preview omitted]

## Composable API

All sub-components are exported for custom layouts:

| Component               | Description                                                                                                                               |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `ToolFallback.Root`     | Collapsible container with scroll lock                                                                                                    |
| `ToolFallback.Trigger`  | Header with tool name, status icon, and shimmer                                                                                           |
| `ToolFallback.Content`  | Animated collapsible content wrapper                                                                                                      |
| `ToolFallback.Args`     | Displays tool arguments                                                                                                                   |
| `ToolFallback.Result`   | Displays tool execution result                                                                                                            |
| `ToolFallback.Error`    | Displays error or cancellation messages                                                                                                   |
| `ToolFallback.Approval` | Renders Allow / Deny buttons for `requires-action` tools until a decision is recorded; wires `resume` / `addResult` / `respondToApproval` |

```
import {
  ToolFallback,
  ToolFallbackRoot,
  ToolFallbackTrigger,
  ToolFallbackContent,
  ToolFallbackArgs,
  ToolFallbackResult,
  ToolFallbackError,
  ToolFallbackApproval,
} from "@/components/assistant-ui/tool-fallback";

// Compound component syntax
<ToolFallback.Root>
  <ToolFallback.Trigger toolName="get_weather" status={status} />
  <ToolFallback.Content>
    <ToolFallback.Error status={status} />
    <ToolFallback.Args argsText={argsText} />
    <ToolFallback.Approval
      addResult={addResult}
      resume={resume}
      interrupt={interrupt}
      approval={approval}
      respondToApproval={respondToApproval}
    />
    <ToolFallback.Result result={result} />
  </ToolFallback.Content>
</ToolFallback.Root>
```

## Related Components

- [ToolGroup](/docs/ui/tool-group) - Group consecutive tool calls together