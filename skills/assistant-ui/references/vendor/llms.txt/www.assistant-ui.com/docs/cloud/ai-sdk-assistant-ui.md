# AI SDK + assistant-ui
URL: /docs/cloud/ai-sdk-assistant-ui

Integrate cloud persistence using assistant-ui runtime and pre-built components.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Overview

This guide shows how to integrate Assistant Cloud with the [AI SDK](https://sdk.vercel.ai/) using assistant-ui's runtime system and pre-built UI components.

## What You Get

This integration provides:

- **`<Thread />`** — A complete chat interface with messages, composer, and status indicators
- **`<ThreadList />`** — A sidebar showing all conversations with auto-generated titles, plus new/delete/manage actions
- **Automatic Persistence** — Messages save as they stream. Threads are created automatically on first message.
- **Runtime Integration** — The assistant-ui runtime handles all cloud synchronization behind the scenes.

## How It Works

The `useChatRuntime` hook from `@assistant-ui/react-ai-sdk` wraps AI SDK's `useChat` and adds cloud persistence via the `cloud` parameter. The runtime automatically:

1. Creates a cloud thread on the first user message
2. Persists messages as they complete streaming
3. Generates a conversation title after the assistant's first response
4. Loads historical messages when switching threads via `<ThreadList />`

You provide the cloud configuration—everything else is handled. The default `AssistantChatTransport` automatically sends requests to `/api/chat`.

## Prerequisites

> [!info]
>
> You need an assistant-cloud account to follow this guide. [Sign up here](https://cloud.assistant-ui.com/) to get started.

## Setup Guide

1. ### Create a Cloud Project

   Create a new project in the [assistant-cloud dashboard](https://cloud.assistant-ui.com/) and from the settings page, copy:

   - **Frontend API URL**: `https://proj-[ID].assistant-api.com`
   - **Assistant Cloud API Key**: `sk_aui_proj_*`

2. ### Configure Environment Variables

   Add the following environment variables to your project:

   **React**

   ```
   # Frontend API URL from your cloud project settings
   NEXT_PUBLIC_ASSISTANT_BASE_URL=https://proj-[YOUR-ID].assistant-api.com

   # API key for server-side operations
   ASSISTANT_API_KEY=your-api-key-here
   ```

3. ### Install Dependencies

   Install the required packages:

   **React**

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-ai-sdk
   ```

4. ### Set Up the Cloud Runtime

   Create a client-side AssistantCloud instance and integrate it with your AI SDK runtime:

   **React**

   ```
   "use client";

   import { useMemo } from "react";
   import { AssistantCloud, AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useChatRuntime } from "@assistant-ui/react-ai-sdk";
   import { ThreadList } from "@/components/assistant-ui/thread-list";
   import { Thread } from "@/components/assistant-ui/thread";

   export default function ChatPage() {
     const cloud = useMemo(
       () =>
         new AssistantCloud({
           baseUrl: process.env.NEXT_PUBLIC_ASSISTANT_BASE_URL!,
           anonymous: true, // Creates browser-session based user ID
         }),
       [],
     );

     const runtime = useChatRuntime({
       cloud,
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <div className="grid h-dvh grid-cols-[200px_1fr] gap-x-2 px-4 py-4">
           <ThreadList />
           <Thread />
         </div>
       </AssistantRuntimeProvider>
     );
   }
   ```

## `useChatRuntime` Options

- `cloud?`: `AssistantCloud` — Optional AssistantCloud instance for chat persistence and thread management.

- `adapters?`: `RuntimeAdapters` — Optional runtime adapters to extend or override built-in functionality.

  - `attachments?`: `AttachmentAdapter` — Custom attachment adapter for file uploads. Defaults to the Vercel AI SDK attachment adapter.
  - `speech?`: `SpeechSynthesisAdapter` — Adapter for text-to-speech functionality.
  - `dictation?`: `DictationAdapter` — Adapter for speech-to-text dictation input.
  - `feedback?`: `FeedbackAdapter` — Adapter for collecting user feedback on messages.
  - `history?`: `ThreadHistoryAdapter` — Adapter for loading and saving thread history. Used to restore previous messages when switching threads. The adapter must implement \`withFormat\` when used with AI SDK — see [Persisting Chat History](/docs/runtimes/ai-sdk/v7#persisting-chat-history).

- `toCreateMessage?`: `(message: AppendMessage) => CreateUIMessage` — Optional custom function to convert an assistant-ui AppendMessage into an AI SDK CreateUIMessage before sending. Use this to customize how outgoing messages are formatted, for example to add custom metadata or transform content parts.

- `transport?`: `ChatTransport` — Custom transport implementation. Defaults to AssistantChatTransport which sends requests to '/api/chat'.

- `onResumeError?`: `(error: unknown) => void` — Optional callback when useChatRuntime finds a pending resumable stream id but the automatic reconnect fails. Use it for a toast, telemetry, or retry UI; the stale stream id is still cleared.

## Telemetry

The `useChatRuntime` hook captures full run telemetry including timing data. This integrates with the assistant-ui runtime to provide:

**Automatically captured:**

- `status` — `"completed"`, `"incomplete"`, or `"error"`
- `duration_ms` — Total run duration (measured client-side)
- `steps` — Per-step breakdowns with timing, usage, and tool calls
- `tool_calls` — Tool invocations with name, arguments, results, and source
- `total_steps` — Number of reasoning/tool steps
- `output_text` — Full response text (truncated at 50K characters)

**Requires route configuration:**

- `model_id` — The model used
- `input_tokens` / `output_tokens` — Token usage statistics

To capture model and usage data, add the `messageMetadata` callback to your AI SDK route:

```
import { streamText } from "ai";
import { openai } from "@ai-sdk/openai";

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai("gpt-5.6-luna"),
    messages,
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

Without this configuration, model and token data will be omitted from telemetry reports.

### Customizing Reports

Use the `beforeReport` hook to add custom metadata or filter reports:

```
const cloud = new AssistantCloud({
  baseUrl: process.env.NEXT_PUBLIC_ASSISTANT_BASE_URL!,
  anonymous: true,
  telemetry: {
    beforeReport: (report) => ({
      ...report,
      metadata: { userTier: "pro", region: "us-east" },
    }),
  },
});
```

Return `null` from `beforeReport` to skip reporting a specific run. To disable telemetry entirely, pass `telemetry: false`.

### Sub-Agent Model Tracking

When tool calls delegate to a different model (e.g., the main run uses GPT but a tool invokes Gemini), you can track the delegated model's usage. Pass sampling call data through `messageMetadata.samplingCalls` in your API route, and the telemetry reporter will automatically include it in the report.

See the [AI SDK Telemetry guide](/docs/cloud/ai-sdk#sub-agent-model-tracking) for the full setup with `createSamplingCollector` and `wrapSamplingHandler`.

## Authentication

The React example above uses anonymous mode for quick demos. For production apps with user accounts, pass an explicit cloud instance with a token from your auth provider:

**React**

```
import { useMemo } from "react";
import { useAuth } from "@clerk/nextjs";
import { AssistantCloud } from "@assistant-ui/react";
import { useChatRuntime } from "@assistant-ui/react-ai-sdk";

function Chat() {
  const { getToken } = useAuth();

  const cloud = useMemo(() => new AssistantCloud({
    baseUrl: process.env.NEXT_PUBLIC_ASSISTANT_BASE_URL!,
    authToken: async () => getToken({ template: "assistant-ui" }),
  }), [getToken]);

  const runtime = useChatRuntime({ cloud });
  // ...
}
```

See the [Cloud Authorization](/docs/cloud/authorization) guide for other auth providers.

## Complete Example

Check out the [with-cloud example](https://github.com/assistant-ui/assistant-ui/tree/main/examples/with-cloud) on GitHub for a fully working implementation.