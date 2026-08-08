# AI SDK v6 (legacy)
URL: /docs/runtimes/ai-sdk/v6-legacy

Reference for projects still on AI SDK v6. New projects should use v7.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!warn]
>
> AI SDK v6 is a legacy version. New projects should use [AI SDK v7](/docs/runtimes/ai-sdk/v7). v6 users pin `@assistant-ui/react-ai-sdk@1.3.40` (the last v6-compatible release); later releases target v7.

Requires `ai@^6` and `@ai-sdk/react@^3`. For other versions see the [overview](/docs/runtimes/ai-sdk/overview).

## Quickstart

1. ### Create a project

   **React**

   ```
   npx create-next-app@latest my-app
   cd my-app
   ```

2. ### Install dependencies

   **React**

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-ai-sdk@1.3.40 ai@^6 @ai-sdk/react@^3 @ai-sdk/openai zod
   ```

3. ### Set up the backend route

   For React Native and Ink, host this route in a separate backend project and point the client runtime at its absolute URL.

   ```
   import { openai } from "@ai-sdk/openai";
   import {
     streamText,
     convertToModelMessages,
     tool,
     zodSchema,
   } from "ai";
   import type { UIMessage } from "ai";
   import { z } from "zod";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages }: { messages: UIMessage[] } = await req.json();

     const result = streamText({
       model: openai("gpt-5.6-luna"),
       messages: await convertToModelMessages(messages), // async in v6
       tools: {
         get_current_weather: tool({
           description: "Get the current weather",
           inputSchema: zodSchema(z.object({ city: z.string() })),
           execute: async ({ city }) => {
             return `The weather in ${city} is sunny`;
           },
         }),
       },
     });

     return result.toUIMessageStreamResponse();
   }
   ```

4. ### Set up the frontend

   **React**

   ```
   "use client";

   import { Thread } from "@/components/assistant-ui/thread";
   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useChatRuntime } from "@assistant-ui/react-ai-sdk";

   export default function Home() {
     const runtime = useChatRuntime();
     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <div className="h-full">
           <Thread />
         </div>
       </AssistantRuntimeProvider>
     );
   }
   ```

5. ### Set up UI components

   **React**

   Follow the [UI Components guide](/docs/ui/thread) to wire up the Thread, composer, and supporting primitives.

## Frontend tools and system messages

`AssistantChatTransport` (used by default) forwards system messages and frontend tools to your backend on every request. Consume them with `frontendTools`:

```
import { openai } from "@ai-sdk/openai";
import { streamText, convertToModelMessages, zodSchema } from "ai";
import type { UIMessage } from "ai";
import { frontendTools } from "@assistant-ui/react-ai-sdk";

export async function POST(req: Request) {
  const {
    messages,
    system,
    tools,
  }: {
    messages: UIMessage[];
    system?: string;
    tools?: any;
  } = await req.json();

  const result = streamText({
    model: openai("gpt-5.6-luna"),
    system,
    messages: await convertToModelMessages(messages),
    tools: {
      ...frontendTools(tools),
      // your backend tools...
    },
  });

  return result.toUIMessageStreamResponse();
}
```

Frontend tools are registered through the provider's `config` (see the [tools guide](/docs/tools/defining-tools)) and serialized for the backend via `frontendTools`.

## Multi-step tool calls

AI SDK v6 supports running multiple tool-call rounds in a single request via `stopWhen`. Use `stepCountIs(N)` to cap iterations:

```
import { openai } from "@ai-sdk/openai";
import {
  streamText,
  convertToModelMessages,
  stepCountIs,
} from "ai";

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai("gpt-5.6-luna"),
    messages: await convertToModelMessages(messages),
    tools: {
      /* ... */
    },
    stopWhen: stepCountIs(10), // cap at 10 tool-call iterations
  });

  return result.toUIMessageStreamResponse();
}
```

Without a `stopWhen`, AI SDK runs a single inference step. Set `stepCountIs` (or one of AI SDK's other stop conditions) when your tools chain multiple calls.

## Server-side tool approval

AI SDK v6 lets a tool gate its own execution with `needsApproval`. The server pauses, emits an `approval-requested` part, and resumes once the client posts an approval response. assistant-ui surfaces the gate as `approval` on the tool part and adds a `respondToApproval` prop on the renderer, so tool components stay decoupled from `chatHelpers`.

On the backend, mark the tool with `needsApproval` (boolean or function):

```
import { openai } from "@ai-sdk/openai";
import { streamText, convertToModelMessages, tool } from "ai";
import { z } from "zod";

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai("gpt-5.6-luna"),
    messages: await convertToModelMessages(messages),
    tools: {
      deploy: tool({
        description: "Deploy the current build to an environment.",
        inputSchema: z.object({ target: z.string() }),
        needsApproval: ({ input }) => input.target === "production",
        execute: async ({ target }) => ({ deployed: target }),
      }),
    },
  });

  return result.toUIMessageStreamResponse();
}
```

On the client, configure `useChat` with `sendAutomaticallyWhen: lastAssistantMessageIsCompleteWithApprovalResponses` so the response is sent back to the server when the user decides:

```
"use client";

import { useChat } from "@ai-sdk/react";
import {
  AssistantRuntimeProvider,
  useAISDKRuntime,
} from "@assistant-ui/react-ai-sdk";
import { lastAssistantMessageIsCompleteWithApprovalResponses } from "ai";
import { Thread } from "@/components/assistant-ui/thread";

export default function Page() {
  const chat = useChat({
    sendAutomaticallyWhen: lastAssistantMessageIsCompleteWithApprovalResponses,
  });
  const runtime = useAISDKRuntime(chat);

  return (
    <AssistantRuntimeProvider runtime={runtime}>
      <Thread />
    </AssistantRuntimeProvider>
  );
}
```

Render the gate with a toolkit entry. The `approval` field carries the gate state, and `respondToApproval` is the only correct way to acknowledge it (it reads the approval id from the part):

```
"use client";

import { useChat } from "@ai-sdk/react";
import {
  AssistantRuntimeProvider,
  AuiConfig,
  defineToolkit,
  Tools,
} from "@assistant-ui/react";
import { useAISDKRuntime } from "@assistant-ui/react-ai-sdk";
import { lastAssistantMessageIsCompleteWithApprovalResponses } from "ai";
import { Thread } from "@/components/assistant-ui/thread";

const toolkit = defineToolkit({
  deploy: {
    type: "backend",
    render: ({ args, approval, respondToApproval, result }) => {
      if (approval?.approved === undefined) {
        return (
          <div>
            <p>Approve deploy to {args.target}?</p>
            <button onClick={() => respondToApproval({ approved: true })}>
              Approve
            </button>
            <button
              onClick={() =>
                respondToApproval({ approved: false, reason: "user denied" })
              }
            >
              Deny
            </button>
          </div>
        );
      }
      if (approval?.approved === false) return <p>Denied</p>;
      if (result === undefined) return <p>Approved, deploying…</p>;
      return <p>Deployed {result.deployed}</p>;
    },
  },
});

export default function Page() {
  const chat = useChat({
    sendAutomaticallyWhen: lastAssistantMessageIsCompleteWithApprovalResponses,
  });
  const runtime = useAISDKRuntime(chat);
  const config = AuiConfig({ tools: Tools({ toolkit }) });
  return (
    <AssistantRuntimeProvider runtime={runtime} config={config}>
      <Thread />
    </AssistantRuntimeProvider>
  );
}
```

See the [tool UI guide](/docs/tools/tool-ui#server-side-approval-gates) for the full three-state semantics of `approval.approved` and the `isAutomatic` flag.

## Quote context

assistant-ui's composer can attach quote metadata to user messages (e.g. when the user selects text and clicks "Quote"). On the server, `injectQuoteContext` flattens that metadata into a markdown blockquote prefix so the LLM sees the quoted text:

```
import {
  streamText,
  convertToModelMessages,
} from "ai";
import { injectQuoteContext } from "@assistant-ui/react-ai-sdk";
import { openai } from "@ai-sdk/openai";

export async function POST(req: Request) {
  const { messages } = await req.json();
  const result = streamText({
    model: openai("gpt-5.6-luna"),
    messages: await convertToModelMessages(injectQuoteContext(messages)),
  });
  return result.toUIMessageStreamResponse();
}
```

`injectQuoteContext` is idempotent: if the blockquote prefix is already present, it is not duplicated.

## Token usage

`useThreadTokenUsage` exposes thread-level token usage on the client. Attach `usage` and `modelId` via `messageMetadata` on the server:

```
import { streamText, convertToModelMessages } from "ai";
import { frontendTools } from "@assistant-ui/react-ai-sdk";

export async function POST(req: Request) {
  const { messages, tools, config } = await req.json();
  const result = streamText({
    model: getModel(config?.modelName),
    messages: await convertToModelMessages(messages),
    tools: frontendTools(tools),
  });
  return result.toUIMessageStreamResponse({
    messageMetadata: ({ part }) => {
      if (part.type === "finish") {
        return { usage: part.totalUsage };
      }
      if (part.type === "finish-step") {
        return { modelId: part.response.modelId };
      }
      return undefined;
    },
  });
}
```

```
"use client";

import { useThreadTokenUsage } from "@assistant-ui/react-ai-sdk";

export function TokenCounter() {
  const usage = useThreadTokenUsage();
  if (!usage) return null;
  return <div>{usage.totalTokens} total tokens</div>;
}
```

`getThreadMessageTokenUsage(message)` returns per-message usage if you need to show it inline.

## Attachments

Attachments work through the standard [attachment adapter](/docs/runtimes/concepts/adapters#attachment-adapter). Pass it via `useChatRuntime`:

```
const runtime = useChatRuntime({
  adapters: { attachments: myAttachmentAdapter },
});
```

Your attachment adapter's `send` should return `content` parts in the AI SDK shape, e.g. for files:

```
return {
  ...attachment,
  status: { type: "complete" },
  content: [
    {
      type: "file",
      mimeType: attachment.contentType ?? "",
      filename: attachment.name,
      data: await getFileDataURL(attachment.file),
    },
  ],
};
```

For images, use `{ type: "image", image: <data url or remote url> }`. AI SDK will pass these to providers that accept multimodal content.

## Persisting chat history

By default, messages live only in memory and reset on reload. To persist and restore history per thread, provide a `ThreadHistoryAdapter` via `adapters.history`.

> [!warn]
>
> The adapter **must** implement `withFormat`. `useChatRuntime` persists history through `withFormat(fmt)` so messages round-trip as AI SDK `UIMessage` objects. An adapter without `withFormat` throws at runtime; `load` and `append` on the top level are unused in the AI SDK path.

> [!info]
>
> For server-side cloud persistence with zero adapter code, see the [AssistantCloud integration](/docs/cloud/ai-sdk-assistant-ui).

```
"use client";

import { useChatRuntime } from "@assistant-ui/react-ai-sdk";
import type { ThreadHistoryAdapter } from "@assistant-ui/react";

const historyAdapter: ThreadHistoryAdapter = {
  // Required by the type — unused by useChatRuntime.
  async load() {
    return { headId: null, messages: [] };
  },
  async append() {},

  // fmt encodes UIMessage <-> storage rows (ai-sdk/v6 format).
  withFormat: (fmt) => ({
    async load() {
      const rows = await fetch("/api/history").then((r) => r.json());
      return { messages: rows.map(fmt.decode) };
    },
    async append(item) {
      await fetch("/api/history", {
        method: "POST",
        body: JSON.stringify({
          id: fmt.getId(item.message),
          parent_id: item.parentId,
          format: fmt.format,
          content: fmt.encode(item),
        }),
      });
    },
  }),
};

function Chat() {
  const runtime = useChatRuntime({ adapters: { history: historyAdapter } });
  // ...
}
```

Each persisted row follows `{ id, parent_id, format, content }`. `fmt.encode` produces the `content` payload and `fmt.decode` reverses it, so your backend never needs to know about `UIMessage` internals.

## Custom transport

`AssistantChatTransport` is the default. To point at a non-default endpoint, instantiate it with a custom `api`:

```
import {
  useChatRuntime,
  AssistantChatTransport,
} from "@assistant-ui/react-ai-sdk";

const runtime = useChatRuntime({
  transport: new AssistantChatTransport({ api: "/my-custom-api/chat" }),
});
```

If you need a transport that does not inherit from `AssistantChatTransport`, you forfeit automatic system-message and frontend-tool forwarding; the transport is then a regular AI SDK `ChatTransport` and you control everything explicitly.

## Advanced: useAISDKRuntime

When you need direct access to the `useChat` instance (e.g. to share it with non-assistant-ui code), drop down to `useAISDKRuntime`:

```
import { useChat } from "@ai-sdk/react";
import { useAISDKRuntime } from "@assistant-ui/react-ai-sdk";

const chat = useChat();
const runtime = useAISDKRuntime(chat);
```

`useAISDKRuntime` does not provide `cloud` or the higher-level adapter slots; those are part of `useChatRuntime`. If you need both your own `useChat` access AND cloud thread support, you generally want `useChatRuntime` and to read the chat state through assistant-ui hooks instead.

## Adapter support

| Adapter     | Supported via                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------- |
| Attachments | `adapters.attachments`                                                                          |
| Speech      | `adapters.speech`                                                                               |
| Dictation   | `adapters.dictation`                                                                            |
| Feedback    | `adapters.feedback`                                                                             |
| History     | `adapters.history` (must implement `withFormat`)                                                |
| threadList  | Via `cloud` (managed) or [RemoteThreadListRuntime](/docs/runtimes/concepts/threads) (custom DB) |

## Key changes from v5

| Feature                  | v5                            | v6                                        |
| ------------------------ | ----------------------------- | ----------------------------------------- |
| `ai` package             | `ai@^5`                       | `ai@^6`                                   |
| `@ai-sdk/react`          | `@ai-sdk/react@^2`            | `@ai-sdk/react@^3`                        |
| `convertToModelMessages` | Sync                          | Async (`await`)                           |
| Tool schema              | `parameters: z.object({...})` | `inputSchema: zodSchema(z.object({...}))` |
| Response                 | `toDataStreamResponse()`      | `toUIMessageStreamResponse()`             |

## Example

The maintained example targets v7: [`examples/with-ai-sdk-v7`](https://github.com/assistant-ui/assistant-ui/tree/main/examples/with-ai-sdk-v7). The last v6 revision of that example lives in the repository history alongside the `@assistant-ui/react-ai-sdk` 1.x releases.

For responses that should survive a page reload mid-stream, see [Resumable streams](/docs/guides/resumable-streams), which wraps the same v6 byte stream in `assistant-stream/resumable`.

## Next

- [Adapters](/docs/runtimes/concepts/adapters) — Attachments, speech, feedback, history, suggestions; one source of truth.
- [Threads](/docs/runtimes/concepts/threads) — Single-thread, AssistantCloud, custom thread list.
- [Resumable streams](/docs/guides/resumable-streams) — Survive page reloads and connection drops mid-response.