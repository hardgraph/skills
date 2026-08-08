# Mastra Integration
URL: /docs/integrations/frameworks/mastra/overview

Wire the Mastra TypeScript agent framework into a React chat UI with assistant-ui — full streaming, tool calling, multi-agent support, and thread management.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

[Mastra](https://mastra.ai/) is an open-source TypeScript agent framework. It provides primitives for AI applications: agents with memory and tool calling, deterministic LLM workflows, RAG, model routing, workflow graphs, and automated evals.

> [!info]
>
> This is an integration guide, not a runtime adapter. assistant-ui does not ship a `@assistant-ui/react-mastra` package. You wire up Mastra through the standard [AI SDK runtime](/docs/runtimes/ai-sdk/v7) by routing your API endpoint through Mastra's agent stream.

## Pick a pattern

| Pattern                                                                 | When to pick                                                                                                            |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| [Full-stack](/docs/integrations/frameworks/mastra/full-stack)           | One Next.js app: API routes call Mastra in-process. Simpler deployment, single repo.                                    |
| [Separate server](/docs/integrations/frameworks/mastra/separate-server) | Mastra runs as its own service; the Next.js frontend hits its API. Independent scaling, clearer separation of concerns. |

Both use the same client-side `useChatRuntime` from [`@assistant-ui/react-ai-sdk`](/docs/runtimes/ai-sdk/v7). The only difference is where the Mastra agent lives.

## Architecture

Mastra integrates at the LLM-client layer on the server. assistant-ui talks to your API route via the AI SDK runtime; the route calls `agent.stream(messages)` and returns the result wrapped in a UI message stream. The client side is built on [`ExternalStoreRuntime`](/docs/runtimes/custom/external-store) through the AI SDK adapter.

Shared adapters (attachments, speech, feedback, history) work the same way described in [adapters](/docs/runtimes/concepts/adapters). Multi-thread support uses [AssistantCloud](/docs/cloud) or a [custom thread list](/docs/runtimes/concepts/threads).

## Requirements

- A Next.js project, or another framework that can run AI SDK route handlers.
- Model API keys (OpenAI, Anthropic, etc.) configured in your environment.
- Node 18 or newer.

## Next

- [Full-stack integration](/docs/integrations/frameworks/mastra/full-stack) — Run Mastra inside your Next.js API routes.
- [Separate server integration](/docs/integrations/frameworks/mastra/separate-server) — Run Mastra as a standalone server, frontend connects via API.
- [AI SDK runtime](/docs/runtimes/ai-sdk/v7) — The runtime that handles the client side of this integration.