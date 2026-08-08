# Auth Configs (/reference/api-reference/auth-configs)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/auth-configs.mdx, not this file. */}

An auth config is a blueprint that defines how a toolkit authenticates across all your users. It specifies the authentication method, the scopes your tools can request, and which credentials Composio uses to run the OAuth or token flow.

A single auth config applies to every user who connects that toolkit. When a user authenticates against it, Composio creates a [connected account](/reference/api-reference/connected-accounts) that stores their tokens and links them to your user ID.

Each auth config defines:

* **Auth scheme**: OAuth2, API key, Bearer token, or Basic Auth
* **Scopes**: what your tools are allowed to do on the user's behalf
* **Credentials**: Composio's managed app, or your own OAuth client and secrets

Reach for a custom auth config when you need your own branding on consent screens, custom scopes, a dedicated rate-limit quota, or a custom toolkit instance. See [managed vs custom auth](/docs/custom-app-vs-managed-app) for the decision and [how Composio handles authentication](/docs/authentication) for the full picture.

# Auth schemes [#auth-schemes]

The `auth_scheme` on an auth config determines how users authenticate to the toolkit. Composio supports four. The schemes available for a given toolkit come from the toolkit itself.

| Scheme         | What it is                                                                                                                                                                            | When it's used                                                                                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `OAUTH2`       | OAuth 2.0 authorization-code flow. The user authorizes through a hosted consent screen, and Composio stores and automatically refreshes the access and refresh tokens.                | Most apps with user accounts (Gmail, GitHub, Slack, Notion, and so on). Uses Composio's managed OAuth app by default; bring your own for custom branding or scopes. |
| `API_KEY`      | A static API key the user provides. There's no OAuth flow: the key is stored on the connected account and sent on each request.                                                       | Services that authenticate with a key, such as SendGrid, Tavily, or PostHog.                                                                                        |
| `BEARER_TOKEN` | A bearer access token you already hold (for example, from your own OAuth flow). Composio sends it as `Authorization: Bearer <token>` and does not refresh it, so you keep it current. | Bringing an existing OAuth or server-to-server token into Composio, or apps that issue long-lived tokens.                                                           |
| `BASIC`        | HTTP Basic authentication with a username and password.                                                                                                                               | Services that use Basic Auth.                                                                                                                                       |

Most OAuth toolkits work out of the box with Composio managed auth. For the others you supply the credential fields. To choose or customize the scheme, see [managed vs custom auth](/docs/custom-app-vs-managed-app).

These endpoints use your project API key in the `x-api-key` header. Each auth config is addressed by its `nanoid`, and you can enable or disable one without deleting it.

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `POST` | `/api/v3.1/auth_configs` | [Create new authentication configuration](/reference/api-reference/auth-configs/postAuthConfigs) |
| `GET` | `/api/v3.1/auth_configs` | [List authentication configurations with optional filters](/reference/api-reference/auth-configs/getAuthConfigs) |
| `GET` | `/api/v3.1/auth_configs/{nanoid}` | [Get single authentication configuration by ID](/reference/api-reference/auth-configs/getAuthConfigsByNanoid) |
| `PATCH` | `/api/v3.1/auth_configs/{nanoid}` | [Update an authentication configuration](/reference/api-reference/auth-configs/patchAuthConfigsByNanoid) |
| `DELETE` | `/api/v3.1/auth_configs/{nanoid}` | [Delete an authentication configuration](/reference/api-reference/auth-configs/deleteAuthConfigsByNanoid) |
| `PATCH` | `/api/v3.1/auth_configs/{nanoid}/{status}` | [Enable or disable an authentication configuration](/reference/api-reference/auth-configs/patchAuthConfigsByNanoidByStatus) |

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

