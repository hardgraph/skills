# Managing multiple connected accounts (/docs/managing-multiple-connected-accounts)

Users can connect multiple accounts for the same toolkit (e.g., personal and work Gmail accounts). This guide covers how to enable multi-account mode, label accounts with aliases, and select which account to use.

# Multi-account mode [#multi-account-mode]

By default, each session uses **one account per toolkit**. Enable multi-account mode to let users connect and use multiple accounts for the same toolkit within a single session.

**Python:**

```python
session = composio.create(
    user_id="user_123",
    toolkits=["gmail"],
    multi_account={
        "enable": True,
        "max_accounts_per_toolkit": 3,
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123", {
  toolkits: ["gmail"],
  multiAccount: {
    enable: true,
    maxAccountsPerToolkit: 3,
  },
});
```

## Configuration options [#configuration-options]

| Option (TS / Python)                                      | Type      | Default | Description                                                                                                                                                                                                                                                                                                                                                          |
| --------------------------------------------------------- | --------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `enable`                                                  | `boolean` | `false` | Enable multi-account mode for this session                                                                                                                                                                                                                                                                                                                           |
| `maxAccountsPerToolkit` / `max_accounts_per_toolkit`      | `number`  | `5`     | Maximum connected accounts per toolkit (2-10)                                                                                                                                                                                                                                                                                                                        |
| `requireExplicitSelection` / `require_explicit_selection` | `boolean` | `false` | When true and a toolkit has multiple active connected accounts, the agent must provide the `account` parameter in the tool execution call to select which account to use. `account` can be either a connected account ID or an alias (see the Aliases section below). When false, the default account (most recently connected active account) is used automatically |

When multi-account mode is disabled (the default), each session uses the most recently connected account for each toolkit.

# Connecting multiple accounts [#connecting-multiple-accounts]

Call `session.authorize()` multiple times for the same toolkit. Each call creates a separate connected account.

**Python:**

```python
session = composio.create(user_id="user_123", multi_account={"enable": True})

# Connect work account
work_auth = session.authorize("gmail", alias="work-gmail")
print(f"Connect work Gmail: {work_auth.redirect_url}")
work_connection = work_auth.wait_for_connection()

# Connect personal account
personal_auth = session.authorize("gmail", alias="personal-gmail")
print(f"Connect personal Gmail: {personal_auth.redirect_url}")
personal_connection = personal_auth.wait_for_connection()
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123", {
  toolkits: ["gmail"],
  multiAccount: { enable: true },
});

// Connect work account
const workAuth = await session.authorize("gmail", { alias: "work-gmail" });
console.log(`Connect work Gmail: ${workAuth.redirectUrl}`);
const workConnection = await workAuth.waitForConnection();

// Connect personal account
const personalAuth = await session.authorize("gmail", { alias: "personal-gmail" });
console.log(`Connect personal Gmail: ${personalAuth.redirectUrl}`);
const personalConnection = await personalAuth.waitForConnection();
```

# Aliases [#aliases]

Aliases are human-readable labels for connected accounts (e.g., `"work-gmail"`, `"personal-github"`). They make it easier for agents and users to identify which account is which.

* Must be unique per user and toolkit within a project
* Can be set during connection or updated after

## Setting an alias during connection [#setting-an-alias-during-connection]

Pass `alias` to `session.authorize()`:

**Python:**

```python
session = composio.create(user_id="user_123")

connection_request = session.authorize("gmail", alias="work-gmail")
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");
const connectionRequest = await session.authorize("gmail", { alias: "work-gmail" });
```

The direct-execution methods `connectedAccounts.initiate()` and `connectedAccounts.link()` accept the same `alias` parameter.

## Updating or clearing an alias [#updating-or-clearing-an-alias]

**Python:**

```python
# Set or update an alias
composio.connected_accounts.update("ca_abc123", alias="work-gmail")

# Clear an alias
composio.connected_accounts.update("ca_abc123", alias="")
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
// Set or update an alias
await composio.connectedAccounts.update("ca_abc123", { alias: "work-gmail" });

// Clear an alias
await composio.connectedAccounts.update("ca_abc123", { alias: "" });
```

# Selecting a specific account for a session [#selecting-a-specific-account-for-a-session]

Pin a session to specific accounts by passing their IDs in the session config. To retrieve connected account IDs, see [List accounts](/docs/auth-configuration/connected-accounts#list-accounts).

**Python:**

```python
session = composio.create(
    user_id="user_123",
    connected_accounts={
        "gmail": ["ca_work_gmail_id"],
        "github": ["ca_personal_github_id"],
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123", {
  connectedAccounts: {
    gmail: ["ca_work_gmail_id"],
    github: ["ca_personal_github_id"],
  },
});
```

# Viewing session's active accounts [#viewing-sessions-active-accounts]

Use `session.toolkits()` to see which accounts are currently active:

**Python:**

```python
toolkits = session.toolkits()

for toolkit in toolkits.items:
    if toolkit.connection and toolkit.connection.connected_account:
        print(f"{toolkit.name}: {toolkit.connection.connected_account.id}")
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");
const toolkits = await session.toolkits();

for (const toolkit of toolkits.items) {
  if (toolkit.connection?.connectedAccount) {
    console.log(`${toolkit.name}: ${toolkit.connection.connectedAccount.id}`);
  }
}
```

# Next [#next]

- [Shared connections](/docs/shared-connections): Make one connected account usable by multiple users with a per-user access control list

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

