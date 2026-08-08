---
name: composio
description: Composio — a tool-calling platform that sits between an LLM/agent framework and external apps, giving an agent connected tools, managed authentication, tool search, triggers, a sandboxed workbench, and an MCP bridge. Use when an agent needs to take real actions in 1000+ SaaS/developer apps (Gmail, GitHub, Slack, Notion, Linear, etc.) without hand-writing each integration, when you want OAuth/API-key auth handled per connected account, when you need the same tools reachable over Model Context Protocol, or when wiring tools into OpenAI, Anthropic, LangChain, Autogen, CrewAI, LlamaIndex, Mastra, or the Vercel AI SDK.
metadata:
  display-name: Composio
  category: AI and agents
  tags: [composio, ai-agents, tool-calling, mcp, integrations, managed-auth]
---

# Composio

Composio is a tool-calling layer between your model and the outside world. The
job it does is the part of building a tool-using agent that is boring, brittle,
and security-sensitive: keeping tool schemas current, brokering OAuth and API
keys for every connected account, and turning the result into something your
framework can already call.

The bet is that you stop maintaining one auth flow and one JSON-schema wrapper
per integration. You tell Composio which tools a user needs; it returns those
tools as provider-ready schemas and runs the actual API call (with that user's
stored credentials) when the model invokes one. The same tool set is reachable
directly as function-calling schemas or over Model Context Protocol from the
same session.

## Mental model

| Concept               | What it is                                                                                                                                            |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Toolkit**           | A bundle of related tools for one app (e.g. the GitHub toolkit). Composio publishes 1000+.                                                            |
| **Tool**              | One callable action with a JSON schema the model can choose and fill.                                                                                 |
| **Session**           | A per-user (or per-agent) handle that resolves a configured set of tools, their auth, and optional MCP URL. Created with `composio.create(...)`.      |
| **Connected account** | One user's stored credential for one app (an OAuth grant or an API key). Auth attaches here, not to the tool.                                         |
| **Provider adapter**  | The shim that renders a session's tools in your framework's shape (`composio_openai`, `@composio/openai`, LangChain, etc.).                           |
| **Trigger**           | An event-driven tool: when something happens in an app, Composio fires a webhook your agent reacts to. Opposite of an on-demand tool the model calls. |
| **Sandbox**           | A hosted, isolated environment (formerly "workbench") where tool code runs; remote or local.                                                          |
| **Tool search**       | Given many toolkits, pick the few relevant to a request so the model is not flooded with schemas.                                                     |

The flow is: create a session for a user, get the connected tools back as
provider-ready schemas, hand those to the model, and when the model emits a tool
call the provider adapter returns the result. Authentication is resolved against
the connected account at call time, not encoded in the schema the model sees.

## When to use Composio vs hand-rolled function calling

| Reach for Composio when…                                                                        | Hand-roll instead when…                                                  |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| The integration needs OAuth (token refresh, scopes, redirects) and you do not want to own that. | It is a single internal API with a static key and a stable schema.       |
| You need many app integrations and cannot maintain each one's schema drift.                     | You need tight control over exact request/response shape or retry logic. |
| You want the same tools callable from multiple frameworks or over MCP without rewriting.        | The tool is a pure compute function with no external credential.         |
| Per-user/per-tenant credential storage and rotation is the actual problem.                      | Credentials are a single shared service account that never rotates.      |

Composio earns its place at the auth-and-schema boundary. If you would have
written the same OAuth dance three times, it is the right tool. If you would
have written one `fetch`, it is overhead.

## The provider adapter layer

Tools come back framework-shaped. A session built for OpenAI yields OpenAI
function-calling JSON; the same Composio account wired to LangChain yields
LangChain tools. The adapter is a thin render layer over one canonical tool
definition — you pick the adapter by the model client you already use.

First-class adapters exist for OpenAI, Anthropic, the Vercel AI SDK, Google,
LangChain, Autogen, CrewAI, LlamaIndex, and Mastra, plus a path for custom
providers. Switching framework usually means swapping the adapter package, not
rewriting the tool wiring.

## Managed authentication

This is the load-bearing feature. For each connected account Composio owns:

- the OAuth handshake (or API-key submission),
- token storage and refresh,
- scope enforcement,
- redirect/white-label configuration.

Two modes matter when you design the connection flow:

- **Managed app** — Composio's own OAuth client is used; fastest to start, the
  end user sees Composio's branding unless you white-label.
- **Custom app** — you bring your own OAuth client and credentials; the user
  sees your brand and you keep the app registration. The trade is setup cost
  for branding and ownership.

Multiple connected accounts per user (two Gmail inboxes, three Slack
workspaces) are first-class: a tool call targets a specific account, not "the
Gmail account." Shared connections and importing existing connections exist for
team and migration scenarios.

## Triggers vs on-demand tools

| On-demand tool                     | Trigger                                               |
| ---------------------------------- | ----------------------------------------------------- |
| Model decides to call it now.      | An event in an app causes a webhook to fire.          |
| Pull model — agent acts on demand. | Push model — agent reacts to something that happened. |
| Typical: "send this email."        | Typical: "a new GitHub PR was opened, review it."     |

Triggers turn Composio into an event source: you subscribe to events, Composio
delivers them as webhooks, and your agent responds. Custom OAuth webhooks extend
this to apps that need per-event OAuth setup. Choose triggers when the agent
should react to the world; choose on-demand tools when the agent drives.

## The MCP bridge

The same session is reachable over Model Context Protocol. Create the session
with `mcp: true` and read back an MCP server URL; any MCP-compatible client can
then enumerate and call the session's tools. This is the path when your agent is
already an MCP host, or when you want one tool surface that both function-calling
clients and MCP clients consume.

Reach for the MCP bridge over the native adapters when your runtime speaks MCP
already — you avoid a Composio-specific dependency in the agent process. Reach
for the native adapters when you want the framework's native tool shape and
ergonomics.

## Tool search and the sandbox

- **Tool search** solves the "1000 toolkits, finite context window" problem:
  given a user request, return the handful of relevant tools so the model is
  choosing among five schemas, not five thousand. Use it whenever you expose a
  broad catalog to a single model turn.
- **Sandbox (remote or local)** is where tool execution can run isolated —
  useful for tools that execute code (bash, code review, PR actions) rather
  than just call a SaaS API. The local sandbox runs in your environment; the
  remote sandbox is hosted.

## Current vs deprecated

- The **current API is v3.1**; **v3.0 is legacy** ("supported, not for new
  code"). New integrations target the current reference; treat any v3.0-only
  surface as a migration target. Confirm the current version against the
  reference, not memory — Composio revises this surface.
- **Sessions replaced the older `toolkits`/direct-tool model.** The migration
  guides cover direct-to-sessions, MCP-servers-to-sessions, and the tool-router
  beta. If older example code uses `ComposioToolSet` / `get_toolkits` patterns,
  expect to port it to the session model.
- **SDK versions and supported frameworks move.** Resolve the current package
  names and adapter wiring from the SDK and provider references for Python and
  TypeScript rather than recalling them.
- **"Sandbox" is the current name; "workbench" is the prior one.** Older
  material and some meta-tool names still say workbench.

## References

- [Composio docs (overview)](https://docs.composio.dev/docs.md)
- [Quickstart](https://docs.composio.dev/docs/quickstart.md)
- [How Composio works](https://docs.composio.dev/docs/how-composio-works.md)
- [Providers](https://docs.composio.dev/docs/providers.md) and per-framework adapters (OpenAI, Anthropic, Vercel, LangChain, Autogen, CrewAI, LlamaIndex, [Mastra](https://docs.composio.dev/docs/providers/mastra.md))
- [Authentication](https://docs.composio.dev/docs/authentication.md) and [custom vs managed app](https://docs.composio.dev/docs/custom-app-vs-managed-app.md)
- [Sessions over MCP](https://docs.composio.dev/docs/sessions-via-mcp.md)
- [Triggers](https://docs.composio.dev/docs/triggers.md)
- [Sandbox (remote)](https://docs.composio.dev/docs/sandbox/remote.md) / [local](https://docs.composio.dev/docs/sandbox/local.md)
- [Migration guide](https://docs.composio.dev/docs/migration-guide.md)
- [API reference (current)](https://docs.composio.dev/reference.md) and [rate limits](https://docs.composio.dev/reference/rate-limits.md)
- [Toolkits catalog](https://docs.composio.dev/toolkits.md)
