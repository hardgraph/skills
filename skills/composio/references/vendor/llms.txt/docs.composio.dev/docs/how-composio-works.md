# What is a session? (/docs/how-composio-works)

A **session** is the runtime context for an agentic run: the scoped environment an AI agent works in while it acts for one of your users. You create it with `composio.create(userId)`, and it ties together the user, the toolkits available, authentication, and connected accounts. By default it gives the agent meta tools to discover, authenticate, and execute app tools at runtime, instead of loading hundreds of tool definitions into context.

# The basics [#the-basics]

Create a session for a user, then read its tools formatted for your framework. To connect over MCP instead, see [Using sessions via MCP](/docs/sessions-via-mcp).

**Python:**

```python
session = composio.sessions.create(user_id="user_123")

tools = session.tools()
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");

const tools = await session.tools();
```

A session scopes four things:

* **userID**: whose connected accounts and tool executions are in scope.
* **Tool access**: all toolkits by default, or a filtered set of toolkits, tools, or tags.
* **Authentication**: managed auth, custom auth configs, and connected-account selection.
* **Execution state**: logs, tool memory, MCP state, and workbench files for the task.

# Tools and toolkits [#tools-and-toolkits]

A **toolkit** is a collection of related tools for a service. The `github` toolkit, for example, contains tools for creating issues, managing pull requests, and starring repositories.

A **tool** is an individual action your agent can execute. Each tool has an input schema (its parameters) and an output schema (what it returns), and follows a `{TOOLKIT}_{ACTION}` naming pattern, like `GITHUB_CREATE_ISSUE`.

Every toolkit in the catalog is discoverable by default. Create a session without a `toolkits` parameter and the agent can find any of them at runtime. To restrict the set, pass `toolkits` when you create the session. See [Enable and disable toolkits](/docs/configuring-sessions). You can also bind local, in-process tools to a session with the experimental [custom tools and toolkits](/docs/extending-sessions/custom-tools-and-toolkits) API.

# Users [#users]

A user is an identifier from your app. Composio stores connections under that ID, so tools run with the right account and stay isolated from other users. Use a stable identifier like your database ID, never one that can change.

**userID best practices**

    * **Recommended:** database UUID or primary key (`user.id`)
    * **Acceptable:** unique username (`user.username`)
    * **Avoid:** email addresses (they can change)
    * **Never:** `default` in production (it exposes other users' data)

A user can connect multiple accounts for the same toolkit, like work and personal Gmail. Use the same userID, then select the connected account when a session needs a specific one. See [Managing multiple connected accounts](/docs/managing-multiple-connected-accounts).

# Meta tools [#meta-tools]

A session gives your agent meta tools, a small fixed set that discover, authenticate, and execute tools at runtime, so you never load hundreds of tool definitions into context:

The agent searches for relevant tools, authenticates if needed, and executes them through the same session. Meta-tool calls share context, so the agent searches in one call and executes in the next without losing state. See the [Meta Tools reference](/toolkits/meta-tools) for each tool's input and output schema.

Know the exact tools upfront? The [direct tools preset](/docs/configuring-sessions#direct-tools-preset) returns them directly from `session.tools()` with no search step, while keeping session auth, connected accounts, and the workbench.

# Authentication [#authentication]

When a tool needs a connection, the session generates a Connect Link with `session.authorize()`, or the agent handles the flow through `COMPOSIO_MANAGE_CONNECTIONS`.

In chat, the agent can pause, ask the user to connect an app, then retry the tool once auth completes. Composio manages the OAuth redirects, token exchange, and refresh. Once a user connects a toolkit, the connected account persists and future sessions reuse it without re-authentication.

For OAuth toolkits, Composio uses [managed apps](/docs/custom-app-vs-managed-app) by default. Bring your own app when you need your own branding, scopes, or consent screen.

# Sandbox [#sandbox]

Handle large responses and bulk operations in the remote sandbox. Instead of stuffing long tool responses into the model context, the agent reads files, searches outputs, writes Python, transforms data, and calls Composio tools in bulk.

The sandbox is scoped to the session, so files, variables, helper functions, and intermediate results stay available while the agent works through a task.

# How sessions behave [#how-sessions-behave]

Every `create()` call returns a new session ID. Use it for a fresh task context.

Sessions persist on the server and don't expire. For multi-turn conversations, store the session ID and reuse it with `composio.use()` instead of calling `create()` again.

**Python:**

```python
session = composio.use("session_id")
tools = session.tools()
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.use("session_id");
const tools = await session.tools();
```

You can also update a session in place instead of creating a new one:

**Python:**

```python
session.update(
    toolkits=["gmail", "slack"],
    auth_configs={"gmail": "ac_new_config"},
    connected_accounts={"slack": ["ca_work_slack"]},
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.use("session_id");
await session.update({
  toolkits: ["gmail", "slack"],
  authConfigs: { gmail: "ac_new_config" },
  connectedAccounts: { slack: ["ca_work_slack"] },
});
```

Create a new session for a different user or a fundamentally different task setup. Reuse or update a session when the same conversation should keep its tool, auth, and workbench context.

# Next [#next]

- [Configuring Sessions](/docs/configuring-sessions): Enable toolkits, set auth configs, and select connected accounts

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

