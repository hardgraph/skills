# Integrations
URL: /docs/integrations

Adapters for Vercel AI SDK, LangChain, LangGraph, Mastra, plus auth, persistence, observability, and tool services — drop into a React chat UI built with assistant-ui.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Integrations are wiring guides for using third-party services with assistant-ui, plus canonical recipes for the adapter slots (persistence, attachments). They are distinct from [runtimes](/docs/runtimes/pick-a-runtime), which are the React adapter packages (`@assistant-ui/react-ai-sdk`, `@assistant-ui/react-langgraph`, etc.) that connect the UI to a backend. An integration assumes you already have a working runtime and adds something on top.

## Where integrations slot in

Integrations live on the server. **Agent frameworks** like Mastra take over the API route. **Gateways** swap the upstream provider URL. **Observability** logs or traces every call. **Auth** gates the route and scopes per-user data. **Persistence** and **attachments** are adapter recipes for storing chat data outside the default in-memory path.

## Frameworks

Framework integrations that pair with assistant-ui at the API-route layer.

- [Vercel AI SDK](/docs/integrations/frameworks/ai-sdk) — The canonical first-party adapter. Full setup lives under runtimes; this entry is the architectural pointer.
- **React only**

  [Cloudflare Agents](/docs/integrations/frameworks/cloudflare-agents/overview) — Stateful Durable Object agents. Wired through the AI SDK runtime with WebSocket streaming.
- [Mastra](/docs/integrations/frameworks/mastra/overview) — TypeScript agent framework. Routed through the AI SDK runtime, full-stack or separate server.

## Tools

Tool catalogs and protocols have their own [Tools](/docs/tools) section.

- [Model Context Protocol (MCP)](/docs/tools/mcp) — Connect any MCP server as a tool catalog through the AI SDK MCP client.

## Gateways

OpenAI-compatible proxies that add catalog routing, fallback, BYOK, or self-hosting.

- [LLM gateways](/docs/integrations/gateways) — OpenRouter, Portkey, LiteLLM Proxy. Swap baseURL, route across providers.

## Observability

Log, monitor, trace, and evaluate LLM calls. These pair with any runtime.

- [Helicone](/docs/integrations/observability/helicone) — Observability proxy. Drop-in baseURL swap that logs cost, latency, and prompts.
- [Langfuse](/docs/integrations/observability/langfuse) — OpenTelemetry-based tracing and evals. Open source, self-hostable.
- [LangSmith](/docs/integrations/observability/langsmith) — LangChain ecosystem tracing via wrapAISDK. Datasets and evals included.

**React only**

## Auth

Gate the chat route and scope thread data to the signed-in user. These pages are the **non-cloud** path; pair with [custom thread persistence](/docs/integrations/persistence/custom-adapter) when you own the database.

- [Clerk](/docs/integrations/auth/clerk) — Hosted auth with a Next.js middleware. auth() returns userId; Clerk Orgs map to multi-tenant chat.
- [better-auth](/docs/integrations/auth/better-auth) — TypeScript-first, owns the user table and session. session.user.id is populated by default.
- [Auth.js (next-auth)](/docs/integrations/auth/next-auth) — The OSS Next.js standard. Requires jwt/session callbacks to populate session.user.id.

For AssistantCloud users, [cloud authorization](/docs/cloud/authorization) handles the JWT exchange for Clerk, Auth0, Supabase, and Firebase without DB code.

## Persistence

Store threads and messages outside AssistantCloud.

- [Custom thread persistence](/docs/integrations/persistence/custom-adapter) — Worked example: Postgres + Drizzle, RemoteThreadListAdapter, ThreadHistoryAdapter with withFormat.

## Attachments

Upload chat attachments to object storage instead of inlining as data URLs.

- [Custom attachment uploads](/docs/integrations/attachments/custom-adapter) — Production AttachmentAdapter with presigned URLs. Tabs for S3/R2, Vercel Blob, Uploadthing.

## Don't see your service?

assistant-ui doesn't ship a guide for every tool, but most fit one of two patterns:

- **Routes through your AI SDK handler** (agent frameworks, observability proxies, gateways): adapt the [Mastra full-stack](/docs/integrations/frameworks/mastra/full-stack) or [Helicone proxy](/docs/integrations/observability/helicone) pattern using the service's own SDK.
- **Replaces the runtime entirely** (custom backends): see [custom backend](/docs/runtimes/custom/overview).

If you build something useful, [open an issue](https://github.com/assistant-ui/assistant-ui/issues) or post in [Discord](https://discord.gg/S9dwgCNEFs); the docs are open to contributions.

## Related

- [Pick a runtime](/docs/runtimes/pick-a-runtime) — Choose the React adapter that matches your backend before adding integrations.
- [Runtime concepts](/docs/runtimes/concepts/architecture) — Architecture, adapters, threads, and stability across all runtimes.