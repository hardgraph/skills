# Message Timing
URL: /docs/ui/message-timing

Display streaming performance stats — TTFT, total time, tok/s, and chunk count — as a badge with hover popover.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

> [!warn]
>
> This component is experimental. The API and displayed metrics may change in future versions. When used with the Vercel AI SDK, token counts and tok/s are **estimated** client-side and may be inaccurate — see [Accuracy](#accuracy) below.

## Getting Started

1. ### Add `message-timing`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/message-timing
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/message-timing.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react
   ```

   ```bash
   npx shadcn@latest add tooltip
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/message-timing.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/message-timing.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/message-timing.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/message-timing.tsx
   ```

   This adds a `/components/assistant-ui/message-timing.tsx` file to your project.

2. ### Use in your application

   Place `MessageTiming` inside `ActionBarPrimitive.Root` in your `thread.tsx`. It will inherit the action bar's auto-hide behaviour and only renders after the stream completes.

   ```
   import { ActionBarPrimitive } from "@assistant-ui/react";
   import { MessageTiming } from "@/components/assistant-ui/message-timing";

   const AssistantActionBar: FC = () => {
     return (
       <ActionBarPrimitive.Root
         hideWhenRunning
         autohide="not-last"
       >
         <ActionBarPrimitive.Copy />
         <ActionBarPrimitive.Reload />
         <MessageTiming />
       </ActionBarPrimitive.Root>
     );
   };
   ```

## What It Shows

The badge displays `totalStreamTime` inline and reveals a popover on hover with the full breakdown:

| Metric          | Description                                               |
| --------------- | --------------------------------------------------------- |
| **First token** | Time from request start to first text chunk (TTFT)        |
| **Total**       | Total wall-clock time from start to stream end            |
| **Speed**       | Output tokens per second (hidden for very short messages) |
| **Chunks**      | Number of stream chunks received                          |

## Accuracy

Timing accuracy depends on how your backend is connected.

### Data Stream (accurate)

When using the [Data Stream](/docs/runtimes/custom/data-stream) protocol on the backend (via `assistant-stream`), token counts come directly from the model's usage data sent in `step-finish` chunks. The `tokensPerSecond` metric is exact whenever your backend reports `outputTokens`.

### Vercel AI SDK (estimated)

When using the AI SDK integration (`useChatRuntime`), token counts are **estimated** client-side using a 4 characters per token approximation. This can overcount significantly for short messages.

## API Reference

### `MessageTiming` component

| Prop        | Type                                     | Default   | Description                                |
| ----------- | ---------------------------------------- | --------- | ------------------------------------------ |
| `className` | `string`                                 | —         | Additional class names on the root element |
| `side`      | `"top" \| "right" \| "bottom" \| "left"` | `"right"` | Side of the tooltip relative to the badge  |

Renders `null` until `totalStreamTime` is available (i.e., while streaming or for user messages).

For the underlying `useMessageTiming()` hook, field definitions, and runtime-specific setup (LocalRuntime, ExternalStore, etc.), see the [Message Timing guide](/docs/guides/message-timing).

## Related

- [Message Timing guide](/docs/guides/message-timing) — `useMessageTiming()` hook, runtime support table, and custom timing UI
- [Thread](/docs/ui/thread) — The action bar context that `MessageTiming` is typically placed inside