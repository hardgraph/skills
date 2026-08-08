# Python SDK Reference (/reference/sdk-reference/python)

# Python SDK Reference [#python-sdk-reference]

Complete API reference for the `composio` Python package.

# Installation [#installation]

# Classes [#classes]

| Class                                                                     | Description                                                                         |
| ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [`Composio`](/reference/sdk-reference/python/composio)                    | Composio SDK for Python.  Generic parameters: TTool: The individual tool type re... |
| [`Tools`](/reference/sdk-reference/python/tools)                          | Tools class definition  This class is used to manage tools in the Composio SDK. ... |
| [`Toolkits`](/reference/sdk-reference/python/toolkits)                    | Toolkits are a collectiono of tools that can be used to perform various tasks. T... |
| [`Triggers`](/reference/sdk-reference/python/triggers)                    | Triggers (instance) class                                                           |
| [`ConnectedAccounts`](/reference/sdk-reference/python/connected-accounts) | Manage connected accounts.  This class is used to manage connected accounts in t... |
| [`AuthConfigs`](/reference/sdk-reference/python/auth-configs)             | Manage authentication configurations.                                               |
| [`MCP`](/reference/sdk-reference/python/mcp)                              | MCP (Model Control Protocol) class. Provides enhanced MCP server operations  Thi... |
| [`Session`](/reference/sdk-reference/python/session)                      | A Composio session — the object returned by `composio.create(...)` / \`\`composi... |

# Quick Start [#quick-start]

```python
from composio import Composio

composio = Composio(api_key="your-api-key")

# Get tools for a user
tools = composio.tools.get("user-123", toolkits=["github"])

# Execute a tool
result = composio.tools.execute(
    "GITHUB_GET_REPOS",
    arguments={"owner": "composio"},
    user_id="user-123"
)
```

# Decorators [#decorators]

## before\_execute [#before_execute]

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/_modifiers.py#L280)

```python
@before_execute(modifier: BeforeExecute | None = ..., tools: List[str | None] = ..., toolkits: List[str | None] = ...)
def my_modifier(...):
    ...
```

## after\_execute [#after_execute]

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/_modifiers.py#L241)

```python
@after_execute(modifier: AfterExecute | None = ..., tools: List[str | None] = ..., toolkits: List[str | None] = ...)
def my_modifier(...):
    ...
```

## before\_file\_upload [#before_file_upload]

Build a `Modifier` for the file-upload hook (same scoping pattern as :func:`before_execute`).  Your callable may take **either**:  - a single `context` argument (:class:`BeforeFileUploadContext`) — the preferred form, exposes `context["source"]` (`"path"` or `"url"`), or - three positional arguments `(path, tool, toolkit)` — legacy form, kept for back-compat.  Return a new path/URL string to substitute, or `False` to abort the upload (raises :class:`~composio.exceptions.FileUploadAbortedError`).  Pass the returned `Modifier` in `modifiers=[...]` on :meth:`composio.core.models.tools.Tools.execute` or `tools.get`. Multiple such modifiers are composed in list order.

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/_modifiers.py#L319)

```python
@before_file_upload(modifier: BeforeFileUploadLike | None = ..., tools: List[str | None] = ..., toolkits: List[str | None] = ...)
def my_modifier(...):
    ...
```

## schema\_modifier [#schema_modifier]

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/_modifiers.py#L377)

```python
@schema_modifier(modifier: SchemaModifier | None = ..., tools: List[str | None] = ..., toolkits: List[str | None] = ...)
def my_modifier(...):
    ...
```

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

