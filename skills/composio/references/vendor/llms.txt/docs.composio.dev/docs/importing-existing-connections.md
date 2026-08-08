# Importing existing connections (/docs/importing-existing-connections)

If your users have already authenticated with a service and you have their credentials (API keys, bearer tokens, etc.), you can pass those directly into Composio. No re-authentication required.

This is useful when:

* Your app already stores API keys or tokens for users
* You're adopting Composio and want to onboard existing users without disrupting them
* You want to use bearer tokens with OAuth toolkits (Gmail, GitHub, Slack, etc.) without setting up an OAuth app

# How it works [#how-it-works]

# Prerequisites [#prerequisites]

1. **An [auth config](/docs/programmatic-auth-configs)** for the toolkit you're importing into
2. **The existing credentials** for each user (API keys, bearer tokens, username/password, etc.)
3. **A userID** for each user. Any string that uniquely identifies them in your system.

# API keys [#api-keys]

For services that use API key authentication (e.g., SendGrid, Tavily, PostHog):

**Python:**

```python
from composio import Composio
from composio.types import auth_scheme

composio = Composio(api_key="your-api-key")

connection = composio.connected_accounts.initiate(
    user_id="user_123",
    auth_config_id="ac_your_auth_config",
    config=auth_scheme.api_key({
        "api_key": "sg-existing-sendgrid-key",
    }),
)

# API key connections are immediately active
print(f"Connected: {connection.id}")
```

**TypeScript:**

```typescript
import { Composio, AuthScheme } from '@composio/core';

const composio = new Composio({ apiKey: 'your-api-key' });

const connection = await composio.connectedAccounts.initiate(
  'user_123',
  'ac_your_auth_config',
  {
    config: AuthScheme.APIKey({
      api_key: 'sg-existing-sendgrid-key',
    }),
  }
);

// API key connections are immediately active
console.log('Connected:', connection.id);
```

# Bearer tokens [#bearer-tokens]

If you manage your own OAuth flow and already have an access token for a service, you can import it into Composio as a bearer token. This lets you bring existing OAuth connections into Composio without re-authenticating your users. It works with **all toolkits that support OAuth2 or S2S auth** (Gmail, GitHub, Slack, Google Docs, and more). Any additional parameters the toolkit supports (e.g., `subdomain`, `base_url`) work the same way.

Since you're providing your own token, Composio won't handle OAuth refresh. You're responsible for refreshing the token on your end and pushing the updated value to Composio via the [PATCH method](#updating-credentials) whenever it changes.

After [creating an auth config](/docs/programmatic-auth-configs) with `authScheme: "BEARER_TOKEN"`, use the snippet below to create a connected account:

**Python:**

```python
from composio import Composio
from composio.types import auth_scheme

composio = Composio(api_key="your-api-key")

connection = composio.connected_accounts.initiate(
    user_id="user_123",
    auth_config_id="ac_your_auth_config",
    config=auth_scheme.bearer_token({
        "token": "existing-bearer-token",
    }),
)

# Bearer token connections are immediately active
print(f"Connected: {connection.id}")
```

**TypeScript:**

```typescript
import { Composio, AuthScheme } from '@composio/core';

const composio = new Composio({ apiKey: 'your-api-key' });

const connection = await composio.connectedAccounts.initiate(
  'user_123',
  'ac_your_auth_config',
  {
    config: AuthScheme.BearerToken({
      token: 'existing-bearer-token',
    }),
  }
);

// Bearer token connections are immediately active
console.log('Connected:', connection.id);
```

# Basic auth [#basic-auth]

**Python:**

```python
from composio import Composio
from composio.types import auth_scheme

composio = Composio(api_key="your-api-key")

connection = composio.connected_accounts.initiate(
    user_id="user_123",
    auth_config_id="ac_your_auth_config",
    config=auth_scheme.basic({
        "username": "user@example.com",
        "password": "existing-password",
    }),
)

# Basic auth connections are immediately active
print(f"Connected: {connection.id}")
```

**TypeScript:**

```typescript
import { Composio, AuthScheme } from '@composio/core';

const composio = new Composio({ apiKey: 'your-api-key' });

const connection = await composio.connectedAccounts.initiate(
  'user_123',
  'ac_your_auth_config',
  {
    config: AuthScheme.Basic({
      username: 'user@example.com',
      password: 'existing-password',
    }),
  }
);

// Basic auth connections are immediately active
console.log('Connected:', connection.id);
```

# Updating credentials [#updating-credentials]

When credentials expire or rotate, update them in place without recreating the connection. Fields you omit are preserved. Fields set to `null` are removed.

**Bearer token:**

```python
composio.connected_accounts.update(
    "ca_your_connection_id",
    connection={
        "state": {
            "authScheme": "BEARER_TOKEN",
            "val": {"token": "new-access-token"},
        },
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your-api-key' });
await composio.connectedAccounts.update('ca_your_connection_id', {
  connection: {
    state: {
      authScheme: 'BEARER_TOKEN',
      val: { token: 'new-access-token' },
    },
  },
});
```

**curl:**

```bash
curl -X PATCH https://backend.composio.dev/api/v3.1/connected_accounts/ca_xxx \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"connection":{"state":{"authScheme":"BEARER_TOKEN","val":{"token":"new-access-token"}}}}'
```

**API key:**

```python
composio.connected_accounts.update(
    "ca_your_connection_id",
    connection={
        "state": {
            "authScheme": "API_KEY",
            "val": {"generic_api_key": "new-api-key"},
        },
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your-api-key' });
await composio.connectedAccounts.update('ca_your_connection_id', {
  connection: {
    state: {
      authScheme: 'API_KEY',
      val: { generic_api_key: 'new-api-key' },
    },
  },
});
```

**curl:**

```bash
curl -X PATCH https://backend.composio.dev/api/v3.1/connected_accounts/ca_xxx \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"connection":{"state":{"authScheme":"API_KEY","val":{"generic_api_key":"new-api-key"}}}}'
```

**Basic auth:**

```python
composio.connected_accounts.update(
    "ca_your_connection_id",
    connection={
        "state": {
            "authScheme": "BASIC",
            "val": {"username": "user@example.com", "password": "new-password"},
        },
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your-api-key' });
await composio.connectedAccounts.update('ca_your_connection_id', {
  connection: {
    state: {
      authScheme: 'BASIC',
      val: { username: 'user@example.com', password: 'new-password' },
    },
  },
});
```

**curl:**

```bash
curl -X PATCH https://backend.composio.dev/api/v3.1/connected_accounts/ca_xxx \
  -H 'x-api-key: YOUR_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"connection":{"state":{"authScheme":"BASIC","val":{"username":"user@example.com","password":"new-password"}}}}'
```

# Using in your session [#using-in-your-session]

Pass the auth config or connection ID when creating a session:

**Python:**

```python
session = composio.create(
    "user_123",
    auth_configs={"gmail": "ac_your_auth_config"},
    toolkits=["gmail"],
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your-api-key' });
const session = await composio.create('user_123', {
  authConfigs: { gmail: 'ac_your_auth_config' },
  toolkits: ['gmail'],
});
```

# Next [#next]

- [Managing multiple accounts](/docs/managing-multiple-connected-accounts): Pin and select connected accounts for a user

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

