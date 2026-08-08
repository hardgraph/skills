# Using sessions via MCP (/docs/sessions-via-mcp)

By default Composio gives your agent **tools it can call directly**. Composio formats them as function schemas for your framework through a [provider package](/docs/providers). That's what the [Quickstart](/docs/quickstart) uses, and it's the right choice for most agents.

If you'd rather connect over the [Model Context Protocol](https://modelcontextprotocol.io), every session also exposes a hosted MCP endpoint. This is useful when you want to plug a session into an MCP-native client like Claude Desktop, Cursor, or any framework's MCP transport. No provider package required.

> Want to bring tools from your own remote MCP server into Composio instead? See [Custom MCP](/docs/extending-sessions/custom-mcp).

# The MCP endpoint [#the-mcp-endpoint]

Opt into MCP by passing `mcp: true` when you create the session. The session then exposes its hosted MCP server. Read the URL and headers off `session.mcp`:

**Python:**

```python
from composio import Composio

composio = Composio()
session = composio.sessions.create(user_id="user_123", mcp=True)

mcp_url = session.mcp.url
mcp_headers = session.mcp.headers
```

**TypeScript:**

```typescript
import { Composio } from "@composio/core";

const composio = new Composio();
const session = await composio.create("user_123", { mcp: true });

const mcpUrl = session.mcp.url;
const mcpHeaders = session.mcp.headers;
```

You don't need a provider package to use the MCP endpoint, so you can drop it from your `Composio()` setup if MCP is all you need.

> Resuming a stored session? Pass the same flag, `composio.use(sessionId, { mcp: true })` (TypeScript) or `composio.use(session_id, mcp=True)` (Python), to surface `session.mcp` on the reused session.

> The MCP endpoint and `session.tools()` are backed by the same session. Toolkits, auth configs, and connected accounts you set when [configuring the session](/docs/configuring-sessions) apply to both.

# A single URL for a fixed set of tools [#a-single-url-for-a-fixed-set-of-tools]

Combine `mcp: true` with the [direct-tools preset](/docs/configuring-sessions) to get one MCP URL that serves exactly the tools you list, with no search or meta tools in front of them. This is the closest equivalent to a classic hosted MCP server scoped to a handful of tools.

**Python:**

```python
from composio import Composio, SESSION_PRESET_DIRECT_TOOLS

composio = Composio()

session = composio.sessions.create(
    user_id="user_123",
    toolkits=["gmail"],
    tools={"gmail": {"enable": ["GMAIL_FETCH_EMAILS", "GMAIL_CREATE_EMAIL_DRAFT"]}},
    session_preset=SESSION_PRESET_DIRECT_TOOLS,
    mcp=True,
)

# A single MCP URL that exposes just these two tools
print(session.mcp.url)
```

**TypeScript:**

```typescript
import { Composio, SessionPreset } from "@composio/core";

const composio = new Composio();

const session = await composio.create("user_123", {
  toolkits: ["gmail"],
  tools: { gmail: { enable: ["GMAIL_FETCH_EMAILS", "GMAIL_CREATE_EMAIL_DRAFT"] } },
  sessionPreset: SessionPreset.DIRECT_TOOLS,
  mcp: true,
});

// A single MCP URL that exposes just these two tools
console.log(session.mcp.url);
```

Any MCP client pointed at that URL sees only `GMAIL_FETCH_EMAILS` and `GMAIL_CREATE_EMAIL_DRAFT`. See [Configuring Sessions](/docs/configuring-sessions) for the full set of toolkit, tool, and auth filters.

# Wire it into your framework [#wire-it-into-your-framework]

Pass `session.mcp.url` and `session.mcp.headers` to your framework's MCP client.

**OpenAI Agents (Python):**

```python
from agents import Agent, HostedMCPTool

agent = Agent(
    name="Assistant",
    tools=[
        HostedMCPTool(
            tool_config={
                "type": "mcp",
                "server_label": "composio",
                "server_url": session.mcp.url,
                "headers": session.mcp.headers,
                "require_approval": "never",
            }
        )
    ],
)
```

**Claude Agent SDK (Python):**

```python
from claude_agent_sdk import ClaudeAgentOptions

options = ClaudeAgentOptions(
    mcp_servers={
        "composio": {
            "type": "http",
            "url": session.mcp.url,
            "headers": session.mcp.headers,
        }
    },
)
```

**Vercel AI SDK (TypeScript):**

```typescript
import { Composio } from "@composio/core";
import { createMCPClient } from "@ai-sdk/mcp";

const composio = new Composio();
const { mcp } = await composio.create("user_123", { mcp: true });

const client = await createMCPClient({
  transport: {
    type: "http",
    url: mcp.url,
    headers: mcp.headers,
  },
});
const tools = await client.tools();
```

# Trade-offs [#trade-offs]

MCP is the more portable option. Any MCP-compatible client connects with just a URL, and it's supported across more frameworks and apps (Claude Desktop, Cursor, the OpenAI Responses API, and others) without a provider package.

The trade-off is that the MCP client talks to Composio's server directly, so anything the SDK does *around* tool execution doesn't apply:

* **Tool-call modifiers don't run.** `beforeExecute` / `afterExecute` hooks and `modifySchema` transforms live in the SDK's execution path. Over MCP the client executes tools against the server and bypasses them, so you can't intercept, reshape, log, or gate calls the way you can with tools your agent calls directly.
* **Session-bound custom tools and toolkits don't work.** Tools created with `experimental_createTool` / `experimental_createToolkit` in TypeScript, or `composio.experimental.tool` / `composio.experimental.Toolkit` in Python, run in your process. The MCP server only exposes Composio's hosted tools, so your local custom tools aren't available over the endpoint.

If you need any of those, call tools directly through a [provider](/docs/providers) instead of over MCP.

# Next [#next]

- [Configuring Sessions](/docs/configuring-sessions): Restrict toolkits, set custom auth configs, and select connected accounts

---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)

---

# Composio SDK — Notes for AI Code Generators

**Purpose:** Reference for generating current (v3) [Composio](https://composio.dev/) integration code.
**Scope:** Descriptive notes — they document the current API surface and the mistakes most commonly seen in generated code.

---

## 1. Recommended Integration: Sessions

Composio supports two integration modes: **Native Tools** (with a provider package) and **MCP** (no provider package needed).

### Native Tools

```python
# ✅ CORRECT — Python (defaults to OpenAI)
from composio import Composio

composio = Composio()
session = composio.create(user_id="user_123")
tools = session.tools()
# Pass tools to your agent/LLM framework
```

```typescript
// ✅ CORRECT — TypeScript (defaults to OpenAI)
import { Composio } from "@composio/core";

const composio = new Composio();
const session = await composio.create("user_123");
const tools = await session.tools();
// Pass tools to your agent/LLM framework
```

For other providers, pass the provider explicitly. Provider packages follow the naming convention: `composio_<provider>` for Python, `@composio/<provider>` for TypeScript.

### MCP

Use `session.mcp.url` and `session.mcp.headers` with any MCP-compatible client (Claude Desktop, Cursor, OpenAI Agents, etc.). No provider package needed.

---

## 2. Integration Notes

- `composio.create(user_id)` is the standard entry point for agent integrations. Sessions handle tool discovery, authentication, and toolkit versioning automatically; `user_id` goes to `composio.create()` — individual tool calls in session mode don't take one.
- Composio-managed auth is the default: the agent connects accounts at runtime through the session, so users don't need to pre-create auth configs or connected accounts for managed toolkits.
- Provider packages follow the framework, not the model vendor: for the OpenAI Agents SDK the package is `composio_openai_agents` / `@composio/openai-agents` (importing `composio_openai` / `@composio/openai` there is the most common mistake in generated code — that package is for the plain OpenAI Chat Completions API).
- **Direct execution** (`composio.tools.get()`, `composio.tools.execute()`, `provider.handle_tool_calls()`) is a fully supported lower-level interface: your code picks the tool, no runtime discovery. It fits deterministic workflows and scripts; sessions fit agents that decide at runtime. The tradeoffs are documented at https://docs.composio.dev/docs/sessions-vs-direct-execution. Note that direct execution requires a toolkit version (https://docs.composio.dev/docs/tools-direct/toolkit-versioning).

---

# 3. Calling the REST API directly

## REST API version

The current REST API version is **v3.1**, served at `https://backend.composio.dev/api/v3.1`. Prefer it for new code and new examples.

`https://backend.composio.dev/api/v3` is the previous version. It is frozen with pinned tool-version defaults and remains supported — existing v3 integrations keep working and do not need to migrate.

## Tool-endpoint version defaults on v3.1

On v3.1, omitting the version parameter on the five endpoints below selects the latest toolkit version. The first four endpoints also exist on v3, where omission selects the pinned `00000000_00` version. `POST /tools/scopes/required` is v3.1-only.

| Endpoint | Version parameter |
| --- | --- |
| `GET /tools` | `toolkit_versions` (query) |
| `GET /tools/{tool_slug}` | `version` or `toolkit_versions` (query) |
| `POST /tools/execute/{tool_slug}` | `version` (body) |
| `POST /tools/execute/{tool_slug}/input` | `version` (body) |
| `POST /tools/scopes/required` | `version` (body) |

A v3.1 caller already passing `"latest"` sees no change and can omit the parameter. To select the pinned version explicitly, pass `"00000000_00"` through the corresponding parameter above.

This version-default change is limited to the five endpoints above.


---

## Terminology Migration (old → current)

If you encounter these terms in error messages, old documentation, or user prompts, translate them to the current equivalents. **Do not use the old terms in generated code or explanations.**

| Old term (v1/v2) | Current term (v3) | In code |
|---|---|---|
| entity ID | user ID | `user_id` parameter |
| actions | tools | e.g., `GITHUB_CREATE_ISSUE` is a *tool* |
| apps / appType | toolkits | e.g., `github` is a *toolkit* |
| integration / integration ID | auth config / auth config ID | `auth_config_id` parameter |
| connection | connected account | `connected_accounts` namespace |
| ComposioToolSet / OpenAIToolSet | `Composio` class with a provider | `Composio(provider=...)` |
| toolset | provider | e.g., `OpenAIProvider` |

If a user says "entity ID", they mean `user_id`. If they say "integration", they mean "auth config". Always respond using the current terminology.

