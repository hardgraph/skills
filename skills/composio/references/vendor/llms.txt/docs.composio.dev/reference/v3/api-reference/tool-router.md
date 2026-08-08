# Sessions (prev Tool Router) (/reference/v3/api-reference/tool-router)

> **API version:** This page documents Composio REST API v3.0 at `https://backend.composio.dev/api/v3`, the previous version. v3.1 is current, at `https://backend.composio.dev/api/v3.1` — the same page on v3.1 is /reference/api-reference/tool-router.md.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/tool-router.mdx, not this file. */}

These are Composio's session endpoints. A **session** is the runtime context your agent uses to work for one of your users: it scopes which user's connected accounts are in play, which tools are available, how authentication happens, and where execution state lives. Read [What is a session?](/docs/how-composio-works) for the full concept.

> Sessions were formerly called the "tool router", which is why these endpoints live under `tool_router`. They are the same thing.

In the SDK you do not call these endpoints directly. Use `composio.create(...)` to start a session and `composio.use(...)` to resume one, then call `session.tools()`, `session.execute(...)`, and `session.authorize(...)` on the returned object.

Reach for the raw API when you need lower-level control: creating and patching a session config, attaching to an existing session, searching for tools, executing tools and meta tools, opening link sessions for auth, proxying authenticated requests, and reading or writing files in a session mount.

See [Configuring sessions](/docs/configuring-sessions) for toolkits, auth configs, account selection, and presets.

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `POST` | `/api/v3/tool_router/session` | [Create a new tool router session](/reference/v3/api-reference/tool-router/postToolRouterSession) |
| `POST` | `/api/v3/tool_router/session/{session_id}/execute` | [Execute a tool within a tool router session](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdExecute) |
| `POST` | `/api/v3/tool_router/session/{session_id}/execute_meta` | [Execute a meta tool within a tool router session](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdExecuteMeta) |
| `GET` | `/api/v3/tool_router/session/{session_id}` | [Get a tool router session by ID](/reference/v3/api-reference/tool-router/getToolRouterSessionBySessionId) |
| `PATCH` | `/api/v3/tool_router/session/{session_id}` | [Patch a tool router session config](/reference/v3/api-reference/tool-router/patchToolRouterSessionBySessionId) |
| `POST` | `/api/v3/tool_router/session/{session_id}/link` | [Create a link session for a toolkit in a tool router session](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdLink) |
| `POST` | `/api/v3/tool_router/session/{session_id}/proxy_execute` | [Execute proxy request within a tool router session](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdProxyExecute) |
| `GET` | `/api/v3/tool_router/session/{session_id}/toolkits` | [Get toolkits for a tool router session](/reference/v3/api-reference/tool-router/getToolRouterSessionBySessionIdToolkits) |
| `GET` | `/api/v3/tool_router/session/{session_id}/tools` | [List tools with schemas for a tool router session](/reference/v3/api-reference/tool-router/getToolRouterSessionBySessionIdTools) |
| `POST` | `/api/v3/tool_router/session/{session_id}/search` | [Search for tools using a query](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdSearch) |
| `GET` | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/items` | [List files in a session mount](/reference/v3/api-reference/tool-router/getToolRouterSessionBySessionIdMountsByMountIdItems) |
| `POST` | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/download_url` | [Create a presigned download URL for a mount file](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdMountsByMountIdDownloadUrl) |
| `POST` | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/upload_url` | [Create a presigned upload URL for a mount file](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdMountsByMountIdUploadUrl) |
| `POST` | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/delete` | [Delete a file from a session mount](/reference/v3/api-reference/tool-router/postToolRouterSessionBySessionIdMountsByMountIdDelete) |

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

