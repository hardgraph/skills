# Data Stream Protocol
URL: /docs/runtimes/custom/data-stream

Standard message-streaming protocol on top of LocalRuntime.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`@assistant-ui/react-data-stream` consumes the data stream protocol, a standardized format for streaming AI responses. It is layered on `LocalRuntime` (see [architecture](/docs/runtimes/concepts/architecture)), so all `LocalRuntime` features apply.

The protocol supports streaming text, tool calls, conversation context, error handling, cancellation, and attachments.

## When to use it

Pick this runtime when:

- Your backend already speaks the data stream protocol (or you can make it do so).
- You want a thin message-stream contract without writing a `ChatModelAdapter`.
- You are migrating from AI SDK v4 and want the v4 pattern preserved (see [v4 docs](/docs/runtimes/ai-sdk/v4-legacy)).

If your backend exposes a richer state surface, consider [`AssistantTransport`](/docs/runtimes/custom/assistant-transport) instead.

## Wire protocols

`useDataStreamRuntime` accepts two wire formats. When `protocol` is omitted, it detects known Vercel AI response markers before falling back to UI message stream for compatibility.

| Protocol              | Detection                                               | Matching backend                                                                                                         |
| --------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `"ui-message-stream"` | `x-vercel-ai-ui-message-stream: v1`, otherwise fallback | AI SDK v5+'s `result.toUIMessageStreamResponse()` (SSE)                                                                  |
| `"data-stream"`       | `x-vercel-ai-data-stream: v1`                           | `createAssistantStreamResponse` from `assistant-stream`, or AI SDK v4's `toDataStreamResponse()` (Vercel data stream v1) |

Set `protocol` explicitly only for custom endpoints that do not preserve or expose the response marker.

## Install

**React**

```bash
npm install @assistant-ui/react @assistant-ui/react-data-stream
```

## Quickstart

1. ### Set up the runtime

   **React**

   ```
   "use client";

   import { useDataStreamRuntime } from "@assistant-ui/react-data-stream";
   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { Thread } from "@/components/assistant-ui/thread";

   export default function ChatPage() {
     const runtime = useDataStreamRuntime({ api: "/api/chat" });
     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <Thread />
       </AssistantRuntimeProvider>
     );
   }
   ```

2. ### Create the backend endpoint

   Your backend should accept POST requests and return data stream responses: For React Native and Ink, host this endpoint in a separate backend project and point the client runtime at its absolute URL.

   ```
   import { createAssistantStreamResponse } from "assistant-stream";

   export async function POST(request: Request) {
     const { messages, tools, system, threadId } = await request.json();

     return createAssistantStreamResponse(async (controller) => {
       const stream = await processWithAI({ messages, tools, system });
       for await (const chunk of stream) {
         controller.appendText(chunk.text);
       }
     });
   }
   ```

   The request body includes `messages`, `tools`, `system` (if configured), and `threadId`.

   `createAssistantStreamResponse` returns a standard Web `Response`. It drops into any Fetch-style route (Next.js App Router, Hono, Bun.serve, Deno, Cloudflare Workers) without ceremony. On frameworks that use their own response object (Express, Fastify), copy `status` and `headers` over, then pipe `response.body` into the framework's writable stream. Avoid sending the `Response` body into a `res` object whose lifecycle is already controlled by the framework (that is the `ERR_INVALID_STATE: Controller is already closed` failure mode).

## Headers and authentication

```
const runtime = useDataStreamRuntime({
  api: "/api/chat",
  headers: { Authorization: `Bearer ${token}`, "X-Custom-Header": "value" },
  credentials: "include",
});
```

Evaluate per-request:

```
const runtime = useDataStreamRuntime({
  api: "/api/chat",
  headers: async () => ({
    Authorization: `Bearer ${await getAuthToken()}`,
  }),
  body: async () => ({
    requestId: crypto.randomUUID(),
    timestamp: Date.now(),
    signature: await computeSignature(),
  }),
});
```

## Event callbacks

```
const runtime = useDataStreamRuntime({
  api: "/api/chat",
  onResponse: (response) => console.log("status:", response.status),
  onFinish: (message) => console.log("done:", message),
  onError: (error) => console.error(error),
  onCancel: () => console.log("cancelled"),
});
```

## Tool integration

> [!warn]
>
> Human-in-the-loop tools (`unstable_humanToolNames`, `human()` interrupts) are not supported in the data stream runtime. Use [`LocalRuntime`](/docs/runtimes/custom/local-runtime#human-in-the-loop-tools) directly if you need them.

### Frontend tools

Serialize client-side tools with `toToolsJSONSchema`:

```
import { tool } from "@assistant-ui/react";
import { toToolsJSONSchema } from "assistant-stream";

const myTools = {
  get_weather: tool({
    description: "Get current weather",
    parameters: z.object({ location: z.string() }),
    execute: async ({ location }) => {
      const weather = await fetchWeather(location);
      return `Weather in ${location}: ${weather}`;
    },
  }),
};

const runtime = useDataStreamRuntime({
  api: "/api/chat",
  body: { tools: toToolsJSONSchema(myTools) },
});
```

### Backend tool processing

```
const { tools } = await request.json();
const response = await ai.generateText({ messages, tools });
```

Tool results stream back automatically.

## Message conversion

### Generic (recommended)

```
import { toGenericMessages, toToolsJSONSchema } from "assistant-stream";

const genericMessages = toGenericMessages(messages);
const toolSchemas = toToolsJSONSchema(tools);
```

`GenericMessage` is a union of `system`, `user` (with text and file parts), `assistant` (with text and tool-call parts), and `tool` (with tool-result parts). It is easy to convert to any LLM provider format.

### AI SDK specific

```
import { toLanguageModelMessages } from "@assistant-ui/react-data-stream";

const languageModelMessages = toLanguageModelMessages(messages, {
  unstable_includeId: true,
});
```

`toLanguageModelMessages` internally uses `toGenericMessages` with AI-SDK-specific transformations. For new integrations prefer `toGenericMessages` directly.

## Assistant Cloud integration

```
import { useCloudRuntime } from "@assistant-ui/react-data-stream";

const runtime = useCloudRuntime({
  cloud: assistantCloud,
  assistantId: "my-assistant-id",
});
```

> [!info]
>
> `useCloudRuntime` is currently under active development and not yet ready for production.

## LocalRuntimeOptions

`useDataStreamRuntime` accepts every `LocalRuntimeOptions` option in addition to its own. The `chatModel` adapter slot is handled internally and cannot be overridden.

```
const runtime = useDataStreamRuntime({
  api: "/api/chat",
  initialMessages: [
    { role: "user", content: [{ type: "text", text: "Hello" }] },
    { role: "assistant", content: [{ type: "text", text: "Hi!" }] },
  ],
  maxSteps: 5,
  cloud, // see "AssistantCloud" in /docs/runtimes/concepts/threads
  adapters: {
    attachments: myAttachmentAdapter,
    history: myHistoryAdapter,
    speech: mySpeechAdapter,
    feedback: myFeedbackAdapter,
    suggestion: mySuggestionAdapter,
  },
});
```

See [adapters](/docs/runtimes/concepts/adapters) for adapter contracts and [LocalRuntime](/docs/runtimes/custom/local-runtime) for inherited options.

## Error handling

The runtime handles common error scenarios automatically:

- Network errors: retried with exponential backoff.
- Stream interruptions: gracefully handled with partial content preserved.
- Tool execution errors: displayed in the UI with error states.
- Cancellation: clean abort signal handling.

## Examples

[`examples/`](https://github.com/assistant-ui/assistant-ui/tree/main/examples) contains reference implementations.

## API reference

For the full hook reference, see [`@assistant-ui/react-data-stream` API](/docs/api-reference/integrations/react-data-stream).

## Related

- [LocalRuntime](/docs/runtimes/custom/local-runtime) — The core runtime data-stream is built on.
- [Assistant Transport](/docs/runtimes/custom/assistant-transport) — State-streaming protocol alternative.
- [Adapters](/docs/runtimes/concepts/adapters) — Attachments, speech, feedback, history, suggestions.