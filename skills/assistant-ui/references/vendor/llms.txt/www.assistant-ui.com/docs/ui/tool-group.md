# ToolGroup
URL: /docs/ui/tool-group

Wrapper for consecutive tool calls with collapsible and styled options.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A wrapper component that groups consecutive tool calls together, displaying them in a collapsible container with auto-expand behavior during streaming.

\[interactive preview omitted]

## Getting Started

1. ### Add `tool-group` and `tool-fallback`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/tool-group @assistant-ui/tool-fallback
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/tool-group.json https://r.assistant-ui.com/base/tool-fallback.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react class-variance-authority tw-shimmer
   ```

   ```bash
   npx shadcn@latest add collapsible button
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/tool-group.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-group.tsx)
   - [components/assistant-ui/tool-fallback.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/tool-group.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-group.tsx \
     -o components/assistant-ui/tool-fallback.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx
   ```

   This adds `/components/assistant-ui/tool-group.tsx` and `/components/assistant-ui/tool-fallback.tsx` files to your project, which you can adjust as needed.

2. ### Use it in your application

   Use `MessagePrimitive.GroupedParts` with `groupPartByType` to map tool-call parts to `"group-tool"`. The group case wraps `children`; the tool-call leaf renders the resolved tool UI or your fallback.

   ```
   import { MessagePrimitive, groupPartByType } from "@assistant-ui/react";
   import { ToolFallback } from "@/components/assistant-ui/tool-fallback";
   import {
     ToolGroupContent,
     ToolGroupRoot,
     ToolGroupTrigger,
   } from "@/components/assistant-ui/tool-group";

   const AssistantMessage = () => {
     return (
       <MessagePrimitive.Root>
         <MessagePrimitive.GroupedParts
           groupBy={groupPartByType({
             "tool-call": ["group-tool"],
           })}
         >
           {({ part, children }) => {
             switch (part.type) {
               case "group-tool":
                 return (
                   <ToolGroupRoot>
                     <ToolGroupTrigger
                       count={part.indices.length}
                       active={part.status.type === "running"}
                     />
                     <ToolGroupContent>{children}</ToolGroupContent>
                   </ToolGroupRoot>
                 );
               case "tool-call":
                 return part.toolUI ?? <ToolFallback {...part} />;
               default:
                 return null;
             }
           }}
         </MessagePrimitive.GroupedParts>
       </MessagePrimitive.Root>
     );
   };
   ```

## Variants

Use the `variant` prop on `ToolGroup.Root` to change the visual style:

```
<ToolGroup.Root variant="outline">...</ToolGroup.Root>
<ToolGroup.Root variant="muted">...</ToolGroup.Root>
```

| Variant   | Description                  |
| --------- | ---------------------------- |
| `outline` | Rounded border (default)     |
| `ghost`   | No additional styling        |
| `muted`   | Muted background with border |

## Examples

### Streaming Demo (Custom UI + Fallback)

Interactive demo showing tool group with **custom tool UIs** and `ToolFallback` working together. Watch as weather cards stream in with loading states, followed by a search tool using the fallback UI.

\[interactive preview omitted]

### Custom Tool UIs

ToolGroup works with any custom tool UI components:

```
// Custom Weather Tool UI
function WeatherToolUI({ location, temperature, condition }) {
  return (
    <div className="flex items-center gap-3 rounded-lg border p-3">
      <WeatherIcon condition={condition} />
      <div>
        <div className="text-xs text-muted-foreground">{location}</div>
        <div className="text-lg font-medium">{temperature}°F</div>
      </div>
    </div>
  );
}

// Use in ToolGroup
<ToolGroupRoot variant="outline">
  <ToolGroupTrigger count={3} />
  <ToolGroupContent>
    <WeatherToolUI location="New York" temperature={65} condition="Cloudy" />
    <WeatherToolUI location="London" temperature={55} condition="Rainy" />
    <SearchToolUI query="best restaurants" results={24} />
  </ToolGroupContent>
</ToolGroupRoot>
```

## Composable API

All sub-components are exported for custom layouts:

| Component           | Description                                            |
| ------------------- | ------------------------------------------------------ |
| `ToolGroup.Root`    | Collapsible container with scroll lock and variants    |
| `ToolGroup.Trigger` | Header with tool count, shimmer animation, and chevron |
| `ToolGroup.Content` | Animated collapsible content wrapper                   |

```
import {
  ToolGroup,
  ToolGroupRoot,
  ToolGroupTrigger,
  ToolGroupContent,
} from "@/components/assistant-ui/tool-group";

// Compound component syntax
<ToolGroup.Root variant="outline" defaultOpen>
  <ToolGroup.Trigger count={3} active={false} />
  <ToolGroup.Content>
    {/* Any tool UI components - custom or ToolFallback */}
  </ToolGroup.Content>
</ToolGroup.Root>
```

## API Reference

### ToolGroupRoot

- `variant`: `"outline" | "ghost" | "muted"` (default `"outline"`) — Visual variant of the tool group container.
- `open?`: `boolean` — Controlled open state.
- `onOpenChange?`: `(open: boolean) => void` — Callback when open state changes.
- `defaultOpen`: `boolean` (default `false`) — Initial open state for uncontrolled usage.

### ToolGroupTrigger

- `count`: `number` — Number of tool calls to display in the label.
- `active`: `boolean` (default `false`) — Shows loading spinner and shimmer animation when true.

### ToolGroup (Legacy Wrapper)

`ToolGroup` is kept for existing code that still uses the deprecated `components.ToolGroup` prop on `MessagePrimitive.Parts`. Prefer composing `ToolGroupRoot`, `ToolGroupTrigger`, and `ToolGroupContent` inside `MessagePrimitive.GroupedParts`.

- `startIndex`: `number` — The index of the first tool call in the group.
- `endIndex`: `number` — The index of the last tool call in the group.
- `children`: `ReactNode` — The rendered tool call components.

## Related Components

- [ToolFallback](/docs/ui/tool-fallback) - Default UI for tools without custom renderers
- [PartGrouping](/docs/ui/part-grouping) - Advanced message part grouping (experimental)