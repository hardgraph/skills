# Vercel AI SDK Integration
URL: /docs/integrations/frameworks/ai-sdk

Wire the Vercel AI SDK into a React chat UI with assistant-ui — useChat, streaming, tools, attachments, multi-step agents, and persistence covered end-to-end.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

[Vercel AI SDK](https://ai-sdk.dev/) is the most common framework people pair with assistant-ui. The full setup, attachments, persistence, tool-call patterns, and version notes are documented under [runtimes/ai-sdk](/docs/runtimes/ai-sdk/overview); this page is the entry point in the integrations tree for discoverability and architecture context.

> [!info]
>
> If you arrived here looking to wire up your first chat: jump to [AI SDK v7 quickstart](/docs/runtimes/ai-sdk/v7). This page is a high-level pointer.

## Where it slots in

`@assistant-ui/react-ai-sdk` wraps the AI SDK's `useChat` hook and exposes it as an assistant-ui runtime. The runtime owns conversation state on the client; your `/api/chat` route returns a UI message stream from `streamText`. Everything else (tools, attachments, observability, gateways, custom persistence) layers on top of this base.

## Pick a version

Four versions of `ai` are supported. New projects should pick **v7**; v6, v5, and v4 are documented for migration and existing apps that haven't upgraded.

- [AI SDK v7 (current)](/docs/runtimes/ai-sdk/v7) — Requires ai@^7 and @ai-sdk/react@^4. Async convertToModelMessages, tool inputSchema, toUIMessageStreamResponse.
- [AI SDK v6 (legacy)](/docs/runtimes/ai-sdk/v6-legacy) — Requires ai@^6 and @ai-sdk/react@^3. Async convertToModelMessages, tool inputSchema, toUIMessageStreamResponse.
- [AI SDK v5 (legacy)](/docs/runtimes/ai-sdk/v5-legacy) — Requires ai@^5 and @ai-sdk/react@^2. Synchronous convertToModelMessages, transitional API.
- [AI SDK v4 (legacy)](/docs/runtimes/ai-sdk/v4-legacy) — The original useChat-based path. Maintained for migration only.

## When to pick AI SDK

AI SDK is the default choice for new projects on Next.js, Remix, or any framework with a Node-compatible API route. Pick it when:

- You want a single direct path from the chat UI to your model with the smallest possible code surface.
- You will compose with a framework like [Mastra](/docs/integrations/frameworks/mastra/overview), an [observability tool](/docs/integrations/observability/helicone), an [LLM gateway](/docs/integrations/gateways), or [tools through MCP](/docs/tools/mcp), all of which assume an AI SDK route.
- You want first-party `frontendTools`, attachments, multi-step tool calls, token-usage metadata, and persisted history via `withFormat`.

If you need streaming agent state (subgraph events, generative UI messages), look at [LangGraph](/docs/runtimes/langgraph/overview) instead. If you have a different protocol-shaped backend (A2A, AG-UI, OpenCode), see [pick a runtime](/docs/runtimes/pick-a-runtime).

## Related

- [AI SDK runtime overview](/docs/runtimes/ai-sdk/overview) — The full runtime documentation and version selector.
- [Pick a runtime](/docs/runtimes/pick-a-runtime) — Decision guide if you're not sure AI SDK is the right runtime.
- [Mastra](/docs/integrations/frameworks/mastra/overview) — The other framework integration in this section. Wired through AI SDK.