# ConnectedAccounts (/reference/sdk-reference/python/connected-accounts)

# Methods [#methods]

## update() [#update]

Update a connected account's alias and/or credentials.

```python
def update(nanoid: str, alias: str | None = ..., connection: connected_account_patch_params.Connection | None = ...) -> connected_account_patch_response.ConnectedAccountPatchRes...
```

**Parameters**

| Name          | Type                                                |
| ------------- | --------------------------------------------------- |
| `nanoid`      | `str`                                               |
| `alias?`      | `str \| None`                                       |
| `connection?` | `connected_account_patch_params.Connection \| None` |

**Returns**

`connected_account_patch_response.ConnectedAccountPatchRes...` — Response with `id`, `status`, and `success`.

**Example**

```python
# Set an alias
composio.connected_accounts.update('ca_abc123', alias='work-gmail')

# Clear an alias
composio.connected_accounts.update('ca_abc123', alias='')
```

***

## update\_acl() [#update_acl]

Update the per-user ACL on a SHARED connected account. Experimental — shape may change in future releases.  Only valid on SHARED connections; raises `ComposioAclOnlyForSharedError` on a PRIVATE connection. Omit a parameter to leave it unchanged; pass an empty list to clear an allow/deny list. At least one parameter must be provided.

```python
def update_acl(nanoid: str, allow_all_users: bool | None = ..., allowed_user_ids: List[str | None] = ..., not_allowed_user_ids: List[str | None] = ...) -> connected_account_patch_response.ConnectedAccountPatchRes...
```

**Parameters**

| Name                    | Type                |
| ----------------------- | ------------------- |
| `nanoid`                | `str`               |
| `allow_all_users?`      | `bool \| None`      |
| `allowed_user_ids?`     | `List[str \| None]` |
| `not_allowed_user_ids?` | `List[str \| None]` |

**Returns**

`connected_account_patch_response.ConnectedAccountPatchRes...` — Response with `id`, `status`, and `success`.

**Example**

```python
composio.connected_accounts.update_acl(
    'ca_abc',
    allow_all_users=True,
    not_allowed_user_ids=['user_bob'],
)
```

***

## initiate() [#initiate]

Compound function to create a new connected account. This function creates a new connected account and returns a connection request.  Users can then wait for the connection to be established using the `wait_for_connection` method.

```python
def initiate(user_id: str, auth_config_id: str, callback_url: str | None = ..., allow_multiple: bool = ..., config: connected_account_create_params.ConnectionState | None = ..., alias: str | None = ...) -> ConnectionRequest
```

**Parameters**

| Name              | Type                                                      |
| ----------------- | --------------------------------------------------------- |
| `user_id`         | `str`                                                     |
| `auth_config_id`  | `str`                                                     |
| `callback_url?`   | `str \| None`                                             |
| `allow_multiple?` | `bool`                                                    |
| `config?`         | `connected_account_create_params.ConnectionState \| None` |
| `alias?`          | `str \| None`                                             |

**Returns**

`ConnectionRequest` — The connection request.

***

## link() [#link]

Create a Composio Connect Link for a user to connect their account to a given auth config.  This method will return an external link which you can use for the user to connect their account.

```python
def link(user_id: str, auth_config_id: str, callback_url: str | None = ..., alias: str | None = ..., allow_multiple: bool = ..., experimental: link_create_params.Experimental | None = ...) -> ConnectionRequest
```

**Parameters**

| Name              | Type                                      |
| ----------------- | ----------------------------------------- |
| `user_id`         | `str`                                     |
| `auth_config_id`  | `str`                                     |
| `callback_url?`   | `str \| None`                             |
| `alias?`          | `str \| None`                             |
| `allow_multiple?` | `bool`                                    |
| `experimental?`   | `link_create_params.Experimental \| None` |

**Returns**

`ConnectionRequest` — Connection request object.

**Example**

```python
# Create a connection request and redirect the user to the redirect url
connection_request = composio.connected_accounts.link('user_123', 'auth_config_123')
redirect_url = connection_request.redirect_url
print(f"Visit: {redirect_url} to authenticate your account")

# Wait for the connection to be established
connected_account = connection_request.wait_for_connection()

# Create a connection request with callback URL
connection_request = composio.connected_accounts.link(
    'user_123',
    'auth_config_123',
    callback_url='https://your-app.com/callback'
)
redirect_url = connection_request.redirect_url
print(f"Visit: {redirect_url} to authenticate your account")

# Wait for the connection to be established
connected_account = composio.connected_accounts.wait_for_connection(connection_request.id)

connection_request = composio.connected_accounts.link(
    'user_creator',
    'auth_config_123',
    experimental={
        'account_type': 'SHARED',
        'acl_config_for_shared': {
            'allow_all_users': True,
            'not_allowed_user_ids': ['user_bob'],
        },
    },
)
```

***

## wait\_for\_connection() [#wait_for_connection]

Wait for connected account with given ID to be active

```python
def wait_for_connection(id: str, timeout: float | None = ...) -> connected_account_retrieve_response.ConnectedAccountRetri...
```

**Parameters**

| Name       | Type            |
| ---------- | --------------- |
| `id`       | `str`           |
| `timeout?` | `float \| None` |

**Returns**

`connected_account_retrieve_response.ConnectedAccountRetri...`

***

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/connected_accounts.py#L340)

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

