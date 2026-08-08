# Migrating from Direct Tools to Sessions (/docs/migration-guide/direct-to-sessions)

> _Written February 2026._

This guide is for developers who are using Composio's **direct tool execution** pattern and want to migrate to **sessions**. For a comparison of the two patterns, see [Sessions vs Direct Execution](/docs/sessions-vs-direct-execution).

With sessions, you don't need to manage tool fetching, authentication, or execution yourself. You create a session and it handles everything. Your existing auth configs and connected accounts carry over, so your users don't need to re-authenticate.

If you're starting fresh, use [Configuring Sessions](/docs/configuring-sessions) instead.

# What changes [#what-changes]

|                      | Direct tools                                      | Sessions                                                               |
| -------------------- | ------------------------------------------------- | ---------------------------------------------------------------------- |
| **Tool discovery**   | You fetch specific tools by name/toolkit          | Agent discovers tools at runtime via meta tools                        |
| **Authentication**   | You manage connect links and wait for connections | In-chat auth prompts users automatically, or use `session.authorize()` |
| **Execution**        | You call `tools.execute()` with version pinning   | Agent executes tools through `COMPOSIO_MULTI_EXECUTE_TOOL`             |
| **Toolkit versions** | You manage versions manually                      | Handled automatically                                                  |

# Migrating [#migrating]

#### Pass your existing auth configs to the session

You already have auth configs (e.g. `ac_github_config`, `ac_slack_config`). Pass them when creating a session so it uses your existing OAuth credentials and your users' connected accounts carry over.

**Python:**

```python
from composio import Composio

composio = Composio()

# Before: manual tool fetching with user_id
# tools = composio.tools.get(user_id="user_123", toolkits=["GITHUB", "SLACK"])

# After: create a session with your existing auth configs
session = composio.create(
    user_id="user_123",
    auth_configs={
        "github": "ac_your_github_config",
        "slack": "ac_your_slack_config"
    }
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
// Before: manual tool fetching with userId
// const tools = await composio.tools.get("user_123", { toolkits: ["GITHUB", "SLACK"] });

// After: create a session with your existing auth configs
const session = await composio.create("user_123", {
  authConfigs: {
    github: "ac_your_github_config",
    slack: "ac_your_slack_config",
  },
});
```

Since the session uses the same `user_id` and auth configs, it automatically picks up existing connected accounts. No re-authentication needed.

#### Replace manual tool fetching with session tools

Instead of fetching specific tools by name, get the session's meta tools. These let the agent discover, authenticate, and execute any tool at runtime.

**Python:**

```python
# Before: manually specifying which tools to fetch
# tools = composio.tools.get(user_id="user_123", toolkits=["GITHUB"])

# After: session provides meta tools that handle discovery
tools = session.tools()
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");
// Before: manually specifying which tools to fetch
// const tools = await composio.tools.get("user_123", { toolkits: ["GITHUB"] });

// After: session provides meta tools that handle discovery
const tools = await session.tools();
```

#### Remove manual auth flows

If you were manually creating connect links and waiting for connections, sessions handle this automatically. When a tool requires authentication, the agent prompts the user with a connect link in-chat.

**Python:**

```python
# Before: manual auth flow
# connection_request = composio.connected_accounts.link(
#     user_id="user_123",
#     auth_config_id="ac_your_github_config",
#     callback_url="https://your-app.com/callback"
# )
# redirect_url = connection_request.redirect_url

# After: auth happens automatically in-chat
# Or if you need to pre-authenticate outside of chat:
connection_request = session.authorize("github")
print(connection_request.redirect_url)
connected_account = connection_request.wait_for_connection()
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");
// Before: manual auth flow
// const connectionRequest = await composio.connectedAccounts.link("user_123", "ac_your_github_config", {
//   callbackUrl: "https://your-app.com/callback"
// });
// const redirectUrl = connectionRequest.redirectUrl;

// After: auth happens automatically in-chat
// Or if you need to pre-authenticate outside of chat:
const connectionRequest = await session.authorize("github", {
  callbackUrl: "https://your-app.com/callback",
});
console.log(connectionRequest.redirectUrl);
const connectedAccount = await connectionRequest.waitForConnection();
```

#### Remove toolkit version management

If you were pinning toolkit versions in your code or environment variables, you can remove that. Sessions handle versioning automatically.

**Python:**

```python
# Before: manual version pinning
# composio = Composio(
#     toolkit_versions={
#         "github": "20251027_00",
#         "slack": "20251027_00",
#     }
# )

# After: no version management needed
composio = Composio()
session = composio.create(user_id="user_123")
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
// Before: manual version pinning
// const composio = new Composio({
//   toolkitVersions: {
//     github: "20251027_00",
//     slack: "20251027_00",
//   },
// });

// After: no version management needed
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");
```

# Restricting toolkits [#restricting-toolkits]

If you were fetching tools from specific toolkits, you can restrict the session to only those toolkits:

**Python:**

```python
session = composio.create(
    user_id="user_123",
    toolkits=["github", "gmail", "slack"],
    auth_configs={
        "github": "ac_your_github_config",
        "gmail": "ac_your_gmail_config",
        "slack": "ac_your_slack_config"
    }
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123", {
  toolkits: ["github", "gmail", "slack"],
  authConfigs: {
    github: "ac_your_github_config",
    gmail: "ac_your_gmail_config",
    slack: "ac_your_slack_config",
  },
});
```

# Multiple connected accounts [#multiple-connected-accounts]

If your users have multiple accounts for the same toolkit (e.g., work and personal Gmail), you can specify which one to use per session:

**Python:**

```python
session = composio.create(
    user_id="user_123",
    auth_configs={
        "gmail": "ac_your_gmail_config"
    },
    connected_accounts={
        "gmail": ["ca_work_gmail"]
    }
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123", {
  authConfigs: {
    gmail: "ac_your_gmail_config",
  },
  connectedAccounts: {
    gmail: ["ca_work_gmail"],
  },
});
```

If you don't specify, the most recently connected account is used. See [Managing multiple connected accounts](/docs/managing-multiple-connected-accounts) for details.

# Triggers [#triggers]

Triggers don't work with sessions yet. Continue using them the same way you do today with `composio.triggers.create()`, `composio.triggers.enable()`, and webhook subscriptions.

See [Triggers](/docs/triggers) for setup instructions.

# White labeling [#white-labeling]

If you've set up white-labeled OAuth screens with custom auth configs, those carry over automatically. Just pass the same auth config IDs to your session via `auth_configs` / `authConfigs`. Your users will continue to see your branding on consent screens.

See [White-labeling authentication](/docs/white-labeling-authentication) for more.

# Next [#next]

- [Configuring Sessions](/docs/configuring-sessions): Toolkits, auth configs, account selection, and session methods

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

