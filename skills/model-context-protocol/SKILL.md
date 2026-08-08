---
name: model-context-protocol
description: Model Context Protocol (MCP) — the open JSON-RPC 2.0 standard that lets an AI host application (an IDE, a coding agent, a chat desktop) call external Tools, read Resources, run Prompts, and elicit user input from any compliant MCP server over stdio or Streamable HTTP. Use it when building or consuming an MCP server or client, choosing between the Python/TypeScript/C#/Go/Java/Rust SDKs, defining tools/resources/prompts, negotiating capabilities and protocol versions, wiring authorization (OAuth 2.1, enterprise-managed, client credentials), testing with the MCP Inspector, or publishing to the MCP Registry.
metadata:
  display-name: Model Context Protocol
  category: AI and agents
  tags: [mcp, model-context-protocol, agents, tools, llm, protocol]
---

# Model Context Protocol

MCP is a wire protocol, not a runtime. It defines how an AI application and an
external program exchange context — what functions exist, what data is
reachable, what the user should be asked — over JSON-RPC 2.0, regardless of who
wrote either side. The bet is the same as JDBC or LSP: agree on the contract so
that any client can drive any server, and every new server is instantly usable
by every existing host.

The protocol is owned by the Model Context Protocol Project (an open community
stewarded by Anthropic and other contributors), versioned by calendar date, and
moves fast. Treat the spec version, the SDK tiers, and the deprecated list as
mutable facts — resolve them from the upstream pages linked under References
rather than from memory.

## Mental model: host, client, server

MCP is a client-server architecture sitting inside one host application.

| Role       | What it is                                                                                      |
| ---------- | ----------------------------------------------------------------------------------------------- |
| **Host**   | The AI application that wants more context or actions — Claude Desktop, an IDE, a custom agent. |
| **Client** | A connection object the host creates **per server**. One server = one client inside the host.   |
| **Server** | The program exposing context/actions. Runs locally (stdio) or remotely (Streamable HTTP).       |

A host federates many servers. Visual Studio Code, for example, is one host; it
spawns one client per connected server (filesystem, Sentry, a database, …) and
merges their primitives into a single registry the model can reach. Local
stdio servers typically serve one client; a remote Streamable HTTP server
typically serves many.

The protocol is layered:

- **Data layer** — JSON-RPC 2.0: requests, responses, notifications, the
  primitive set (tools/resources/prompts), capability and version discovery.
- **Transport layer** — how the bytes move: **stdio** (local subprocess) or
  **Streamable HTTP** (remote, HTTP POST + optional SSE streaming). The data
  layer is identical across both.

## The capability primitives

Primitives are the vocabulary. Each is a set of JSON-RPC methods with a
`*/list`, a `*/get` (or `*/call`), and change notifications. They split by who
exposes them.

| Primitive       | Side    | Status (2026-07-28) | What it is                                                                                                             |
| --------------- | ------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Tools**       | Server  | Active              | Functions the model can invoke (`tools/call`). The workhorse.                                                          |
| **Resources**   | Server  | Active              | Addressable context data (`resources/read`), keyed by URI.                                                             |
| **Prompts**     | Server  | Active              | Reusable interaction templates (`prompts/get`).                                                                        |
| **Elicitation** | Client  | Active              | Server asks the user for structured input (`elicitation/create`).                                                      |
| **Sampling**    | Client  | **Deprecated**      | Server asks the host's LLM for a completion. Migrate to direct LLM provider calls.                                     |
| **Logging**     | Utility | **Deprecated**      | Server→client log messages. Migrate to stderr / OpenTelemetry.                                                         |
| **Roots**       | Client  | **Deprecated**      | Client tells the server which filesystem roots to scope to. Pass paths via tool args / resource URIs / config instead. |

Rule of thumb: servers **expose** (tools/resources/prompts); clients **elicit**
(ask the user). The deprecated set (sampling, logging, roots) was deprecated by
SEP-2577 in spec `2026-07-28` — still specced, scheduled for removal no earlier
than the first revision on or after `2027-07-28`, but **new servers should not
adopt them**. See the deprecation table below for the full picture.

Tool definitions carry a `name`, a human-readable `title`, a `description`, and
an `inputSchema` (JSON Schema) plus an optional `outputSchema`. The model sees
the description and schema; good descriptions are how tools get chosen
correctly. Tool names are namespaced (e.g. `weather_current`, not `current`)
to avoid collisions across federated servers.

## Transports: stdio vs Streamable HTTP

| Transport           | Use when                                                                 | Notes                                                                                                          |
| ------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| **stdio**           | Local tools, full filesystem/process access, one client, no auth needed. | Host launches the server as a subprocess; messages over stdin/stdout. Lowest latency.                          |
| **Streamable HTTP** | Remote/multi-tenant servers, internet-hosted, needs real auth.           | HTTP POST for client→server; optional SSE for streaming. Bearer/API-key/custom-header auth; OAuth recommended. |

The older **HTTP+SSE** transport is **deprecated** (since `2025-03-26`) and
replaced by Streamable HTTP — build new remote servers on Streamable HTTP.
The Inspector's "protocol eras" handling exists precisely to bridge legacy
(`initialize` handshake) and modern (`server/discover`) servers, so a client
can still talk to both.

## Lifecycle and message shape

Two flows exist. Modern servers use **discovery**; legacy and many SDK examples
still show the **initialize** handshake. A robust client supports both.

**Modern (stateless discovery, `2026-07-28`).** The protocol is now stateless:
**every** request carries `protocolVersion` and the sender's `capabilities`
(and normally its identity) in a `_meta` block, so the server needs no
connection memory.

1. Client sends `server/discover` (mandatory on the server side; optional for
   the client). The response lists `supportedVersions`, the server's
   `capabilities`, its identity, and a `ttlMs`/`cacheScope` hint so discovery
   can be cached instead of repeated per request.
2. Client lists primitives (`tools/list`, `resources/list`, `prompts/list`),
   optionally opens a `subscriptions/listen` stream for change notifications.
3. Client calls (`tools/call`), reads (`resources/read`), or runs templates.
4. If a version is unsupported the server returns an
   `UnsupportedProtocolVersionError` listing the versions it does support; the
   client retries with a mutually supported version.

**Legacy (`initialize` → `initialized`).** Older servers expect an explicit
`initialize` request/response negotiating `protocolVersion` and capabilities,
followed by an `initialized` notification, before any other call. This is the
flow many SDK quickstarts and most pre-2026 servers use. When connecting to an
unknown server, assume it may need the legacy handshake; the Inspector
negotiates the era automatically.

Conceptually a JSON-RPC 2.0 message is `{ jsonrpc: "2.0", id, method, params }`
for a request, the same minus `method`/`params` plus `result` for a response,
and **no `id`** for a notification. Capabilities gate which methods are legal:
a server that didn't advertise `tools` has no business receiving `tools/call`.

## Authorization

Remote (Streamable HTTP) servers secure themselves with **OAuth**, on top of
standard HTTP bearer/API-key/custom-header schemes. The base spec targets
OAuth 2.1; authorization-server discovery follows RFC 9728 protected-resource
metadata.

| Extension / mechanism                | When                                                                                 |
| ------------------------------------ | ------------------------------------------------------------------------------------ |
| **Authorization Server Discovery**   | Client finds the OAuth metadata for a remote server.                                 |
| **Client ID Metadata Documents**     | URL-based client registration — **replaces** deprecated Dynamic Client Registration. |
| **OAuth Client Credentials**         | Machine-to-machine (no end user). SEP-1046 extension.                                |
| **Enterprise-Managed Authorization** | Centralized IdP policy control for corporate deployments.                            |

Local stdio servers usually skip OAuth entirely — the host's process trust is
the trust boundary. Don't bolt OAuth onto a stdio server that only the local
host can reach.

## SDKs and the tiering system

The official SDKs implement the same surface idiomatically per language. They
are **tiered** by feature completeness, protocol-version support, and
maintenance commitment — pick a Tier 1 SDK for anything production-critical.

| Tier  | SDKs                       | Meaning                                                              |
| ----- | -------------------------- | -------------------------------------------------------------------- |
| **1** | TypeScript, Python, C#, Go | Full feature/protocol coverage, first-class support. Default choice. |
| **2** | Java, Rust, Ruby           | Solid coverage, may lag on newest spec features.                     |
| **3** | Swift, PHP, Kotlin         | Community/maintenance tiers; verify the feature you need.            |

All SDKs can build both servers and clients and support both transports. The
Python and TypeScript SDKs are the reference implementations most quickstarts
and the Inspector itself are written against.

## Tooling

- **MCP Inspector** — the interactive test/debug client. Runs in the browser,
  as a TUI, and as a CLI. Use it to exercise a server's tools/resources/prompts
  by hand, watch JSON-RPC traffic, step the OAuth flow, and compare legacy vs.
  modern protocol behavior. The CLI form is scriptable for CI.
- **MCP Registry** — the official, moderated index of publishable servers.
  Publishing a remote server to the registry is how hosts discover it; there
  are GitHub Actions and versioning docs for automating that.
- **MCP Apps** — an extension letting a server render interactive UI inside a
  host (e.g. Claude Desktop) beyond plain tool results.

## Decision criteria

| You want to…                                      | Reach for                                                            |
| ------------------------------------------------- | -------------------------------------------------------------------- |
| Give an agent a new action (API call, query)      | A **Tool**.                                                          |
| Give an agent read-only context (a schema, a doc) | A **Resource**, addressed by URI.                                    |
| Ship a reusable prompt/few-shot template          | A **Prompt**.                                                        |
| Server needs to ask the user something            | **Elicitation** (active).                                            |
| Server wants the host's LLM                       | Don't use **Sampling** (deprecated) — call an LLM provider directly. |
| Local tool, full disk/process access              | **stdio** server.                                                    |
| Internet-hosted, multi-tenant, needs login        | **Streamable HTTP** server + OAuth.                                  |
| Build in production                               | A **Tier 1** SDK (TypeScript/Python/C#/Go).                          |
| Test or debug a server                            | **MCP Inspector**.                                                   |
| Make a server discoverable                        | Publish to the **MCP Registry**.                                     |

## Current vs deprecated (spec `2026-07-28`)

Resolve the live list from the upstream Deprecated Features page — it is the
normative record and changes per revision. As of `2026-07-28`:

| Deprecated feature               | Migrate to                                                            |
| -------------------------------- | --------------------------------------------------------------------- |
| Roots                            | Pass dirs/files via tool parameters, resource URIs, or server config. |
| Sampling                         | Integrate directly with LLM provider APIs.                            |
| Logging                          | Log to `stderr` (stdio) or use OpenTelemetry.                         |
| Dynamic Client Registration      | Client ID Metadata Documents.                                         |
| HTTP+SSE transport               | Streamable HTTP.                                                      |
| `includeContext` sampling values | Omit the field or use `"none"`.                                       |

Nothing has been removed under the policy yet, but the earliest-removal window
for the SEP-2577 batch is the first revision on or after `2027-07-28`. Plan
migrations before then; don't start new code on a deprecated primitive.

MCP is complementary to, not competing with, Google's **Agent2Agent (A2A)**
protocol: MCP standardizes an _agent's tools and context_, A2A standardizes
_agent-to-agent_ collaboration. They compose — an agent can be an MCP host and
an A2A participant simultaneously.

## References

- [What is MCP? (getting started)](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro)
- [Architecture overview](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture)
- [Understanding MCP servers](https://modelcontextprotocol.io/docs/2026-07-28/learn/server-concepts)
- [Understanding MCP clients](https://modelcontextprotocol.io/docs/2026-07-28/learn/client-concepts)
- [Build an MCP server](https://modelcontextprotocol.io/docs/2026-07-28/develop/build-server)
- [Build an MCP client](https://modelcontextprotocol.io/docs/2026-07-28/develop/build-client)
- [Client Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/develop/clients/client-best-practices)
- [Specification (2026-07-28)](https://modelcontextprotocol.io/specification/2026-07-28/index)
- [Transports (stdio, Streamable HTTP)](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/index)
- [Server primitives: Tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools) · [Resources](https://modelcontextprotocol.io/specification/2026-07-28/server/resources) · [Prompts](https://modelcontextprotocol.io/specification/2026-07-28/server/prompts)
- [Elicitation](https://modelcontextprotocol.io/specification/2026-07-28/client/elicitation)
- [Authorization (OAuth 2.1)](https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/index)
- [Deprecated Features](https://modelcontextprotocol.io/specification/2026-07-28/deprecated)
- [Versioning](https://modelcontextprotocol.io/docs/2026-07-28/learn/versioning)
- [SDKs and tiering](https://modelcontextprotocol.io/docs/2026-07-28/sdk) · [SDK Tiering System](https://modelcontextprotocol.io/community/sdk-tiers)
- [MCP Inspector](https://modelcontextprotocol.io/docs/2026-07-28/tools/inspector) · [Inspector CLI](https://modelcontextprotocol.io/docs/2026-07-28/tools/inspector/cli) · [Inspector protocol eras](https://modelcontextprotocol.io/docs/2026-07-28/tools/inspector/protocol-eras)
- [MCP Registry](https://modelcontextprotocol.io/registry/about) · [Publishing quickstart](https://modelcontextprotocol.io/registry/quickstart)
- [Security Best Practices](https://modelcontextprotocol.io/docs/2026-07-28/tutorials/security/security_best_practices)
- [Feature Lifecycle and Deprecation Policy](https://modelcontextprotocol.io/community/feature-lifecycle)
