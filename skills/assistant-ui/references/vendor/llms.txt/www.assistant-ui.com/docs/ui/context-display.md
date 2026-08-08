# Context Display
URL: /docs/ui/context-display

Visualize token usage relative to a model's context window — ring, bar, or text — with a detailed hover popover.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

> [!info]
>
> This component requires server-side setup to [forward token usage metadata](#forward-token-usage-from-your-route-handler). Without it, ContextDisplay will show 0 usage and no breakdown data.

## Getting Started

1. ### Add `context-display`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/context-display
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/context-display.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-ai-sdk
   ```

   ```bash
   npx shadcn@latest add tooltip
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/context-display.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/context-display.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/context-display.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/context-display.tsx
   ```

   This adds a `/components/assistant-ui/context-display.tsx` file to your project.

2. ### Forward token usage from your route handler

   Use `messageMetadata` in your Next.js route to attach `usage` from `finish` and `modelId` from `finish-step`:

   ```
   import { streamText, convertToModelMessages } from "ai";

   export async function POST(req: Request) {
     const { messages, config } = await req.json();
     const result = streamText({
       model: getModel(config?.modelName),
       messages: await convertToModelMessages(messages),
     });
     return result.toUIMessageStreamResponse({
       messageMetadata: ({ part }) => {
         if (part.type === "finish") {
           return {
             usage: part.totalUsage,
           };
         }
         if (part.type === "finish-step") {
           return {
             modelId: part.response.modelId,
           };
         }
         return undefined;
       },
     });
   }
   ```

3. ### Use in your application

   Pick a variant and place it in your thread footer, composer, or sidebar. Pass `modelContextWindow` with your model's token limit.

   ```
   import { ContextDisplay } from "@/components/assistant-ui/context-display";

   const ThreadFooter: FC = () => {
     return (
       <div className="flex items-center justify-end px-3 py-1.5">
         <ContextDisplay.Bar modelContextWindow={128000} />
       </div>
     );
   };
   ```

## Variants

Three preset variants are available, each wrapping the shared tooltip popover:

```
// SVG donut ring (default, compact)
<ContextDisplay.Ring modelContextWindow={128000} />

// Horizontal progress bar with label
<ContextDisplay.Bar modelContextWindow={128000} />

// Minimal monospace text
<ContextDisplay.Text modelContextWindow={128000} />
```

All presets accept `className` for styling overrides and `side` to control tooltip placement (`"top"`, `"bottom"`, `"left"`, `"right"`).

## Composable API

For custom visualizations, use the building blocks directly:

```
import { ContextDisplay } from "@/components/assistant-ui/context-display";

<ContextDisplay.Root modelContextWindow={128000}>
  <ContextDisplay.Trigger aria-label="Context usage">
    <MyCustomGauge />
  </ContextDisplay.Trigger>
  <ContextDisplay.Content side="top" />
</ContextDisplay.Root>
```

| Component | Description                                                                                                                            |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `Root`    | Uses provided `usage` when supplied, otherwise fetches token usage internally; provides shared context and wraps children in a tooltip |
| `Trigger` | Button that opens the tooltip on hover                                                                                                 |
| `Content` | Tooltip popover with the token breakdown (Usage %, Input, Cached, Output, Reasoning, Total)                                            |

## API Reference

### Preset Props

All preset variants (`Ring`, `Bar`, `Text`) share the same props:

| Prop                 | Type                                     | Default | Description                                                                        |
| -------------------- | ---------------------------------------- | ------- | ---------------------------------------------------------------------------------- |
| `modelContextWindow` | `number`                                 | —       | Maximum token limit of the current model (required)                                |
| `className`          | `string`                                 | —       | Additional class names on the trigger button                                       |
| `side`               | `"top" \| "bottom" \| "left" \| "right"` | `"top"` | Tooltip placement                                                                  |
| `usage`              | `ThreadTokenUsage`                       | —       | Optional externally-provided usage data (skips internal usage fetch when provided) |

### Color Thresholds

Ring and Bar share the same severity colors:

| Level    | Threshold   | Ring                 | Bar              |
| -------- | ----------- | -------------------- | ---------------- |
| Low      | `< 65%`     | `stroke-emerald-500` | `bg-emerald-500` |
| Warning  | `65% – 85%` | `stroke-amber-500`   | `bg-amber-500`   |
| Critical | `> 85%`     | `stroke-red-500`     | `bg-red-500`     |

Text displays numeric values only — no severity color.

## Related

- [Message Timing](/docs/ui/message-timing) — Streaming performance stats (TTFT, tok/s)
- [Thread](/docs/ui/thread) — The thread component where ContextDisplay is typically placed