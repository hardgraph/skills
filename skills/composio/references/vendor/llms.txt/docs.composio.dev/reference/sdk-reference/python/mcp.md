# MCP (/reference/sdk-reference/python/mcp)

> **Deprecated**: Use a session's MCP endpoint instead: `composio.create(user_id, mcp=True)` returns a session that exposes `session.mcp.url` / `session.mcp.headers`. MCP is now opt-in per session; this standalone `composio.mcp` server-management API is kept only for backwards compatibility. Prefer the session MCP endpoint; do not generate new code against `composio.mcp`. See [https://docs.composio.dev/docs/sessions-via-mcp](https://docs.composio.dev/docs/sessions-via-mcp)

# Methods [#methods]

## create() [#create]

Create a new MCP server configuration with specified toolkits and authentication settings.

```python
def create(name: str, toolkits: List[Union[ConfigToolkit, str]], manually_manage_connections: bool = ..., allowed_tools: List[str | None] = ...) -> MCPCreateResponse
```

**Parameters**

| Name                           | Type                              |
| ------------------------------ | --------------------------------- |
| `name`                         | `str`                             |
| `toolkits`                     | `List[Union[ConfigToolkit, str]]` |
| `manually_manage_connections?` | `bool`                            |
| `allowed_tools?`               | `List[str \| None]`               |

**Returns**

`MCPCreateResponse` — Created server details with generate method

**Example**

```python
>>> # Using toolkit configuration objects with auth
>>> server = composio.experimental.mcp.create(
...     'personal-mcp-server',
...     toolkits=[
...         {
...             'toolkit': 'github',
...             'auth_config_id': 'ac_xyz',
...         },
...         {
...             'toolkit': 'slack',
...             'auth_config_id': 'ac_abc',
...         },
...     ],
...     allowed_tools=['GITHUB_CREATE_ISSUE', 'GITHUB_LIST_REPOS', 'SLACK_SEND_MESSAGE'],
...     manually_manage_connections=False
... )
>>>
>>> # Using simple toolkit names (most common usage)
>>> server = composio.experimental.mcp.create(
...     'simple-mcp-server',
...     toolkits=['composio_search', 'text_to_pdf'],
...     allowed_tools=['COMPOSIO_SEARCH_DUCK_DUCK_GO_SEARCH', 'TEXT_TO_PDF_CONVERT_TEXT_TO_PDF']
... )
>>>
>>> # Using all tools from toolkits (default behavior)
>>> server = composio.experimental.mcp.create(
...     'all-tools-server',
...     toolkits=['composio_search', 'text_to_pdf']
...     # allowed_tools=None means all tools from these toolkits
... )
>>>
>>> # Get server instance for a user
>>> mcp = server.generate('user_12345')
```

***

## list() [#list]

List MCP servers with optional filtering and pagination.

```python
def list(page_no: int | None = ..., limit: int | None = ..., toolkits: str | None = ..., auth_config_ids: str | None = ..., name: str | None = ..., order_by: Literal['created_at', 'updated_at' | None] = ..., order_direction: Literal['asc', 'desc' | None] = ...) -> MCPListResponse
```

**Parameters**

| Name               | Type                                          |
| ------------------ | --------------------------------------------- |
| `page_no?`         | `int \| None`                                 |
| `limit?`           | `int \| None`                                 |
| `toolkits?`        | `str \| None`                                 |
| `auth_config_ids?` | `str \| None`                                 |
| `name?`            | `str \| None`                                 |
| `order_by?`        | `Literal['created_at', 'updated_at' \| None]` |
| `order_direction?` | `Literal['asc', 'desc' \| None]`              |

**Returns**

`MCPListResponse` — Paginated list of MCP servers

**Example**

```python
>>> # List all servers
>>> all_servers = composio.experimental.mcp.list()
>>>
>>> # List with pagination
>>> paged_servers = composio.experimental.mcp.list(page_no=2, limit=5)
>>>
>>> # Filter by toolkit
>>> github_servers = composio.experimental.mcp.list(toolkits='github', name='personal')
```

***

## get() [#get]

Retrieve detailed information about a specific MCP server/config.

```python
def get(server_id: str)
```

**Parameters**

| Name        | Type  |
| ----------- | ----- |
| `server_id` | `str` |

**Example**

```python
>>> server = composio.experimental.mcp.get('mcp_12345')
>>>
>>> print(server['name'])  # "My Personal MCP Server"
>>> print(server['allowed_tools'])  # ["GITHUB_CREATE_ISSUE", "SLACK_SEND_MESSAGE"]
>>> print(server['toolkits'])  # ["github", "slack"]
>>> print(server['server_instance_count'])  # 3
```

***

## update() [#update]

Update an existing MCP server configuration.

```python
def update(server_id: str, name: str | None = ..., toolkits: List[Union[ConfigToolkit, str | None]] = ..., manually_manage_connections: bool | None = ..., allowed_tools: List[str | None] = ...)
```

**Parameters**

| Name                           | Type                                      |
| ------------------------------ | ----------------------------------------- |
| `server_id`                    | `str`                                     |
| `name?`                        | `str \| None`                             |
| `toolkits?`                    | `List[Union[ConfigToolkit, str \| None]]` |
| `manually_manage_connections?` | `bool \| None`                            |
| `allowed_tools?`               | `List[str \| None]`                       |

**Example**

```python
>>> # Update server name only
>>> updated_server = composio.experimental.mcp.update(
...     'mcp_12345',
...     name='My Updated MCP Server'
... )
>>>
>>> # Update toolkits and tools
>>> server_with_new_tools = composio.experimental.mcp.update(
...     'mcp_12345',
...     toolkits=['github', 'slack'],
...     allowed_tools=['GITHUB_CREATE_ISSUE', 'SLACK_SEND_MESSAGE']
... )
>>>
>>> # Update with auth configs
>>> server_with_auth = composio.experimental.mcp.update(
...     'mcp_12345',
...     toolkits=[
...         {'toolkit': 'github', 'auth_config_id': 'auth_abc123'},
...         {'toolkit': 'slack', 'auth_config_id': 'auth_def456'}
...     ],
...     allowed_tools=['GITHUB_CREATE_ISSUE', 'SLACK_SEND_MESSAGE'],
...     manually_manage_connections=False
... )
```

***

## delete() [#delete]

Permanently delete an MCP server configuration.

```python
def delete(server_id: str) -> Dict[str, Any]
```

**Parameters**

| Name        | Type  |
| ----------- | ----- |
| `server_id` | `str` |

**Returns**

`Dict[str, Any]` — Deletion result

**Example**

```python
>>> # Delete a server
>>> result = composio.experimental.mcp.delete('mcp_12345')
>>>
>>> if result['deleted']:
...     print(f"Server {result['id']} has been successfully deleted")
>>> else:
...     print(f"Failed to delete server {result['id']}")
```

***

## generate() [#generate]

Get server URLs for an existing MCP server.  This matches the TypeScript implementation exactly.

```python
def generate(user_id: str, mcp_config_id: str, manually_manage_connections: bool | None = ...) -> MCPServerInstance
```

**Parameters**

| Name                           | Type           |
| ------------------------------ | -------------- |
| `user_id`                      | `str`          |
| `mcp_config_id`                | `str`          |
| `manually_manage_connections?` | `bool \| None` |

**Returns**

`MCPServerInstance` — MCP server instance

**Example**

```python
>>> mcp = composio.experimental.mcp.generate(
...     'user_12345',
...     'mcp_67890',
...     manually_manage_connections=False
... )
>>>
>>> print(mcp['url'])  # Server URL for the user
>>> print(mcp['allowed_tools'])  # Available tools
```

***

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/mcp.py#L104)

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

