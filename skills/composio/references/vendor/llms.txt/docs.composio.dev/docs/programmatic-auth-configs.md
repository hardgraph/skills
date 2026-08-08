# Programmatic auth configs (/docs/programmatic-auth-configs)

An [auth config](/docs/authentication#behind-the-scenes) is a blueprint for how a toolkit authenticates: the method, scopes, and credentials. Most of the time you create one in the [dashboard](https://dashboard.composio.dev/~/project/auth-configs?utm_source=docs\&utm_medium=content\&utm_campaign=docs-programmatic-auth-configs) and reuse it. Create them in code when you provision auth dynamically: a config per customer, per environment, or spun up and torn down as part of your app's lifecycle.

`composio.authConfigs.create()` returns an auth config ID like `ac_xxxxxxxx`. Store that ID, then [pass it to a session](#use-the-auth-config-in-a-session) so the session authenticates with it.

# Composio managed auth [#composio-managed-auth]

For OAuth2 toolkits, Composio maintains a managed app so you can create an auth config without bringing your own credentials. This is the fastest way to start.

**Python:**

```python
from composio import Composio

composio = Composio()

auth_config = composio.auth_configs.create(
    toolkit="github",
    options={"type": "use_composio_managed_auth", "name": "GitHub"},
)

print(auth_config.id)  # ac_xxxxxxxx
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const authConfig = await composio.authConfigs.create('github', {
  type: 'use_composio_managed_auth',
  name: 'GitHub',
});

console.log(authConfig.id); // ac_xxxxxxxx
```

# Your own OAuth2 credentials [#your-own-oauth2-credentials]

Bring your own OAuth app to show your branding on consent screens, request custom scopes, or get a dedicated rate-limit quota. Register the app in the provider's developer portal, set its authorized redirect URI to Composio's callback, then pass the client ID and secret.

```
https://backend.composio.dev/api/v3.1/toolkits/auth/callback
```

**Python:**

```python
import os
from composio import Composio

composio = Composio()

auth_config = composio.auth_configs.create(
    toolkit="notion",
    options={
        "type": "use_custom_auth",
        "auth_scheme": "OAUTH2",
        "name": "Notion",
        "credentials": {
            "client_id": os.environ["NOTION_CLIENT_ID"],
            "client_secret": os.environ["NOTION_CLIENT_SECRET"],
            "oauth_redirect_uri": "https://backend.composio.dev/api/v3.1/toolkits/auth/callback",
        },
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const authConfig = await composio.authConfigs.create('notion', {
  type: 'use_custom_auth',
  authScheme: 'OAUTH2',
  name: 'Notion',
  credentials: {
    client_id: process.env.NOTION_CLIENT_ID!,
    client_secret: process.env.NOTION_CLIENT_SECRET!,
    oauth_redirect_uri: 'https://backend.composio.dev/api/v3.1/toolkits/auth/callback',
  },
});
```

> Omit `oauth_redirect_uri` to use Composio's default callback. Set it only when you [route the callback through your own domain](/docs/white-labeling-authentication#routing-the-callback-through-your-domain).

# Other auth types [#other-auth-types]

Toolkits that use API keys, bearer tokens, basic auth, or no auth follow the same call. Set `auth_scheme` to the toolkit's scheme and put the required fields in `credentials`. For a toolkit whose key the user supplies at connect time, pass empty `credentials`.

**Python:**

```python
auth_config = composio.auth_configs.create(
    toolkit="perplexityai",
    options={
        "type": "use_custom_auth",
        "auth_scheme": "API_KEY",
        "name": "Perplexity AI",
        "credentials": {},
    },
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const authConfig = await composio.authConfigs.create('perplexityai', {
  type: 'use_custom_auth',
  authScheme: 'API_KEY',
  name: 'Perplexity AI',
  credentials: {},
});
```

# Use the auth config in a session [#use-the-auth-config-in-a-session]

Creating an auth config does not change which credentials a session uses. Pass the auth config ID to `authConfigs` (keyed by toolkit) when you create the session, and the session authenticates that toolkit with your config. Toolkits you leave out keep using Composio managed auth.

**Python:**

```python
session = composio.sessions.create(
    user_id="user_123",
    auth_configs={"notion": auth_config.id},
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const authConfig = { id: 'ac_your_notion_config' };
const session = await composio.create('user_123', {
  authConfigs: { notion: authConfig.id },
});
```

See [Configuring sessions](/docs/configuring-sessions#custom-auth-configs) for how `authConfigs` interacts with account selection and precedence.

# Find auth configs [#find-auth-configs]

In TypeScript, filter `authConfigs.list()` by name or ID with `search`. Disabled configs are excluded by default; set `showDisabled` to include them. Auth config responses use `id` as their canonical identifier.

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const configs = await composio.authConfigs.list({
  search: 'github',
  showDisabled: true,
});

for (const config of configs.items) {
  console.log(config.id);
}
```

# Discover the required fields [#discover-the-required-fields]

Different schemes need different credential fields. To build the `credentials` object dynamically, ask the toolkit which fields it requires for a given scheme before you create the config.

**Python:**

```python
fields = composio.toolkits.get_auth_config_creation_fields(
    toolkit="notion",
    auth_scheme="OAUTH2",
    required_only=True,
)
print(fields)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const fields = await composio.toolkits.getAuthConfigCreationFields('notion', 'OAUTH2', {
  requiredOnly: true,
});

console.log(fields);
```

# Next [#next]

- [Controlling scopes](/docs/controlling-scopes): Override the default OAuth scopes Composio requests for a toolkit

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

