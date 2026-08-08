# Organization (/reference/api-reference/organization)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/organization.mdx, not this file. */}

The Usage API returns **aggregated counts** of tool calls and sessions. Use it to power billing dashboards, customer-facing analytics, or internal utilization reports. For individual events, use the [Logs API](/reference/api-reference/logs).

There are two query shapes:

* **Summary**: totals across one or more entity types in a time window.
* **Breakdown**: one entity type, grouped by a dimension (tool, user, session, etc.).

Each shape comes in an org-scoped and a project-scoped flavor. The org-scoped endpoints are documented below; the project-scoped usage endpoints (`POST /api/v3.1/project/usage/*`) also appear on the [Projects](/reference/api-reference/projects) reference page.

# Authentication [#authentication]

| Endpoint                                     | Header                                   | Scope                    |
| -------------------------------------------- | ---------------------------------------- | ------------------------ |
| `POST /api/v3.1/org/usage/summary`           | `x-org-api-key&#x60; &#x2A;(or org JWT)* | All projects in your org |
| `POST /api/v3.1/org/usage/{entity_type}`     | `x-org-api-key&#x60; &#x2A;(or org JWT)* | All projects in your org |
| `POST /api/v3.1/project/usage/summary`       | `x-api-key&#x60; &#x2A;(or cookie)*      | Single project           |
| `POST /api/v3.1/project/usage/{entity_type}` | `x-api-key&#x60; &#x2A;(or cookie)*      | Single project           |

The org endpoints accept a `project_id` filter so you can slice by project without rotating keys.

# Entity types [#entity-types]

| Entity type  | What it counts                              |
| ------------ | ------------------------------------------- |
| `tool_calls` | Every tool execution (successful or failed) |
| `sessions`   | Sessions created                            |

# Summary [#summary]

Totals across entity types for a time window.

```bash
curl -X POST https://backend.composio.dev/api/v3.1/project/usage/summary \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "from": 1744848000000,
    "to": 1744934400000,
    "entity_types": ["tool_calls", "sessions"]
  }'
```

Response:

```json
{
  "entities": {
    "tool_calls": { "unit": "count", "total_quantity": "142", "event_count": 142 },
    "sessions":  { "unit": "count", "total_quantity": "8",   "event_count": 8 }
  }
}
```

## Summary parameters [#summary-parameters]

| Field                | Type                | Default     | Notes                                                          |
| -------------------- | ------------------- | ----------- | -------------------------------------------------------------- |
| `from`               | number              | 30 days ago | Epoch milliseconds                                             |
| `to`                 | number              | now         | Epoch milliseconds                                             |
| `entity_types`       | string\[]           | all         | Subset of `tool_calls`, `sessions`                             |
| `filters.user_id`    | string \| string\[] | —           | Filter events by initiating user                               |
| `filters.session_id` | string \| string\[] | —           | Filter events by session                                       |
| `filters.project_id` | string \| string\[] | —           | Only meaningful on org endpoints; ignored on project endpoints |

# Breakdown [#breakdown]

One entity type, grouped by a dimension. Useful for answering "top N" questions.

```bash
curl -X POST https://backend.composio.dev/api/v3.1/project/usage/tool_calls \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "from": 1744848000000,
    "to": 1744934400000,
    "group_by": "toolkit_slug",
    "order_by": "total_quantity",
    "order_direction": "desc",
    "limit": 10
  }'
```

Response:

```json
{
  "entity_type": "tool_calls",
  "unit": "count",
  "total_quantity": "142",
  "event_count": 142,
  "groups": [
    { "key": "github", "total_quantity": "80", "event_count": 80 },
    { "key": "slack",  "total_quantity": "62", "event_count": 62 }
  ]
}
```

## Breakdown `group_by` options [#breakdown-group_by-options]

| Entity       | Org scope                                                                                  | Project scope           | Default     |
| ------------ | ------------------------------------------------------------------------------------------ | ----------------------- | ----------- |
| `tool_calls` | `tool_slug`, `toolkit_slug`, `connected_account_id`, `user_id`, `session_id`, `project_id` | same minus `project_id` | `tool_slug` |
| `sessions`   | `user_id`, `project_id`                                                                    | `user_id`               | `user_id`   |

## Breakdown parameters [#breakdown-parameters]

| Field             | Type                | Default          | Notes                                         |
| ----------------- | ------------------- | ---------------- | --------------------------------------------- |
| `from`            | number              | 30 days ago      | Epoch ms                                      |
| `to`              | number              | now              | Epoch ms                                      |
| `group_by`        | string              | see table        | Dimension to group by                         |
| `order_by`        | string              | `total_quantity` | One of `key`, `total_quantity`, `event_count` |
| `order_direction` | `"asc"` \| `"desc"` | `"desc"`         |                                               |
| `limit`           | number              | 50               | Max groups returned                           |
| `filters`         | object              | —                | See Filters below                             |

# Filters [#filters]

Filters live in a `filters` object on the request body. Each filter value can be a **single string** or an **array of strings**:

* Within a single field, values are OR-combined (`user_id: ["a", "b"]` matches events for user *a or b*).
* Across fields, filters are AND-combined.

```json
{
  "filters": {
    "user_id": ["user_123", "user_456"],
    "session_id": "sess_abc"
  }
}
```

The `project_id` filter is only meaningful on the org-scoped endpoints. Project endpoints accept the field but ignore it (your key already pins the scope to a single project).

# Time ranges [#time-ranges]

* `from` and `to` are **epoch milliseconds**.
* `from` defaults to 30 days before `to`.
* `to` defaults to the current time.
* Maximum range: **366 days**. Longer ranges return a 400.

# Recipes [#recipes]

## Top 10 tools my org called last week [#top-10-tools-my-org-called-last-week]

```bash
WEEK_AGO=$(( $(date +%s) - 604800 ))000
NOW=$(date +%s)000
curl -X POST https://backend.composio.dev/api/v3.1/org/usage/tool_calls \
  -H "x-org-api-key: YOUR_ORG_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"from\": ${WEEK_AGO},
    \"to\": ${NOW},
    \"group_by\": \"tool_slug\",
    \"limit\": 10
  }"
```

## Tool call count per user for my project this month [#tool-call-count-per-user-for-my-project-this-month]

```bash
MONTH_AGO=$(( $(date +%s) - 2592000 ))000
NOW=$(date +%s)000
curl -X POST https://backend.composio.dev/api/v3.1/project/usage/tool_calls \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"from\": ${MONTH_AGO},
    \"to\": ${NOW},
    \"group_by\": \"user_id\",
    \"limit\": 50
  }"
```

## Which toolkits is a specific user using? [#which-toolkits-is-a-specific-user-using]

```bash
curl -X POST https://backend.composio.dev/api/v3.1/project/usage/tool_calls \
  -H "x-api-key: YOUR_PROJECT_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "group_by": "toolkit_slug",
    "filters": { "user_id": "user_abc123" }
  }'
```

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `POST` | `/api/v3.1/org/usage/summary` | [Org usage summary](/reference/api-reference/organization/postOrgUsageSummary) |
| `POST` | `/api/v3.1/org/usage/{entity_type}` | [Org usage breakdown](/reference/api-reference/organization/postOrgUsageByEntityType) |

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

