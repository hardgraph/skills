# composio

![composio cover](./assets/readme-cover.png)

Reference skill for [Composio](https://composio.dev/) — the tool-calling
platform that sits between an LLM/agent framework and external apps. It steers
an agent through sessions, managed authentication, the provider adapter layer
(OpenAI, Anthropic, Vercel AI SDK, LangChain, Autogen, CrewAI, LlamaIndex,
Mastra), triggers, the sandboxed workbench, tool search, and the MCP bridge,
without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill composio
```

## Use this skill for

- Giving an agent tools that take real actions in SaaS/developer apps without hand-writing each integration
- Offloading OAuth/API-key auth, token refresh, and per-user credential storage to a managed layer
- Exposing the same connected tools as both native function-calling schemas and over Model Context Protocol
- Choosing between managed-app and custom-app OAuth when designing the connection flow
- Wiring Composio tools into OpenAI, Anthropic, LangChain, Autogen, CrewAI, LlamaIndex, Mastra, or the Vercel AI SDK
- Deciding between on-demand tools the model calls and triggers that react to app events
- Migrating older direct-toolkit code to the current session model and avoiding the legacy v3.0 API surface

## What is included

- [`SKILL.md`](./SKILL.md) — the mental model and the decision criteria for hosted tools vs hand-rolled function calling, auth modes, triggers, and the MCP bridge.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible verbatim mirror of the Composio documentation the seed index links to, used for exact SDK and API reference.

## Source

Reference material is reproduced from the
[Composio documentation](https://docs.composio.dev) via its official
[llms.txt index](https://docs.composio.dev/llms.txt).
