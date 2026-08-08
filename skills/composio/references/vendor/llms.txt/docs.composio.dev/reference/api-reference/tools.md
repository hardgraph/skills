# Tools (/reference/api-reference/tools)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/tools.mdx, not this file. */}

Tools are the individual executable actions inside a toolkit, like `GMAIL_SEND_EMAIL` or `GITHUB_CREATE_ISSUE`. Each tool has an input schema describing its parameters and an output schema describing what it returns. Tool slugs are SCREAMING\_SNAKE\_CASE and follow a `{TOOLKIT}_{ACTION}` pattern.

Reach for these endpoints when you want to:

* List or search the catalog of available tools, optionally scoped to one or more toolkits.
* Fetch a single tool's input and output schema by `tool_slug` before constructing a call.
* Execute a tool on behalf of a user's connected account, or generate the inputs for an execution from natural language.
* Look up the OAuth scopes a set of tools requires, or send an authenticated proxy request to an app's underlying API.

These endpoints authenticate with your project API key in the `x-api-key` header.

> Manual tool execution requires an explicit toolkit version. Pass `toolkit_versions=latest` (or pin a dated version like `20251027_00`) so calls resolve to a known tool definition. See the [toolkit versioning migration guide](/docs/migration-guide/toolkit-versioning).

For the concepts behind tools, schemas, and authentication, see [Tools and toolkits](/docs/how-composio-works).

# Proxy execute [#proxy-execute]

Proxy execute sends an authenticated HTTP request through a toolkit's [connected account](/docs/auth-configuration/connected-accounts) without a predefined tool, and Composio injects the OAuth token, API key, or other credentials on the server side so your code never touches raw secrets. Reach for it when you need an endpoint that Composio's predefined tools do not cover, when you need a request shape (custom query parameters, field masks, or advanced filters) that a predefined tool cannot express, or when a terminal agent would otherwise hardcode a bearer token in a `curl` call.

Call it with `composio.tools.proxyExecute()` in the TypeScript SDK, `composio.tools.proxy()` in the Python SDK, or `POST /api/v3.1/tools/execute/proxy` over HTTP. The `endpoint` can be an absolute URL (`https://api.example.com/v1/resource`) or a path relative to the toolkit's base URL (`/v1/resource`), `method` is the HTTP verb, `connectedAccountId` selects the account to authenticate as, `body` carries the JSON payload, and `parameters` adds extra headers and query parameters. The response forwards the upstream `status`, `headers`, and parsed `data` verbatim.

```typescript
import { Composio } from '@composio/core';

const composio = new Composio({ apiKey: 'your_api_key' });

const { status, data } = await composio.tools.proxyExecute({
  endpoint: '/repos/composiohq/composio/issues/1',
  method: 'GET',
  connectedAccountId: 'ca_github_user_123',
  parameters: [{ name: 'Accept', value: 'application/vnd.github.v3+json', in: 'header' }],
});

console.log(status, data);
```

> Proxy execute rejects cross-domain requests, so the `endpoint` must resolve to the same domain as the connected account's toolkit, and you should not set the `Authorization` header yourself because Composio injects the correct credential for the account's auth scheme. This is an intentional security boundary, not a quota, so it cannot be bypassed by reshaping the request.

Proxy execute is a form of [direct tool execution](/docs/sessions-vs-direct-execution): it bypasses session state, tool schemas, and modifiers. If you are building an agent, prefer [sessions](/docs/configuring-sessions), and use the proxy only for the specific API call that is not available as a tool. The full request and response schema lives in the [`POST /api/v3.1/tools/execute/proxy`](/reference/api-reference/tools/postToolsExecuteProxy) reference.

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `GET` | `/api/v3.1/tools` | [List available tools](/reference/api-reference/tools/getTools) |
| `GET` | `/api/v3.1/tools/enum` | [Get tool enum list](/reference/api-reference/tools/getToolsEnum) |
| `GET` | `/api/v3.1/tools/{tool_slug}` | [Get tool by slug](/reference/api-reference/tools/getToolsByToolSlug) |
| `POST` | `/api/v3.1/tools/execute/{tool_slug}` | [Execute tool](/reference/api-reference/tools/postToolsExecuteByToolSlug) |
| `POST` | `/api/v3.1/tools/execute/{tool_slug}/input` | [Generate tool inputs from natural language](/reference/api-reference/tools/postToolsExecuteByToolSlugInput) |
| `POST` | `/api/v3.1/tools/execute/proxy` | [Execute proxy request](/reference/api-reference/tools/postToolsExecuteProxy) |
| `POST` | `/api/v3.1/tools/scopes/required` | [Get required scopes for tools](/reference/api-reference/tools/postToolsScopesRequired) |

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

