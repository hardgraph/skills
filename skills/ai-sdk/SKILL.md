---
name: ai-sdk
description: Vercel AI SDK — provider-agnostic TypeScript toolkit for building streaming AI applications and agents. Use when generating or streaming text with LLMs, wiring model providers (OpenAI, Anthropic, Google), implementing tool calling, producing structured output with Zod, building chat UIs with useChat, or composing multi-step agents with ToolLoopAgent.
---

# Vercel AI SDK

The AI SDK is a provider-agnostic TypeScript toolkit for building AI-powered
applications and agents. It runs on React, Next.js, Vue, Svelte, Node.js, and
other JavaScript runtimes. A model call stays the same shape whether the provider
is OpenAI, Anthropic, Google, Mistral, or a custom one.

## Mental model

Two layers, kept deliberately separate:

| Layer           | Package                                          | What it does                                                                                                           |
| --------------- | ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| **AI SDK Core** | `ai`                                             | Server-side model calls: `generateText`, `streamText`, `generateObject`, `streamObject`, tools, embeddings, providers. |
| **AI SDK UI**   | `@ai-sdk/react`, `@ai-sdk/svelte`, `@ai-sdk/vue` | Framework hooks for chat and generative UI: `useChat`, `useCompletion`, `useObject`.                                   |

A provider package (`@ai-sdk/openai`, `@ai-sdk/anthropic`, …) plugs a vendor's
models into Core. UI talks to a route handler that calls Core; it never imports a
provider directly.

## Resolving versions

**Resolve the current SDK and model versions from the registry rather than
recalling one.** Training-data versions are reliably wrong.

```bash
npm view ai version          # Core
npm view @ai-sdk/react version
npm view @ai-sdk/openai version
```

The major version of `ai` and the provider/adapter packages move together on a
release. When upgrading, bump `ai`, the provider packages, and the UI adapter as
a set — a mixed set is the most common source of breakage.

## Core calls

```ts
import { generateText, streamText } from "ai";
import { openai } from "@ai-sdk/openai";

// One-shot
const { text } = await generateText({
  model: openai("gpt-4o"),
  prompt: "Summarise the AI SDK in one sentence.",
});

// Streaming
const result = streamText({
  model: openai("gpt-4o"),
  prompt: "Tell me a story.",
});
for await (const delta of result.textStream) {
  process.stdout.write(delta);
}
```

## Structured output

Constrain output to a schema with `generateObject` / `streamObject` and a Zod
definition:

```ts
import { generateObject } from "ai";
import { z } from "zod";

const { object } = await generateObject({
  model: openai("gpt-4o"),
  schema: z.object({
    name: z.string(),
    rating: z.number().min(0).max(10),
  }),
  prompt: "Describe the AI SDK.",
});
```

## Tool calling

Define tools with `tool` and a Zod `parameters` schema; the model decides when to
call them and you return a result it consumes:

```ts
import { generateText, tool } from "ai";
import { z } from "zod";

const { text } = await generateText({
  model: openai("gpt-4o"),
  tools: {
    weather: tool({
      description: "Get the weather for a city",
      parameters: z.object({ city: z.string() }),
      execute: async ({ city }) => fetchWeather(city),
    }),
  },
  maxSteps: 5, // lets the model call a tool and use the result
  prompt: "What is the weather in Hanoi?",
});
```

`maxSteps` (previously `maxToolRoundtrips`) enables multi-step loops where the
model calls a tool, reads the result, and continues.

## Chat UI

`useChat` from `@ai-sdk/react` connects a React component to a route handler that
streams Core output. Use `DefaultChatTransport` to point it at your endpoint:

```ts
import { useChat } from "@ai-sdk/react";
import { DefaultChatTransport } from "ai";

const { messages, sendMessage, status } = useChat({
  transport: new DefaultChatTransport({ api: "/api/chat" }),
});
```

Messages are `UIMessage` objects made of typed `parts` (`TextUIPart`,
`ToolUIPart`, `FileUIPart`, …) — render by part type rather than treating
`message.content` as a plain string.

## Agents

`ToolLoopAgent` runs a model that can call tools across multiple steps with stop
conditions, human-in-the-loop approval, and an MCP client for external tool
servers. Reach for it when a single `maxSteps` loop is not enough — you need
guarded execution, sub-agents, or dynamic tool selection.

## Error handling

Catch provider errors by type rather than sniffing strings:

- `APICallError` — the provider rejected the request (auth, quota, bad input).
- `NoSuchToolError` — the model returned a tool you did not define.
- `UIMessageStreamError` — a UI stream surfaced an error part.

## Current vs deprecated

- Use `maxSteps` for multi-step tool calls; `maxToolRoundtrips` is the old name.
- UI messages are part-based (`UIMessage`); the legacy string `content` model is
  gone in v4+.
- Prefer `streamText` over manual chunked generation; prefer `generateObject`
  over prompt-engineering JSON.

## References

- [AI SDK Core](https://ai-sdk.dev/docs/ai-sdk-core.md)
- [AI SDK UI](https://ai-sdk.dev/docs/ai-sdk-ui.md)
- [Agents](https://ai-sdk.dev/docs/agents.md)
- [Providers](https://ai-sdk.dev/providers/ai-sdk-providers.md)
- [API reference](https://ai-sdk.dev/docs/reference.md)
- [Getting started](https://ai-sdk.dev/docs/getting-started.md)
- [npm: ai](https://www.npmjs.com/package/ai)
