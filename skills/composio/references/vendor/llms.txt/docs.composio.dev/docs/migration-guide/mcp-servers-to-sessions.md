# Migrating from MCP servers to Sessions (/docs/migration-guide/mcp-servers-to-sessions)

> _Written June 2026._

This guide is for developers using Composio's **MCP servers** (`composio.mcp.create` /
`composio.mcp.generate`, the "Single Toolkit MCP" flow) who want to migrate to **sessions**.

Sessions are the next generation of the same idea: you still get an MCP URL that any MCP-compatible
client connects to — but instead of standing up and managing a separate server config per toolkit,
you create a session that handles tool discovery, authentication, context, and versioning for you.

> Starting fresh? Skip this guide and read [Configuring Sessions](/docs/configuring-sessions).

> Just connecting apps to your own agent for personal use — not building an app? You don't need the
SDK. Use **Composio For You** to connect across 1000+ apps in a few clicks — switch to it from the
product switcher in the top-left of the dashboard. Reserve `session.mcp.url` for programmatic,
in-app use.

# What carries over (you keep all of this) [#what-carries-over-you-keep-all-of-this]

* **Your tools** — every tool you exposed on a server is available in a session.
* **Your auth configs and connected accounts** — pass the same `ac_…` IDs; your users **do not
re-authenticate**.
* **The MCP URL pattern** — you still get a URL (`session.mcp.url`) that plugs into your agent — any
MCP-compatible client — the same way, the same protocol.
* **Per-user isolation** — still keyed by `user_id`.
* **Tool restriction** — you can still pin a session to an exact, fixed tool list (see Step 3).

# What changes [#what-changes]

|                       | MCP servers (today)                                                 | Sessions                                                                              |
| --------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Setup**             | Create + manage a server config per toolkit (`composio.mcp.create`) | One `composio.create(user_id)` — no server object to manage                           |
| **Get a URL**         | `composio.mcp.generate(user_id, mcp_config_id)` → `instance.url`    | `session.mcp.url`                                                                     |
| **Tools**             | Fixed `allowed_tools` list, baked into the server                   | Dynamic discovery by default, **or** a fixed list (direct-tools preset) — your choice |
| **Multiple toolkits** | One server per toolkit                                              | One session spans many toolkits                                                       |
| **Context**           | All allowed tools always loaded                                     | Managed — search/preload keep the agent's context lean                                |
| **Versioning**        | Manual                                                              | Handled automatically                                                                 |
| **Auth**              | Pre-authenticate, then generate                                     | Carries over; in-chat auth available, or `session.authorize()`                        |

The *why*: a session is one managed endpoint that replaces N static server configs, gives the agent
runtime tool discovery, and keeps context small — without losing the fixed-tool-list behavior you
have today if that's what you want.

# Migrating [#migrating]

#### Replace the server config + generate with a session

The two-step "create a server, then generate a per-user URL" collapses into a single
`composio.create(...)`. Pass the **same** toolkit, auth config, and tool list you had on the server.

**Python:**

```python
from composio import Composio

composio = Composio(api_key="YOUR_API_KEY")

# Before: create a server config, then generate a per-user URL
# server = composio.mcp.create(
#     name="my-gmail-server",
#     toolkits=[{"toolkit": "gmail", "auth_config": "ac_xyz123"}],
#     allowed_tools=["GMAIL_FETCH_EMAILS", "GMAIL_SEND_EMAIL"],
# )
# instance = composio.mcp.generate(user_id="user-123", mcp_config_id=server.id)
# mcp_url = instance["url"]

# After: one session, same toolkit + auth config + tools
session = composio.create(
    user_id="user-123",
    toolkits=["gmail"],
    auth_configs={"gmail": "ac_xyz123"},
    tools={"gmail": {"enable": ["GMAIL_FETCH_EMAILS", "GMAIL_SEND_EMAIL"]}},
    mcp=True,
)
mcp_url = session.mcp.url
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });

// Before: create a server config, then generate a per-user URL
// const server = await composio.mcp.create("my-gmail-server", {
//   toolkits: [{ toolkit: "gmail", authConfigId: "ac_xyz123" }],
//   allowedTools: ["GMAIL_FETCH_EMAILS", "GMAIL_SEND_EMAIL"],
// });
// const instance = await composio.mcp.generate("user-123", server.id);
// const mcpUrl = instance.url;

// After: one session, same toolkit + auth config + tools
const session = await composio.create("user-123", {
  toolkits: ["gmail"],
  authConfigs: { gmail: "ac_xyz123" },
  tools: { gmail: { enable: ["GMAIL_FETCH_EMAILS", "GMAIL_SEND_EMAIL"] } },
  mcp: true,
});
const { mcp } = session;
const mcpUrl = mcp.url;
```

Same `user_id` + same auth config → existing connected accounts are picked up automatically. No
re-auth.

#### Point your MCP client at the new URL

Swap the generated server URL for `session.mcp.url` wherever your agent connects. Nothing else about
the client changes.

```python
# Before: https://backend.composio.dev/v3/mcp/?user_id=
# After:  session.mcp.url
```

#### Keep an exact, fixed tool list (optional — closest 1:1 with your server)

By default a session gives the agent **dynamic** tool discovery (meta-tools). If you want the *same
static behavior* as your old server — a fixed set of tools, no discovery — add the **direct-tools
preset**. This preloads exactly the tools you enable and turns meta-tools off.

**Python:**

```python
from composio import Composio, SESSION_PRESET_DIRECT_TOOLS

session = composio.create(
    user_id="user-123",
    toolkits=["gmail"],
    auth_configs={"gmail": "ac_xyz123"},
    tools={"gmail": {"enable": ["GMAIL_FETCH_EMAILS", "GMAIL_SEND_EMAIL"]}},
    session_preset=SESSION_PRESET_DIRECT_TOOLS,
)
```

**TypeScript:**

```typescript
import { Composio, SessionPreset } from '@composio/core';

const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });
const session = await composio.create("user-123", {
  toolkits: ["gmail"],
  authConfigs: { gmail: "ac_xyz123" },
  tools: { gmail: { enable: ["GMAIL_FETCH_EMAILS", "GMAIL_SEND_EMAIL"] } },
  sessionPreset: SessionPreset.DIRECT_TOOLS,
});
```

Leave the preset off to get the upgrade: the agent discovers and loads tools at runtime, so you can
span many toolkits without bloating context.

# Beyond the MCP URL [#beyond-the-mcp-url]

A session isn't only an MCP endpoint — it's also a normal SDK object, which is handy for non-agent
code paths.

## Native tools for your framework [#native-tools-for-your-framework]

`session.tools()` returns provider-wrapped native tools for OpenAI, Anthropic, LangChain, the Vercel
AI SDK, and others, so you skip manual schema wiring. With the direct-tools preset it returns your
exact fixed tool set; by default it returns the meta-tools that drive runtime discovery.

**Python:**

```python
tools = session.tools()
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });
const session = await composio.create("user-123");
const tools = await session.tools();
```

## Execute a tool without an LLM [#execute-a-tool-without-an-llm]

For a deterministic, non-agent path, call a tool directly on the session.

**Python:**

```python
result = session.execute(
    "GITHUB_CREATE_ISSUE",
    arguments={"owner": "my-org", "repo": "my-repo", "title": "Fix login bug"},
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });
const session = await composio.create("user-123");
const result = await session.execute("GITHUB_CREATE_ISSUE", {
  owner: "my-org",
  repo: "my-repo",
  title: "Fix login bug",
});
```

# Things to know [#things-to-know]

* **Multiple toolkits on one endpoint** — where you ran several single-toolkit servers, one session
spans them all (`toolkits=["gmail", "slack", "github"]`). Fewer moving parts.
* **Reuse one session — don't create one per request** — sessions persist and are reusable. Create a
session once and keep using it across calls; see [Configuring Sessions](/docs/configuring-sessions).
* **Sharing one account across users** — sessions support **shared connections**
(`account_type:"SHARED"` + an allow/deny ACL), pinned per session. See
[Shared connections](/docs/shared-connections).
* **Tenant-specific params** (SharePoint sub-site, Jira subdomain) — prefill them via shared
credentials on the auth config.
* **White-labeling carries over** — pass the same white-labeled auth config IDs; users keep seeing
your branding on consent screens. See [White-labeling authentication](/docs/white-labeling-authentication).
* **Triggers** — unchanged; continue using `composio.triggers.*` and webhooks (triggers aren't part
of sessions yet).
* **Dashboard** — sessions are created via the SDK. For no-code, personal app connections, use
**Composio For You** — reachable from the product switcher in the top-left of the dashboard.

# Next [#next]

- [Configuring Sessions](/docs/configuring-sessions): Toolkits, auth configs, account selection, presets, and session methods

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

