# Logs (/reference/api-reference/logs)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/logs.mdx, not this file. */}

The Logs API returns **individual tool execution events**, one record per tool call. Use it to debug failures, inspect request/response payloads, and trace specific user activity. For aggregated counts (how many tool calls happened), use the [Usage API](/reference/api-reference/organization) instead.

All endpoints in this section require a **project API key** (`x-api-key`) or a valid session cookie.

# List logs [#list-logs]

```bash
curl -X POST https://backend.composio.dev/api/v3.1/logs/tool_execution \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "limit": 20,
    "time_range": {
      "from": 1744848000000,
      "to": 1744934400000
    },
    "filters": [
      { "field": "toolkit_slug", "operator": "==", "value": "gmail" },
      { "field": "status", "operator": "==", "value": "failed" }
    ]
  }'
```

The response contains a page of log entries and a `next_cursor`:

```json
{
  "logs": [
    {
      "id": "log_-jRTWClpBoVo",
      "timestamp": "2026-04-17T10:25:00.000Z",
      "type": "tool.execution",
      "status": "failed",
      "level": "error",
      "message": "GMAIL_SEND_EMAIL failed: invalid recipient",
      "metadata": { /* tool, toolkit, user_id, connected_account_id, ... */ },
      "metrics": { "duration_ms": 202 },
      "parent": null
    }
  ],
  "next_cursor": "eyJwYWdlIjoyfQ=="
}
```

Pass `next_cursor` back as `cursor` on the next request to paginate. When `next_cursor` is `null`, you've reached the end.

## Filter fields [#filter-fields]

Pass one or more filters in the `filters` array. Filters are **AND**-combined.

| Field                  | What it matches                                             |
| ---------------------- | ----------------------------------------------------------- |
| `tool_slug`            | The specific tool that was called (e.g. `GMAIL_SEND_EMAIL`) |
| `toolkit_slug`         | The toolkit (e.g. `gmail`, `slack`, `github`)               |
| `connected_account_id` | The connected account used for the call                     |
| `auth_config_id`       | The auth config (integration) behind the connected account  |
| `status`               | `success` or `failed`                                       |
| `user_id`              | Entity that initiated the call                              |
| `session_id`           | Tool router session, if routed through a session            |
| `sandbox_id`           | Sandbox the call ran in, if applicable                      |
| `request_id`           | Request ID (useful for correlating with your own logs)      |
| `log_id`               | Exact log ID (equivalent to the detail endpoint)            |

## Operators [#operators]

| Operator       | Meaning                  |
| -------------- | ------------------------ |
| `==`           | Exact match              |
| `!=`           | Not equal                |
| `contains`     | Substring match          |
| `not_contains` | Substring does not match |

## Parameters [#parameters]

| Field             | Type           | Default | Notes                                          |
| ----------------- | -------------- | ------- | ---------------------------------------------- |
| `limit`           | number         | `20`    | Max 100                                        |
| `cursor`          | string \| null | `null`  | Opaque pagination token from previous response |
| `filters`         | array          | `[]`    | AND-combined                                   |
| `time_range.from` | number         | —       | Epoch milliseconds                             |
| `time_range.to`   | number         | —       | Epoch milliseconds                             |

# Get a single log [#get-a-single-log]

Fetch one log by ID to get the **full** payload, including request/response bodies, timing breakdowns, and source metadata:

```bash
curl https://backend.composio.dev/api/v3.1/logs/tool_execution/log_-jRTWClpBoVo \
  -H "x-api-key: YOUR_PROJECT_API_KEY"
```

The detail response includes everything from the list shape plus:

* `timings`: `start_time` and `end_time` in epoch ms
* `context`: `session_id`, `trace_id`, `request_id`
* `source`: `host` (e.g. `mcp`, `sdk`, `api`), `framework`, `language`
* `data`: the full request payload and response body

This is the endpoint to call when you need to reconstruct *exactly* what happened, for example when debugging a 500 from a user report.

# Recipes [#recipes]

## Find failed Gmail tool calls in the last hour [#find-failed-gmail-tool-calls-in-the-last-hour]

```bash
NOW=$(date +%s)000
HOUR_AGO=$(( $(date +%s) - 3600 ))000
curl -X POST https://backend.composio.dev/api/v3.1/logs/tool_execution \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"time_range\": { \"from\": ${HOUR_AGO}, \"to\": ${NOW} },
    \"filters\": [
      { \"field\": \"toolkit_slug\", \"operator\": \"==\", \"value\": \"gmail\" },
      { \"field\": \"status\", \"operator\": \"==\", \"value\": \"failed\" }
    ]
  }"
```

## Get failures for a specific user [#get-failures-for-a-specific-user]

```bash
curl -X POST https://backend.composio.dev/api/v3.1/logs/tool_execution \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "filters": [
      { "field": "user_id", "operator": "==", "value": "user_abc123" },
      { "field": "status", "operator": "==", "value": "failed" }
    ]
  }'
```

## Fetch a single log's full request/response [#fetch-a-single-logs-full-requestresponse]

```bash
curl https://backend.composio.dev/api/v3.1/logs/tool_execution/log_-jRTWClpBoVo \
  -H "x-api-key: YOUR_PROJECT_API_KEY"
```

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `POST` | `/api/v3.1/logs/tool_execution` | [Search and retrieve tool execution logs](/reference/api-reference/logs/postLogsToolExecution) |
| `GET` | `/api/v3.1/logs/tool_execution/{id}` | [Get log details by ID](/reference/api-reference/logs/getLogsToolExecutionById) |

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

