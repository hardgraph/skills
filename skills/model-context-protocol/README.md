# model-context-protocol

![model-context-protocol cover](./assets/readme-cover.png)

Reference skill for the [Model Context Protocol](https://modelcontextprotocol.io)
— the open JSON-RPC 2.0 standard that lets an AI host application (an IDE, a
coding agent, a chat desktop) call external Tools, read Resources, run Prompts,
and elicit user input from any compliant MCP server over stdio or Streamable
HTTP. It steers an agent through the host/client/server model, the capability
primitives, transport and lifecycle choices, OAuth authorization, the SDK
tiering system, the MCP Inspector, the Registry, and which features are current
versus deprecated — without relying on stale spec-version recall.

## Install

```bash
npx skills add hardgraph/skills --skill model-context-protocol
```

## Use this skill for

- Building an MCP server that exposes Tools, Resources, and Prompts
- Building an MCP client or host that federates many servers
- Choosing between stdio and Streamable HTTP transports
- Picking a Tier 1/2/3 SDK (Python, TypeScript, C#, Go, Java, Rust, …)
- Wiring OAuth 2.1 authorization, client credentials, or enterprise-managed auth
- Defining tool `inputSchema`/`outputSchema` and elicitation flows
- Debugging a server with the MCP Inspector (browser, TUI, or CLI)
- Publishing a remote server to the MCP Registry
- Avoiding deprecated primitives (Roots, Sampling, Logging, HTTP+SSE)

## What is included

- [`SKILL.md`](./SKILL.md) — the agent procedure, the primitive/transport/SDK mental model, and the current-vs-deprecated decision criteria.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible verbatim mirror of the Model Context Protocol documentation pages the seed index links to.

## Source

Reference material is reproduced from the
[Model Context Protocol documentation](https://modelcontextprotocol.io) via its
official [llms.txt index](https://modelcontextprotocol.io/llms.txt).
