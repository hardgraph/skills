# Tools (/reference/sdk-reference/python/tools)

# Methods [#methods]

## get\_raw\_composio\_tool\_by\_slug() [#get_raw_composio_tool_by_slug]

Returns schema for the given tool slug.

```python
def get_raw_composio_tool_by_slug(slug: str) -> Tool
```

**Parameters**

| Name   | Type  |
| ------ | ----- |
| `slug` | `str` |

**Returns**

`Tool`

***

## get\_raw\_composio\_tools() [#get_raw_composio_tools]

Get a list of tool schemas based on the provided filters.

```python
def get_raw_composio_tools(tools: list[str | None] = ..., search: str | None = ..., toolkits: list[str | None] = ..., scopes: List[str | None] = ..., limit: int | None = ...) -> list[Tool]
```

**Parameters**

| Name        | Type                |
| ----------- | ------------------- |
| `tools?`    | `list[str \| None]` |
| `search?`   | `str \| None`       |
| `toolkits?` | `list[str \| None]` |
| `scopes?`   | `List[str \| None]` |
| `limit?`    | `int \| None`       |

**Returns**

`list[Tool]`

***

## get\_raw\_tool\_router\_meta\_tools() [#get_raw_tool_router_meta_tools]

Fetches the tools exposed by a tool router session.  This method fetches helper/meta tools and any preloaded app tools from the Composio API and transforms them to the expected format. It provides access to the underlying tool data without provider-specific wrapping.

```python
def get_raw_tool_router_meta_tools(session_id: str, modifiers: 'Modifiers' | None = ...) -> list[Tool]
```

**Parameters**

| Name         | Type                  |
| ------------ | --------------------- |
| `session_id` | `str`                 |
| `modifiers?` | `'Modifiers' \| None` |

**Returns**

`list[Tool]` — The list of meta tools

**Example**

```python
from composio import Composio

composio = Composio()
tools_model = composio.tools

# Get meta tools for a session
meta_tools = tools_model.get_raw_tool_router_meta_tools("session_123")
print(meta_tools)

# Get meta tools with schema modifiers
from composio.core.models import schema_modifier

@schema_modifier
def modify_schema(tool: str, toolkit: str, schema):
    # Customize the schema
    schema.description = f"Modified: {schema.description}"
    return schema

meta_tools = tools_model.get_raw_tool_router_meta_tools(
    "session_123",
    modifiers=[modify_schema]
)
```

***

## get() [#get]

Get a tool or list of tools based on the provided arguments.  The return type is automatically inferred based on the provider's generic parameters. For example: - OpenAIProvider -> list\[ChatCompletionToolParam] - AnthropicProvider -> list\[ToolParam] - CustomProvider\[MyTool, list\[MyTool]] -> list\[MyTool]

```python
def get(user_id: str, slug: str | None = ..., tools: list[str | None] = ..., search: str | None = ..., toolkits: list[str | None] = ..., scopes: List[str | None] = ..., modifiers: Modifiers | None = ..., limit: int | None = ...) -> TToolCollection
```

**Parameters**

| Name         | Type                |
| ------------ | ------------------- |
| `user_id`    | `str`               |
| `slug?`      | `str \| None`       |
| `tools?`     | `list[str \| None]` |
| `search?`    | `str \| None`       |
| `toolkits?`  | `list[str \| None]` |
| `scopes?`    | `List[str \| None]` |
| `modifiers?` | `Modifiers \| None` |
| `limit?`     | `int \| None`       |

**Returns**

`TToolCollection` — Provider-specific tool collection (TToolCollection).

***

## execute() [#execute]

Execute a tool with the provided parameters.  This method calls the Composio API to execute the tool and returns the response.

```python
def execute(slug: str, arguments: Dict, connected_account_id: str | None = ..., custom_auth_params: tool_execute_params.CustomAuthParams | None = ..., custom_connection_data: tool_execute_params.CustomConnectionData | None = ..., user_id: str | None = ..., text: str | None = ..., version: str | None = ..., dangerously_skip_version_check: bool | None = ..., modifiers: Modifiers | None = ...) -> ToolExecutionResponse
```

**Parameters**

| Name                              | Type                                               |
| --------------------------------- | -------------------------------------------------- |
| `slug`                            | `str`                                              |
| `arguments`                       | `Dict`                                             |
| `connected_account_id?`           | `str \| None`                                      |
| `custom_auth_params?`             | `tool_execute_params.CustomAuthParams \| None`     |
| `custom_connection_data?`         | `tool_execute_params.CustomConnectionData \| None` |
| `user_id?`                        | `str \| None`                                      |
| `text?`                           | `str \| None`                                      |
| `version?`                        | `str \| None`                                      |
| `dangerously_skip_version_check?` | `bool \| None`                                     |
| `modifiers?`                      | `Modifiers \| None`                                |

**Returns**

`ToolExecutionResponse` — The response from the tool.

***

## proxy() [#proxy]

Proxy a tool call to the Composio API

```python
def proxy(endpoint: str, method: Literal['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'HEAD'], body: object | None = ..., connected_account_id: str | None = ..., parameters: List[tool_proxy_params.Parameter | None] = ..., custom_connection_data: tool_proxy_params.CustomConnectionData | None = ...) -> tool_proxy_response.ToolProxyResponse
```

**Parameters**

| Name                      | Type                                                       |
| ------------------------- | ---------------------------------------------------------- |
| `endpoint`                | `str`                                                      |
| `method`                  | `Literal['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'HEAD']` |
| `body?`                   | `object \| None`                                           |
| `connected_account_id?`   | `str \| None`                                              |
| `parameters?`             | `List[tool_proxy_params.Parameter \| None]`                |
| `custom_connection_data?` | `tool_proxy_params.CustomConnectionData \| None`           |

**Returns**

`tool_proxy_response.ToolProxyResponse`

***

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/tools.py#L89)

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

