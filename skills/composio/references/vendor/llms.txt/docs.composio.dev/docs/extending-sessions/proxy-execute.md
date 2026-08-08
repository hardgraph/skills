# Proxy execute (/docs/extending-sessions/proxy-execute)

`session.proxyExecute()` calls any HTTP endpoint on a toolkit your session can already reach, and Composio injects the authentication (OAuth token, API key, basic auth, and so on) on the server side. Your code never handles raw credentials.

The session is already scoped to a [userID](/docs/how-composio-works), so you pass a `toolkit` slug rather than an account ID. Composio resolves the user's connected account for that toolkit and signs the request with it.

Proxy execute is a building block, not just a fallback. Use it to build experiences on top of Composio that reach past predefined tools, like the [Pi + Slack bot example](/examples/general-agent-with-pi#reach-the-gaps-with-the-proxy), which drops down to the proxy for the Slack Web API calls the toolkit doesn't wrap as tools. We'll link more examples here as we add them.

> **Use a scoped API key**: Proxy execute is gated behind its own permission. Authenticate with a [scoped project API key](/reference/authenticating-to-composio/project-api-key-permissions) that has the **Proxy execute** permission granted. Default full-access keys already include it.

# When to use it [#when-to-use-it]

* **Endpoints with no predefined tool.** You need a specific endpoint (an unusual GitHub, LinkedIn, or Notion route) that isn't exposed as a Composio tool. Send it through the proxy instead of extracting the raw token and calling the API yourself.
* **Request shapes a tool can't express.** Custom query parameters, partial field masks, or advanced filters on Gmail, Drive, Sheets, and similar. The proxy gives you the full HTTP surface of the upstream API while Composio keeps managing auth.

# Quick start [#quick-start]

**Python:**

```python
from composio import Composio

composio = Composio(api_key="your_api_key")
session = composio.create("user_123", toolkits=["github"])

response = session.proxy_execute(
    toolkit="github",
    endpoint="/repos/composiohq/composio/issues/1",
    method="GET",
    parameters=[
        {"name": "Accept", "value": "application/vnd.github.v3+json", "in": "header"},
    ],
)

print(response.status)
print(response.data)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create('user_123', { toolkits: ['github'] });
const { status, data } = await session.proxyExecute({
  toolkit: 'github',
  endpoint: '/repos/composiohq/composio/issues/1',
  method: 'GET',
  parameters: [
    { name: 'Accept', value: 'application/vnd.github.v3+json', in: 'header' },
  ],
});

console.log(status);
console.log(data);
```

The `endpoint` is a path relative to the toolkit's base URL (`/repos/...` resolves against `api.github.com`). Pass an absolute URL only when you need a host that isn't the toolkit's standard API, such as a regional Salesforce or Zendesk domain.

> Proxy execute rejects cross-domain requests. The `endpoint` must resolve to the same domain as the toolkit's connected account (a GitHub connection can only call `api.github.com` paths). This is an intentional security boundary, not a quota, so you can't work around it by reshaping the request.

# Parameters [#parameters]

| Parameter    | Required | Type                                              | Description                                                                                                       |
| ------------ | -------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `toolkit`    | Yes      | `string`                                          | Toolkit slug (`github`, `gmail`, and so on). Composio uses the session user's connected account for this toolkit. |
| `endpoint`   | Yes      | `string`                                          | Path relative to the toolkit's base URL, or an absolute URL.                                                      |
| `method`     | Yes      | `"GET" \| "POST" \| "PUT" \| "PATCH" \| "DELETE"` | HTTP verb.                                                                                                        |
| `body`       | No       | `object`                                          | JSON request body. Used with `POST`, `PUT`, and `PATCH`.                                                          |
| `parameters` | No       | `Array<{ name, value, in }>`                      | Extra headers or query parameters. `in` is `"header"` or `"query"`.                                               |

# Response shape [#response-shape]

The call returns the upstream response verbatim:

| Field        | Type                     | Description                                                                                |
| ------------ | ------------------------ | ------------------------------------------------------------------------------------------ |
| `status`     | `number`                 | HTTP status code from the upstream API.                                                    |
| `data`       | `unknown`                | Parsed JSON body the API returned.                                                         |
| `headers`    | `Record<string, string>` | Response headers.                                                                          |
| `binaryData` | `object`                 | Present only when the upstream returns a file (`url`, `contentType`, `size`, `expiresAt`). |

> Don't set the `Authorization` header yourself through `parameters`. Composio injects the correct one from the connected account's auth scheme, and setting it manually overrides that credential and usually produces a `401`.

# Error handling [#error-handling]

`status` and `data` reflect exactly what the toolkit API returned, so check `status` and branch on the common failures.

| Status                  | Typical cause                                                   | How to resolve                                                                                              |
| ----------------------- | --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `400 Bad Request`       | Malformed endpoint path, invalid body, or unsupported `method`. | Check the upstream API docs for the expected shape. The proxy doesn't validate upstream schemas.            |
| `401 Unauthorized`      | The connected account's token expired or was revoked.           | Re-authenticate the user, or [import fresh credentials](/docs/importing-existing-connections).              |
| `403 Forbidden`         | The user's OAuth scopes or API key don't cover this endpoint.   | Update the [auth config scopes](/docs/auth-configuration/custom-auth-configs) and have the user re-consent. |
| `429 Too Many Requests` | Upstream rate limit (GitHub, Google, and so on).                | Honor the `Retry-After` header and back off. Composio doesn't retry automatically.                          |

# Next [#next]

- [Custom tools and toolkits](/docs/extending-sessions/custom-tools-and-toolkits): Define in-process tools and toolkits that run alongside Composio tools

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

