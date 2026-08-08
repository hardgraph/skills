# Picking a runtime
URL: /docs/runtimes/pick-a-runtime

Decision guide for choosing the right runtime, by framework or by feature.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A runtime is the connection between assistant-ui's UI primitives and your AI backend. This page helps you pick one. Two lenses, pick whichever maps to what you already know.

## Lens 1: by framework

If you are already using one of these frameworks, the choice is mechanical.

### First-party adapters

assistant-ui ships React adapter packages for these. Pick the matching card and follow its overview.

- [Vercel AI SDK](/docs/runtimes/ai-sdk/overview) — useChat hook, streaming, tools, attachments, multi-step. v6 current; v5 / v4 legacy.
- [Eve](/docs/runtimes/eve/overview) — Filesystem-first durable agents with eve/next, sessions, NDJSON streaming, and HITL.
- [LangGraph](/docs/runtimes/langgraph/overview) — Direct integration with @langchain/langgraph-sdk. Subgraph events, UI messages, message metadata.
- [LangChain useStream](/docs/runtimes/langchain) — Wraps @langchain/react's useStream. Lighter-weight, tracks upstream.
- [Google ADK](/docs/runtimes/google-adk/overview) — ADK JS or Python agents. Tool confirmations, auth flows, multi-agent, code execution.
- [A2A Protocol](/docs/runtimes/a2a/overview) — Any A2A v1.0-compliant agent server. Streaming task state, artifacts, multi-tenancy.
- [AG-UI Protocol](/docs/runtimes/ag-ui/overview) — AG-UI agents (CopilotKit, custom servers). Streaming text, thinking, tool calls, state snapshots.
- **React only**

  [OpenCode](/docs/runtimes/opencode/overview) — OpenCode coding-agent server. Permission flows, questions, fork / revert. Experimental.

**React only**

### Integration guides

For frameworks without a dedicated adapter, these wiring guides route through one of the adapters above.

- [Cloudflare Agents](/docs/integrations/frameworks/cloudflare-agents/overview) — Stateful agents on Durable Objects with WebSocket streaming. Wired through the Vercel AI SDK runtime.
- [Mastra](/docs/integrations/frameworks/mastra/overview) — TypeScript agent framework. Wired through the Vercel AI SDK runtime.

## Lens 2: by needs

If you do not know your framework yet, or your backend is custom, pick by what you need:

| You need                                              | Use                                                             |
| ----------------------------------------------------- | --------------------------------------------------------------- |
| Simple `fetch` call to your API, runtime owns state   | [LocalRuntime](/docs/runtimes/custom/local-runtime)             |
| Keep messages in redux, zustand, tanstack-query, etc. | [ExternalStoreRuntime](/docs/runtimes/custom/external-store)    |
| Backend that already speaks the data stream protocol  | [DataStream](/docs/runtimes/custom/data-stream)                 |
| Stream full agent state snapshots (not just messages) | [AssistantTransport](/docs/runtimes/custom/assistant-transport) |

If none of the framework adapters fits, start at [custom backend](/docs/runtimes/custom/overview).

## Shared concepts

Regardless of which runtime you pick, four ideas apply across the board.

- **Architecture** — Framework adapters wrap one of two **core runtimes** (`LocalRuntime`, `ExternalStoreRuntime`). See [architecture](/docs/runtimes/concepts/architecture) for the full layering.
- **Adapters** (attachments, speech, feedback, history, suggestions) work the same way across runtimes. See [adapters](/docs/runtimes/concepts/adapters).
- **Threads** (single, cloud, custom database) follow the same model. See [threads](/docs/runtimes/concepts/threads).
- **Stability** policy: APIs prefixed with `unstable_` may change in any release. See [stability](/docs/runtimes/concepts/stability).

> [!info]
>
> Need help? Join our [Discord community](https://discord.gg/S9dwgCNEFs) or check the [GitHub repo](https://github.com/assistant-ui/assistant-ui).