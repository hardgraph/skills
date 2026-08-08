# Custom OAuth webhooks (/docs/setting-up-triggers/custom-oauth-webhooks)

This page is only for **realtime triggers where you bring your own OAuth app**. With Composio-managed OAuth, ingress is already set up and you can skip this entirely. Create the trigger and events flow.

Some providers only deliver events to URLs you've registered on your OAuth app. When you bring your own app, you register Composio's ingress URL there once, so events can reach Composio. You need this only when the trigger type's `requires_webhook_endpoint_setup` flag is `true`.

Each OAuth app you bring gets its own ingress URL within a project:

```
https://backend.composio.dev/api/v3.1/webhook_ingress/{toolkit_slug}/{we_xxx}/trigger_event
```

A single OAuth app can serve at most one Composio project: providers accept only one callback URL per OAuth app, and each ingress URL routes to a single project. In return, every project becomes its own webhook tenant, with:

* **Its own ingress rate limit and backpressure budget**
* **Project-scoped credentials**: the signing secret and app-level token you provide are stored against this project alone, never shared across projects. Repeat verification handshakes are rejected after the endpoint is verified, so the signing secret can't be silently swapped by a forged challenge.
* **Clean fan-out**: events reach only that project's trigger instances
* **Per-project metering**

Every inbound event is signature-checked at ingress before any trigger fires:

* **HMAC-SHA256** for Slack, **Ed25519** or shared-token matching for other providers
* **Timestamp replay protection**: when the provider signs a request timestamp, requests outside the allowed skew window are rejected
* **Unsigned or tampered requests** are rejected with `400` at ingress, so third parties can't spoof events onto your triggers

> **Sharing one OAuth app across projects?** Consolidate to a single project or register separate OAuth apps per project before continuing.

The walkthrough below uses Slack as the example and the [Webhook Endpoints API](/reference/api-reference/webhook-endpoints). For setup notes specific to each toolkit, see its FAQ section, for example [Slack](/toolkits/slack) or [Notion](/toolkits/notion).

# Step 1: Discover what credentials the endpoint needs [#step-1-discover-what-credentials-the-endpoint-needs]

Call the schema endpoint for the toolkit. The `setup_fields` in the response tell you exactly what to collect from the provider's app dashboard.

```bash
curl "https://backend.composio.dev/api/v3.1/webhook_endpoints/schema?toolkit_slug=slack" \
  -H "x-api-key: "
```

Sample response:

```json
{
  "toolkit_slug": "slack",
  "setup_fields": {
    "webhook_signing_secret": {
      "display_name": "Signing Secret",
      "description": "Webhook request signing secret from your Slack app dashboard",
      "is_required": true,
      "is_secret": true
    },
    "app_token": {
      "display_name": "App-Level Token",
      "description": "Slack xapp- token with authorizations:read scope for event authorization",
      "is_required": true,
      "is_secret": true
    }
  }
}
```

# Step 2: Create the endpoint [#step-2-create-the-endpoint]

```bash
curl -X POST "https://backend.composio.dev/api/v3.1/webhook_endpoints" \
  -H "x-api-key: " \
  -H "Content-Type: application/json" \
  -d '{
    "toolkit_slug": "slack",
    "client_id": ""
  }'
```

Sample response:

```json
{
  "id": "we_abc123",
  "toolkit_slug": "slack",
  "client_id": "",
  "webhook_url": "https://backend.composio.dev/api/v3.1/webhook_ingress/slack/we_abc123/trigger_event",
  "data": null,
  "created_at": "2026-04-24T10:00:00.000Z"
}
```

Hold on to two values from the response: `id` (used as `` below) and `webhook_url` (you'll paste this into the provider's app dashboard in step 4). The call is **idempotent per `(toolkit_slug, client_id)` within a project**. Calling it again with the same pair returns the existing endpoint without rotating the URL or wiping the secret.

# Step 3: Store the credentials returned by the schema [#step-3-store-the-credentials-returned-by-the-schema]

`PATCH` all the fields the schema returned in a single request. For Slack, that's the signing secret and (when needed) the app-level token together:

```bash
curl -X PATCH "https://backend.composio.dev/api/v3.1/webhook_endpoints/" \
  -H "x-api-key: " \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "webhook_signing_secret": "",
      "app_token": "xapp-..."
    }
  }'
```

For Slack, the credentials come from:

* **Signing secret**: Slack app → Basic Information → App Credentials → Signing Secret.
* **App-level token**: Slack app → Basic Information → App-Level Tokens, with scope `authorizations:read`. Required for direct messages, private channels, and reactions. Omit it if you only need public-channel events.

> **Store the credentials before you switch the provider's callback URL in step 4.** If the provider posts to the URL without a secret on the endpoint, every request fails with `400`, and the provider may auto-disable the endpoint after a window of consecutive failures (Slack: \~36 hours).

# Step 4: Point the provider's app dashboard at the URL [#step-4-point-the-providers-app-dashboard-at-the-url]

Paste the `webhook_url` from step 2 into the provider's app dashboard:

* **Slack** → Event Subscriptions → Request URL
* **Notion** → Webhook Endpoints (in the integration's settings)

For providers that issue a verification challenge on save (Slack `url_verification`, Notion's verification token, and so on), Composio responds automatically, with no handshake code on your side. Once the provider accepts the URL, go [create your trigger](/docs/setting-up-triggers/creating-triggers).

# Updating an endpoint [#updating-an-endpoint]

To rotate the signing secret or update any single field, `PATCH` it (other fields are preserved):

```bash
curl -X PATCH "https://backend.composio.dev/api/v3.1/webhook_endpoints/" \
  -H "x-api-key: " \
  -H "Content-Type: application/json" \
  -d '{ "data": { "webhook_signing_secret": "" } }'
```

To **replace** `data` wholesale (any field you don't include is cleared), `POST` to the same URL:

```bash
curl -X POST "https://backend.composio.dev/api/v3.1/webhook_endpoints/" \
  -H "x-api-key: " \
  -H "Content-Type: application/json" \
  -d '{ "data": { "webhook_signing_secret": "", "app_token": "" } }'
```

The `webhook_url` is immutable for the lifetime of the endpoint. Rotating the signing secret on the provider side is a `PATCH` on the existing endpoint, not a new one.

To inspect a single endpoint:

```bash
curl "https://backend.composio.dev/api/v3.1/webhook_endpoints/" \
  -H "x-api-key: "
```

To list every endpoint in the current project:

```bash
curl "https://backend.composio.dev/api/v3.1/webhook_endpoints" \
  -H "x-api-key: "
```

# Next [#next]

- [Creating triggers](/docs/setting-up-triggers/creating-triggers): Activate a trigger for a user so events start flowing

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

