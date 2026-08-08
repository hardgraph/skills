# AI SDK v4 (legacy)
URL: /docs/runtimes/ai-sdk/v4-legacy

Reference for projects still on AI SDK v4. New projects should use v7.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!warn]
>
> AI SDK v4 is a legacy version. New projects should use [AI SDK v7](/docs/runtimes/ai-sdk/v7). v4 integrations use the older `@assistant-ui/react-data-stream` package, not `@assistant-ui/react-ai-sdk`.

## Why legacy

Vercel ships AI SDK majors roughly yearly; v4 is three majors behind v7. The current `@assistant-ui/react-ai-sdk` package targets the current AI SDK major (v7) exclusively, so v4 users plug into [`@assistant-ui/react-data-stream`](/docs/runtimes/custom/data-stream) instead — the same data stream protocol that v4's `toDataStreamResponse()` emits.

This page is reference for existing v4 projects. Plan to migrate to v7 when feasible.

## Setup

1. ### Install

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-data-stream ai@^4
   ```

2. ### Backend route

   ```
   import { streamText } from "ai";
   import { openai } from "@ai-sdk/openai";

   export async function POST(req: Request) {
     const { messages } = await req.json();
     const result = streamText({ model: openai("gpt-5.6-luna"), messages });
     return result.toDataStreamResponse();
   }
   ```

3. ### Frontend

   ```
   "use client";

   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useDataStreamRuntime } from "@assistant-ui/react-data-stream";
   import { Thread } from "@/components/assistant-ui/thread";

   export default function Home() {
     const runtime = useDataStreamRuntime({ api: "/api/chat" });
     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <div className="h-full">
           <Thread />
         </div>
       </AssistantRuntimeProvider>
     );
   }
   ```

   AI SDK v4's `toDataStreamResponse()` emits the data stream wire format with the `x-vercel-ai-data-stream: v1` response header, so `useDataStreamRuntime` detects it automatically. If a custom proxy strips or hides that header, pass `protocol: "data-stream"` explicitly.

`useDataStreamRuntime` accepts `api`, `initialMessages`, `onFinish`, `onError`, `headers`, `body`, and the standard `LocalRuntimeOptions`. For the full reference, see [Data Stream Protocol](/docs/runtimes/custom/data-stream).

## Differences from v6

| Feature         | v4                                | v6                                        |
| --------------- | --------------------------------- | ----------------------------------------- |
| `ai` package    | `ai@^4`                           | `ai@^6`                                   |
| Runtime package | `@assistant-ui/react-data-stream` | `@assistant-ui/react-ai-sdk`              |
| Runtime hook    | `useDataStreamRuntime`            | `useChatRuntime`                          |
| Response        | `toDataStreamResponse()`          | `toUIMessageStreamResponse()`             |
| Tool schema     | `parameters: z.object({...})`     | `inputSchema: zodSchema(z.object({...}))` |
| Message type    | (untyped)                         | `UIMessage`                               |

## Migration to v7

When you are ready to upgrade:

1. Swap `@assistant-ui/react-data-stream` for `@assistant-ui/react-ai-sdk`.
2. Update your backend to the current AI SDK's `streamText` (see [v7 docs](/docs/runtimes/ai-sdk/v7)).
3. Switch the runtime hook from `useDataStreamRuntime` to `useChatRuntime`.

AI SDK provides codemods at [ai-sdk.dev/docs/migration-guides](https://ai-sdk.dev/docs/migration-guides) that handle the package-side rewrites.

## Alternative: react-ai-sdk\@0.10.16

An older release line of `@assistant-ui/react-ai-sdk` supported AI SDK v4 directly; its last release was `0.10.16`. It is no longer maintained and has no upgrade path; if you are starting a new v4 project, use `@assistant-ui/react-data-stream` instead.

## Related

- [AI SDK v7](/docs/runtimes/ai-sdk/v7) — Current integration; what new projects should target.
- [Data Stream Protocol](/docs/runtimes/custom/data-stream) — The runtime v4 integrations are built on.
- [AI SDK v5 (legacy)](/docs/runtimes/ai-sdk/v5-legacy) — The intermediate legacy version.