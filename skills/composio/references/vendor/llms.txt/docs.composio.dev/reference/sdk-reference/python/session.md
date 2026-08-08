# Session (/reference/sdk-reference/python/session)

# Properties [#properties]

| Name           | Type                              |
| -------------- | --------------------------------- |
| `session_id`   | `str`                             |
| `experimental` | `'ToolRouterSessionExperimental'` |
| `preload`      | `Any`                             |

# Methods [#methods]

## tools() [#tools]

Get provider-wrapped tools for execution with your AI framework.  Returns tools configured for this session, wrapped in the format expected by your AI provider (OpenAI, Anthropic, LangChain, etc.).  When custom tools are bound to the session, execution of COMPOSIO\_MULTI\_EXECUTE\_TOOL is intercepted: local tools are executed in-process, remote tools are sent to the backend.

```python
def tools(modifiers: 'Modifiers' | None = ...) -> TToolCollection
```

**Parameters**

| Name         | Type                  |
| ------------ | --------------------- |
| `modifiers?` | `'Modifiers' \| None` |

**Returns**

`TToolCollection`

***

## authorize() [#authorize]

Authorize a toolkit for the user and get a connection request.  Initiates the OAuth flow and returns a ConnectionRequest with redirect URL.

```python
def authorize(toolkit: str, callback_url: str | None = ..., alias: str | None = ..., experimental: session_link_params.Experimental | None = ...) -> ConnectionRequest
```

**Parameters**

| Name            | Type                                       |
| --------------- | ------------------------------------------ |
| `toolkit`       | `str`                                      |
| `callback_url?` | `str \| None`                              |
| `alias?`        | `str \| None`                              |
| `experimental?` | `session_link_params.Experimental \| None` |

**Returns**

`ConnectionRequest`

***

## toolkits() [#toolkits]

Get toolkit connection states for the session.

```python
def toolkits(toolkits: List[str | None] = ..., next_cursor: str | None = ..., limit: int | None = ..., is_connected: bool | None = ..., search: str | None = ...) -> ToolkitConnectionsDetails
```

**Parameters**

| Name            | Type                |
| --------------- | ------------------- |
| `toolkits?`     | `List[str \| None]` |
| `next_cursor?`  | `str \| None`       |
| `limit?`        | `int \| None`       |
| `is_connected?` | `bool \| None`      |
| `search?`       | `str \| None`       |

**Returns**

`ToolkitConnectionsDetails`

***

## search() [#search]

Search for tools by semantic use case.  Returns relevant tools for the given query with schemas and guidance.

```python
def search(query: str, model: str | None = ...) -> SessionSearchResponse
```

**Parameters**

| Name     | Type          |
| -------- | ------------- |
| `query`  | `str`         |
| `model?` | `str \| None` |

**Returns**

`SessionSearchResponse`

***

## execute() [#execute]

Execute a tool within the session.  For custom tools, accepts the original slug (e.g. "GREP") or the full slug (e.g. "LOCAL\_GREP"). Custom tools are executed in-process; remote tools are sent to the Composio backend.

```python
def execute(tool_slug: str, arguments: Dict[str, Any | None] = ..., account: str | None = ...) -> SessionExecuteResponse
```

**Parameters**

| Name         | Type                     |
| ------------ | ------------------------ |
| `tool_slug`  | `str`                    |
| `arguments?` | `Dict[str, Any \| None]` |
| `account?`   | `str \| None`            |

**Returns**

`SessionExecuteResponse`

***

## custom\_tools() [#custom_tools]

List all custom tools registered in this session.  Returns tools with their final slugs, schemas, and resolved toolkit.

```python
def custom_tools(toolkit: str | None = ...) -> List[RegisteredCustomTool]
```

**Parameters**

| Name       | Type          |
| ---------- | ------------- |
| `toolkit?` | `str \| None` |

**Returns**

`List[RegisteredCustomTool]` — Array of registered custom tools

***

## custom\_toolkits() [#custom_toolkits]

List all custom toolkits registered in this session.  Returns toolkits with their tools showing final slugs.

```python
def custom_toolkits() -> List[RegisteredCustomToolkit]
```

**Returns**

`List[RegisteredCustomToolkit]`

***

## proxy\_execute() [#proxy_execute]

Proxy an API call through Composio's auth layer.

```python
def proxy_execute(toolkit: str, endpoint: str, method: Literal['GET', 'POST', 'PUT', 'DELETE', 'PATCH'], body: Any = ..., parameters: List[Dict[str, Any | None]] = ...) -> SessionProxyExecuteResponse
```

**Parameters**

| Name          | Type                                               |
| ------------- | -------------------------------------------------- |
| `toolkit`     | `str`                                              |
| `endpoint`    | `str`                                              |
| `method`      | `Literal['GET', 'POST', 'PUT', 'DELETE', 'PATCH']` |
| `body?`       | `Any`                                              |
| `parameters?` | `List[Dict[str, Any \| None]]`                     |

**Returns**

`SessionProxyExecuteResponse` — Proxied API response

***

## update() [#update]

Partially update the session configuration.  Only the fields provided will be changed; omitted fields are preserved. Mutates this session's `preload` in-place.  Pass `None` for `manage_connections`, `sandbox`/`workbench`, or `multi_account` to clear the stored value.  `workbench` is a backwards-compatible alias for `sandbox`. Prefer `sandbox` in new code.  All parameters use the same types as the Stainless-generated `client.tool_router.session.patch()` method.

```python
def update(toolkits: Union[session_patch_params.Toolkits, 'Omit'] = ..., tools: Union[Dict[str, session_patch_params.Tools], 'Omit'] = ..., tags: Union[session_patch_params.Tags, 'Omit'] = ..., auth_configs: Union[Dict[str, str], 'Omit'] = ..., connected_accounts: Union[Dict[str, SequenceNotStr[str | None]], 'Omit'] = ..., manage_connections: Union[session_patch_params.ManageConnections | None, 'Omit'] = ..., sandbox: Union[session_patch_params.Workbench | None, 'Omit'] = ..., workbench: Union[session_patch_params.Workbench | None, 'Omit'] = ..., multi_account: Union[session_patch_params.MultiAccount | None, 'Omit'] = ..., preload: Union[session_patch_params.Preload, 'Omit'] = ...) -> None
```

**Parameters**

| Name                  | Type                                                            |
| --------------------- | --------------------------------------------------------------- |
| `toolkits?`           | `Union[session_patch_params.Toolkits, 'Omit']`                  |
| `tools?`              | `Union[Dict[str, session_patch_params.Tools], 'Omit']`          |
| `tags?`               | `Union[session_patch_params.Tags, 'Omit']`                      |
| `auth_configs?`       | `Union[Dict[str, str], 'Omit']`                                 |
| `connected_accounts?` | `Union[Dict[str, SequenceNotStr[str \| None]], 'Omit']`         |
| `manage_connections?` | `Union[session_patch_params.ManageConnections \| None, 'Omit']` |
| `sandbox?`            | `Union[session_patch_params.Workbench \| None, 'Omit']`         |
| `workbench?`          | `Union[session_patch_params.Workbench \| None, 'Omit']`         |
| `multi_account?`      | `Union[session_patch_params.MultiAccount \| None, 'Omit']`      |
| `preload?`            | `Union[session_patch_params.Preload, 'Omit']`                   |

***

## delete() [#delete]

Delete this session.  Deleted sessions immediately stop being retrievable or executable. An already-deleted session surfaces the backend 404.

```python
def delete() -> ToolRouterSessionDeleteResponse
```

**Returns**

`ToolRouterSessionDeleteResponse`

***

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/tool_router_session.py#L85)

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

