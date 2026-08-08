# Scoped Project API Key (/reference/authenticating-to-composio/project-api-key-permissions)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

[Video: Scoped Project API Key walkthrough](https://youtube.com/watch?v=ySMu9lljkWg)

A scoped project API key lets you choose which project resources the key can access. Reach for one when a key needs only a subset of your project, such as executing tools, reading logs, or managing connected accounts.

> You pick a key's permissions when you create it, and they can't be changed afterward. To adjust them, create a new key and rotate your application to use it.

> Default project API keys keep full project API key access. Scoped keys use the permission areas and access levels on this page.

# Create a scoped API key [#create-a-scoped-api-key]

Create a scoped key from the dashboard:

Go to the [Composio Dashboard](https://dashboard.composio.dev).

Select **Platform**.

Select your project.

Go to **Settings**.

Open the **API Keys** tab.

Click **Create API Key**, then choose the permission areas and access levels below.

# Access levels [#access-levels]

| Access level   | What it allows                                                      |
| -------------- | ------------------------------------------------------------------- |
| No access      | The key cannot use routes in that permission area.                  |
| Read only      | The key can use read routes in that permission area.                |
| Write only     | The key can use write routes in that permission area.               |
| Read and write | The key can use both read and write routes in that permission area. |

Some read routes use `POST` because the request body carries filters or lookup input. The access level is based on what the route does, not only the HTTP method.

When v3 and v3.1 expose the same route shape, this page lists one representative version instead of repeating both. Version-specific routes are listed separately.

# Permission areas [#permission-areas]

Jump to each permission area to see the routes it covers.

| Permission area    | Available levels                                 | Routes                             |
| ------------------ | ------------------------------------------------ | ---------------------------------- |
| Auth configs       | No access, Read only, Write only, Read and write | [View routes](#auth-configs)       |
| Connected accounts | No access, Read only, Write only, Read and write | [View routes](#connected-accounts) |
| Tools              | No access, Read only                             | [View routes](#tools)              |
| Tool execution     | No access, Write only                            | [View routes](#tool-execution)     |
| Proxy execute      | No access, Write only                            | [View routes](#proxy-execute)      |
| Toolkits           | No access, Read only, Write only, Read and write | [View routes](#toolkits)           |
| Triggers           | No access, Read only, Write only, Read and write | [View routes](#triggers)           |
| Webhooks           | No access, Read only, Write only, Read and write | [View routes](#webhooks)           |
| Observability      | No access, Read only                             | [View routes](#observability)      |
| Sessions           | No access, Read only, Write only, Read and write | [View routes](#sessions)           |

# Auth configs [#auth-configs]

View and modify auth configs.

| Access | Method   | Endpoint                                 |
| ------ | -------- | ---------------------------------------- |
| Read   | `GET`    | `/api/v3/auth_configs`                   |
| Read   | `GET`    | `/api/v3/auth_configs/{nanoid}`          |
| Write  | `POST`   | `/api/v3/auth_configs`                   |
| Write  | `PATCH`  | `/api/v3/auth_configs/{nanoid}`          |
| Write  | `DELETE` | `/api/v3/auth_configs/{nanoid}`          |
| Write  | `PATCH`  | `/api/v3/auth_configs/{nanoid}/{status}` |

# Connected accounts [#connected-accounts]

View and manage connected accounts.

| Access | Method   | Endpoint                                       |
| ------ | -------- | ---------------------------------------------- |
| Read   | `GET`    | `/api/v3/connected_accounts`                   |
| Read   | `GET`    | `/api/v3/connected_accounts/{nanoid}`          |
| Write  | `POST`   | `/api/v3/connected_accounts`                   |
| Write  | `POST`   | `/api/v3/connected_accounts/link`              |
| Write  | `PATCH`  | `/api/v3/connected_accounts/{nanoid}`          |
| Write  | `PATCH`  | `/api/v3/connected_accounts/{nanoid}/status`   |
| Write  | `POST`   | `/api/v3/connected_accounts/{nanoid}/refresh`  |
| Write  | `DELETE` | `/api/v3/connected_accounts/{nanoid}`          |
| Write  | `POST`   | `/api/v3.1/connected_accounts/{nanoid}/revoke` |

# Tools [#tools]

View tool definitions, inputs, scopes, and versions.

| Access | Method | Endpoint                                       |
| ------ | ------ | ---------------------------------------------- |
| Read   | `GET`  | `/api/v3.1/tools`                              |
| Read   | `GET`  | `/api/v3.1/tools/enum`                         |
| Read   | `GET`  | `/api/v3.1/tools/{tool_slug}`                  |
| Read   | `GET`  | `/api/v3/tools/{tool_slug}/get_latest_version` |
| Read   | `GET`  | `/api/v3.1/tools/scopes/required`              |
| Read   | `GET`  | `/api/v3.1/tools/get_scopes_required`          |
| Read   | `POST` | `/api/v3.1/tools/execute/{tool_slug}/input`    |

# Tool execution [#tool-execution]

Execute predefined Composio tools.

| Access | Method | Endpoint                              |
| ------ | ------ | ------------------------------------- |
| Write  | `POST` | `/api/v3.1/tools/execute/{tool_slug}` |
| Write  | `POST` | `/api/v3/files/upload/request`        |
| Write  | `POST` | `/api/v3/files/upload/response`       |
| Write  | `GET`  | `/api/v3/files/list`                  |

# Proxy execute [#proxy-execute]

Execute raw proxy requests against connected accounts.

Proxy execute is separate from tool execution. Grant it only when your application needs to call a connected account API through the raw proxy path.

| Access | Method | Endpoint                                                 |
| ------ | ------ | -------------------------------------------------------- |
| Write  | `POST` | `/api/v3.1/tools/execute/proxy`                          |
| Write  | `POST` | `/api/v3/tool_router/session/{session_id}/proxy_execute` |

# Toolkits [#toolkits]

View and install toolkits.

| Access | Method | Endpoint                      |
| ------ | ------ | ----------------------------- |
| Read   | `GET`  | `/api/v3/toolkits`            |
| Read   | `GET`  | `/api/v3/toolkits/{slug}`     |
| Read   | `GET`  | `/api/v3/toolkits/categories` |
| Read   | `GET`  | `/api/v3/toolkits/changelog`  |
| Write  | `POST` | `/api/v3/toolkits/multi`      |

# Triggers [#triggers]

View trigger types, manage trigger instances, and subscribe to trigger events. The realtime routes are called by the SDK (`triggers.subscribe()`) and the CLI to receive trigger events.

| Access | Method   | Endpoint                                       |
| ------ | -------- | ---------------------------------------------- |
| Read   | `GET`    | `/api/v3/triggers_types`                       |
| Read   | `GET`    | `/api/v3/triggers_types/{slug}`                |
| Read   | `GET`    | `/api/v3/triggers_types/list/enum`             |
| Read   | `GET`    | `/api/v3/trigger_instances/active`             |
| Read   | `GET`    | `/api/v3/cli/realtime/credentials`             |
| Read   | `POST`   | `/api/v3/cli/realtime/auth`                    |
| Read   | `GET`    | `/api/v3/internal/sdk/realtime/credentials`    |
| Read   | `POST`   | `/api/v3/internal/sdk/realtime/auth`           |
| Write  | `POST`   | `/api/v3/trigger_instances/{slug}/upsert`      |
| Write  | `PATCH`  | `/api/v3/trigger_instances/manage/{triggerId}` |
| Write  | `DELETE` | `/api/v3/trigger_instances/manage/{triggerId}` |

# Webhooks [#webhooks]

View and manage webhook endpoints and subscriptions.

| Access | Method   | Endpoint                                           |
| ------ | -------- | -------------------------------------------------- |
| Read   | `GET`    | `/api/v3/webhook_endpoints`                        |
| Read   | `GET`    | `/api/v3/webhook_endpoints/{nano_id}`              |
| Read   | `GET`    | `/api/v3/webhook_endpoints/schema`                 |
| Read   | `GET`    | `/api/v3/webhook_subscriptions`                    |
| Read   | `GET`    | `/api/v3/webhook_subscriptions/{id}`               |
| Read   | `GET`    | `/api/v3/webhook_subscriptions/event_types`        |
| Write  | `POST`   | `/api/v3/webhook_endpoints`                        |
| Write  | `POST`   | `/api/v3/webhook_endpoints/{nano_id}`              |
| Write  | `PATCH`  | `/api/v3/webhook_endpoints/{nano_id}`              |
| Write  | `DELETE` | `/api/v3/webhook_endpoints/{nano_id}`              |
| Write  | `POST`   | `/api/v3/webhook_subscriptions`                    |
| Write  | `PATCH`  | `/api/v3/webhook_subscriptions/{id}`               |
| Write  | `DELETE` | `/api/v3/webhook_subscriptions/{id}`               |
| Write  | `POST`   | `/api/v3/webhook_subscriptions/{id}/rotate_secret` |

# Observability [#observability]

View execution logs and project usage summaries.

| Access | Method | Endpoint                                |
| ------ | ------ | --------------------------------------- |
| Read   | `POST` | `/api/v3.1/logs/tool_execution`         |
| Read   | `GET`  | `/api/v3.1/logs/tool_execution/{id}`    |
| Read   | `POST` | `/api/v3.1/project/usage/{entity_type}` |
| Read   | `POST` | `/api/v3.1/project/usage/summary`       |

# Sessions [#sessions]

Create and operate sessions and MCP servers. This permission area covers MCP server management, the MCP runtime transport, and the tool router MCP transport.

| Access | Method   | Endpoint                                                                  |
| ------ | -------- | ------------------------------------------------------------------------- |
| Read   | `GET`    | `/api/v3/mcp/servers`                                                     |
| Read   | `GET`    | `/api/v3/mcp/{id}`                                                        |
| Read   | `GET`    | `/api/v3/mcp/app/{app_key}`                                               |
| Read   | `GET`    | `/api/v3/mcp/servers/{server_id}/instances`                               |
| Read   | `GET`    | `/tool_router/{session_id}/mcp`                                           |
| Read   | `GET`    | `/api/v3.1/tool_router/session/{session_id}`                              |
| Read   | `GET`    | `/api/v3/tool_router/session/{session_id}/toolkits`                       |
| Read   | `GET`    | `/api/v3.1/tool_router/session/{session_id}/tools`                        |
| Read   | `GET`    | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/items`        |
| Read   | `GET`    | `/api/v3.1/tool_router/session/{session_id}/config_history`               |
| Write  | `POST`   | `/api/v3/mcp/servers`                                                     |
| Write  | `POST`   | `/api/v3/mcp/servers/generate`                                            |
| Write  | `POST`   | `/api/v3/mcp/servers/custom`                                              |
| Write  | `PATCH`  | `/api/v3/mcp/{id}`                                                        |
| Write  | `DELETE` | `/api/v3/mcp/{id}`                                                        |
| Write  | `POST`   | `/api/v3/mcp/servers/{server_id}/instances`                               |
| Write  | `DELETE` | `/api/v3/mcp/servers/{server_id}/instances/{instance_id}`                 |
| Write  | `POST`   | `/api/v3/mcp/{server_id}/{transport}`                                     |
| Write  | `DELETE` | `/api/v3/mcp/{server_id}/{transport}`                                     |
| Write  | `POST`   | `/tool_router/{session_id}/mcp`                                           |
| Write  | `DELETE` | `/tool_router/{session_id}/mcp`                                           |
| Write  | `POST`   | `/api/v3.1/tool_router/session`                                           |
| Write  | `POST`   | `/api/v3.1/tool_router/session/{session_id}/execute`                      |
| Write  | `POST`   | `/api/v3.1/tool_router/session/{session_id}/execute_meta`                 |
| Write  | `POST`   | `/api/v3/tool_router/session/{session_id}/link`                           |
| Write  | `POST`   | `/api/v3.1/tool_router/session/{session_id}/search`                       |
| Write  | `PATCH`  | `/api/v3.1/tool_router/session/{session_id}`                              |
| Write  | `POST`   | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/upload_url`   |
| Write  | `POST`   | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/download_url` |
| Write  | `POST`   | `/api/v3/tool_router/session/{session_id}/mounts/{mount_id}/delete`       |
| Write  | `POST`   | `/api/v3.1/tool_router/session/{session_id}/attach`                       |

# What to read next [#what-to-read-next]

- [Authenticating to Composio](/reference/authenticating-to-composio): Authenticate API requests with project and organization API keys

- [Projects](/reference/api-reference/projects): Understand projects, project API keys, and project-scoped resources

- [Proxy execute](/reference/api-reference/tools): Call connected account APIs through the raw proxy path

- [Observability](/reference/api-reference/logs): Inspect tool execution logs and usage summaries

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

