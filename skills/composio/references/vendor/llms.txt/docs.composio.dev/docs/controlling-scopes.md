# Controlling scopes (/docs/controlling-scopes)

Scopes are the permissions an OAuth toolkit grants your app: read email, write to a repo, manage calendar events. Composio requests a sensible set of default scopes for each toolkit, so most apps never set scopes at all. Override them when the defaults grant too much or too little: to follow least privilege, or to reach an API the defaults don't cover.

You control scopes on an [auth config](/docs/authentication#behind-the-scenes), then [pass that auth config to a session](#use-the-auth-config-in-a-session) so the session requests your scopes when users connect.

> Scopes apply to OAuth toolkits. Toolkits that authenticate with API keys or bearer tokens don't have scopes to set.

# Set scopes with Composio managed auth [#set-scopes-with-composio-managed-auth]

Pass a `scopes` field in `credentials` to override the defaults while still using Composio's managed OAuth app. Give scopes as a comma-separated string.

**Python:**

```python
from composio import Composio

composio = Composio()

auth_config = composio.auth_configs.create(
    toolkit="hubspot",
    options={
        "type": "use_composio_managed_auth",
        "name": "HubSpot",
        "credentials": {"scopes": "sales-email-read,tickets"},
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const authConfig = await composio.authConfigs.create('hubspot', {
  type: 'use_composio_managed_auth',
  name: 'HubSpot',
  credentials: { scopes: 'sales-email-read,tickets' },
});
```

# Set scopes with your own OAuth app [#set-scopes-with-your-own-oauth-app]

When you bring your own OAuth credentials, put `scopes` alongside the client ID and secret. Make sure your OAuth app has those scopes approved in the provider's portal.

**Python:**

```python
import os

auth_config = composio.auth_configs.create(
    toolkit="github",
    options={
        "type": "use_custom_auth",
        "auth_scheme": "OAUTH2",
        "name": "GitHub",
        "credentials": {
            "client_id": os.environ["GITHUB_CLIENT_ID"],
            "client_secret": os.environ["GITHUB_CLIENT_SECRET"],
            "scopes": "repo,read:org",
        },
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const authConfig = await composio.authConfigs.create('github', {
  type: 'use_custom_auth',
  authScheme: 'OAUTH2',
  name: 'GitHub',
  credentials: {
    client_id: process.env.GITHUB_CLIENT_ID!,
    client_secret: process.env.GITHUB_CLIENT_SECRET!,
    scopes: 'repo,read:org',
  },
});
```

# Update scopes on an existing config [#update-scopes-on-an-existing-config]

Change the scopes on an auth config you already created without recreating it.

**Python:**

```python
composio.auth_configs.update(
    "ac_1234",
    {"type": "default", "scopes": "repo,read:org,read:user"},
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
await composio.authConfigs.update('ac_1234', {
  type: 'default',
  scopes: 'repo,read:org,read:user',
});
```

> Changing scopes affects new connections only. Users with an existing [connected account](/docs/authentication#behind-the-scenes) keep the scopes they already granted until they reconnect. To apply new scopes to a current user, have them re-authenticate.

# Use the auth config in a session [#use-the-auth-config-in-a-session]

Setting scopes on an auth config does nothing until a session uses it. Pass the auth config ID to `authConfigs` (keyed by toolkit) when you create the session, and the session requests your scopes when the user connects that toolkit.

**Python:**

```python
session = composio.create(
    user_id="user_123",
    auth_configs={"github": auth_config.id},
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const authConfig = { id: 'ac_your_github_config' };
const session = await composio.create('user_123', {
  authConfigs: { github: authConfig.id },
});
```

# Next [#next]

- [White-labeling authentication](/docs/white-labeling-authentication): Remove Composio branding from your auth flows

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

