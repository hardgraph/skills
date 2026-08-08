# Overview (/reference/authenticating-to-composio)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

Every Composio API request authenticates with an API key. Send the key in a request header and Composio resolves it to your project or organization.

Composio has three kinds of API key. They share the same authentication flow — they differ in what they can reach.

| Key                                                                        | Header          | Scope                                             |
| -------------------------------------------------------------------------- | --------------- | ------------------------------------------------- |
| Project API key                                                            | `x-api-key`     | Full access to a single project.                  |
| Organization API key                                                       | `x-org-api-key` | Access across every project in your organization. |
| Scoped project API key <span style="{ color: '#059669' }">**· New**</span> | `x-api-key`     | A chosen subset of a single project's resources.  |

# Project API key [#project-api-key]

A project API key authenticates to one project with full access. Use it for most application code.

Get it from the dashboard: sign in to [composio.dev](https://composio.dev), open **Settings → Project Settings**, and copy the key from the **API Keys** section.

Send it in the `x-api-key` header:

```bash
curl https://backend.composio.dev/api/v3.1/tools \
  -H "x-api-key: $COMPOSIO_API_KEY"
```

# Organization API key [#organization-api-key]

An organization API key authenticates across every project in your organization. Use it for organization-level endpoints.

Get it from the dashboard: open **Organization Settings → General Settings** and copy a token under **Organization Access Tokens**.

Send it in the `x-org-api-key` header:

```bash
curl https://backend.composio.dev/api/v3.1/org/projects \
  -H "x-org-api-key: $COMPOSIO_ORG_API_KEY"
```

# Scoped project API key [#scoped-project-api-key]

A scoped project API key authenticates to a single project but reaches only the resources you grant it — for example, executing tools without managing connected accounts. It uses the same `x-api-key` header as a default project key.

Scope a key to the least it needs, then send it like any project key:

```bash
curl https://backend.composio.dev/api/v3.1/tools/execute/HACKERNEWS_GET_USER \
  -H "x-api-key: $COMPOSIO_SCOPED_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"arguments": {"username": "pg"}}'
```

See [Scoped project API keys](/reference/authenticating-to-composio/project-api-key-permissions) for the permission areas, access levels, and the routes each one covers.

- [Scoped project API keys](/reference/authenticating-to-composio/project-api-key-permissions): 
Permission areas, access levels, and covered routes

- [Errors](/reference/errors): 
Understanding API error responses

- [Rate Limits](/reference/rate-limits): 
API rate limits by plan

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

