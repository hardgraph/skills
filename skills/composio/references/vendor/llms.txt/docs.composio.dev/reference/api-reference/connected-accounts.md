# Connected Accounts (/reference/api-reference/connected-accounts)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/connected-accounts.mdx, not this file. */}

A connected account is a single user's authorized connection to a toolkit. It stores their credentials (OAuth tokens or API keys) and links them to your user ID, so your tools can act on that user's behalf.

Composio creates a connected account when a user completes the flow defined by an [auth config](/reference/api-reference/auth-configs). From there you manage its full lifecycle:

* **Create or link**: start a new connection, or generate an auth link session for the user to authorize. See [manually authenticating users](/docs/manually-authenticating).
* **Refresh**: renew authentication for an account whose tokens have expired.
* **Enable, disable, or update**: change an account's status or metadata without removing it.
* **Revoke or delete**: revoke the grant at the provider, or remove the account from Composio.

Each account is addressed by its `nanoid`. List endpoints accept filters so you can find accounts by user, toolkit, or auth config.

# Link auth (Composio Connect Links) [#link-auth-composio-connect-links]

A Composio Connect Link is a hosted, secure sign-in page. You create one with the create auth link session endpoint, redirect the user to the returned URL, and Composio handles the rest: the user signs in, Composio creates the connected account, and Composio stores and refreshes its tokens. Credentials never pass through your app. This works for all Composio managed connections, with no OAuth credentials to set up.

By default a connected account is `PRIVATE` and usable only by its owning user. Mark one `SHARED` to let other users reach it through a per-connection access control list. See [shared connections](/docs/shared-connections).

These endpoints use your project API key in the `x-api-key` header.

> Shared-connection ACL fields are experimental and nested under an `experimental` block on the wire. Pin a specific SDK version if you depend on the current shape.

# Callback identity verification [#callback-identity-verification]

Anyone who opens a Connect Link and consents becomes the account attached to that flow. On its own that is exploitable: someone starts a connection under their own user, copies the authorization URL before consenting, and gets a different person to finish it, attaching that person's provider account under the attacker's identity. This is OAuth session fixation. Callback identity verification defends against it by confirming the returning user before a connection activates.

Set a verifier URL on a project, and Composio holds every OAuth connection there until your server confirms who came back. After the provider callback, Composio redirects the browser to the endpoint you host with one query parameter, `session_uri`, which carries no connection id, user id, or toolkit name. From your server, authenticating with your project API key, you post the `session_uri` and the signed-in `user_id` to the [complete auth endpoint](/reference/api-reference/connected-accounts/postConnectedAccountsCompleteAuth). On a match the connection activates and returns its `connected_account_id` and `toolkit_slug`; a `200` comes back only once it is `ACTIVE`. Only a `user_id` that doesn't match fails the connection: the request returns `400`, and the connection moves to `FAILED` with its `status_reason` set to `Callback identity verification failed`. Any other error leaves the connection pending, so you can restart the flow. The `session_uri` is single-use and valid for ten minutes; redeeming it spends the session whatever the outcome, so a repeat call returns `404`.

Your endpoint owns the redirect from here: once you redeem the session, send the user onward yourself. A `callback_url` set when you create the connection is not used.

Turn it on in the dashboard under **Settings → General → Configuration**. The URL must be public HTTPS, and Composio rejects private and reserved addresses on save, so local development needs a tunnel. Verification is opt-in per project, and once set it covers every OAuth connection in the project that goes through a provider redirect, whether you start it from the API or a Connect Link.

> Connections you start from the dashboard can't complete while a verifier URL is set. A dashboard connection is owned by a Composio dashboard user, which isn't one of your app's users and isn't disclosed to your endpoint, so your app can't report a matching `user_id`. Test from your own app, or clear the verifier URL while you work in the dashboard.

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `POST` | `/api/v3.1/connected_accounts/{nanoid}/revoke` | [Revoke a connected account at the provider](/reference/api-reference/connected-accounts/postConnectedAccountsByNanoidRevoke) |
| `POST` | `/api/v3.1/connected_accounts/complete_auth` | [Complete a deferred OAuth connection after identity verification](/reference/api-reference/connected-accounts/postConnectedAccountsCompleteAuth) |
| `GET` | `/api/v3.1/connected_accounts` | [List connected accounts with optional filters](/reference/api-reference/connected-accounts/getConnectedAccounts) |
| `POST` | `/api/v3.1/connected_accounts` | [Create a new connected account](/reference/api-reference/connected-accounts/postConnectedAccounts) |
| `GET` | `/api/v3.1/connected_accounts/{nanoid}` | [Get connected account details by ID](/reference/api-reference/connected-accounts/getConnectedAccountsByNanoid) |
| `DELETE` | `/api/v3.1/connected_accounts/{nanoid}` | [Delete a connected account](/reference/api-reference/connected-accounts/deleteConnectedAccountsByNanoid) |
| `PATCH` | `/api/v3.1/connected_accounts/{nanoid}` | [Update a connected account](/reference/api-reference/connected-accounts/patchConnectedAccountsByNanoid) |
| `PATCH` | `/api/v3.1/connected_accounts/{nanoId}/status` | [Enable or disable a connected account](/reference/api-reference/connected-accounts/patchConnectedAccountsByNanoIdStatus) |
| `POST` | `/api/v3.1/connected_accounts/{nanoid}/refresh` | [Re-initiate authentication for a connected account (DEPRECATED) (Legacy)](/reference/api-reference/connected-accounts/postConnectedAccountsByNanoidRefresh) |
| `POST` | `/api/v3.1/connected_accounts/link` | [Create a new auth link session](/reference/api-reference/connected-accounts/postConnectedAccountsLink) |

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

