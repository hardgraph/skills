# Webhook Subscriptions (/reference/api-reference/webhook-subscriptions)

> **API version:** This page documents Composio REST API v3.1, the current version, at `https://backend.composio.dev/api/v3.1`. `https://backend.composio.dev/api/v3` is the previous version and remains supported.

{/* Auto-generated from OpenAPI spec. Edit the overview at api-overviews/webhook-subscriptions.mdx, not this file. */}

Webhook subscriptions are outbound delivery configurations. They define the URL Composio posts [trigger](/docs/triggers) events to, along with the signing secret and the set of event types you want to receive.

Reach for these endpoints to register where Composio should send events, to filter delivery to specific event types, and to manage the signing secret used to verify those deliveries. List the available event types with the `/webhook_subscriptions/event_types` endpoint, then create a subscription scoped to the ones you care about.

Each subscription is addressed by its `id`. You can update its URL and filters with `PATCH`, delete it, and rotate its signing secret with `/webhook_subscriptions/{id}/rotate_secret` if the secret leaks.

Every webhook request Composio sends includes `webhook-id`, `webhook-timestamp`, and `webhook-signature` headers. Store the secret as `COMPOSIO_WEBHOOK_SECRET` and verify each payload before trusting it. See [Verifying signatures](/docs/setting-up-triggers/subscribing-to-events#verifying-signatures) for the SDK and manual verification flows.

# Event types [#event-types]

A subscription's `enabled_events` controls which events get delivered to its URL. Two broad families exist:

* **Trigger events** like `composio.trigger.message` — payloads emitted by [triggers](/docs/triggers) you've enabled (new email, new issue, etc.).
* **Lifecycle events** like `composio.connected_account.expired` — emitted when a [connected account](/docs/auth-configuration/connected-accounts) changes state.

List everything you can subscribe to with the `/webhook_subscriptions/event_types` endpoint, then scope a subscription to the events you handle.

# Detecting connection expiry [#detecting-connection-expiry]

Composio automatically refreshes OAuth tokens before they expire. But when a refresh token is revoked or expires, the connection enters an `EXPIRED` state and the user must re-authenticate.

Subscribe to the `composio.connected_account.expired` event to detect this proactively, instead of waiting for a tool execution to fail.

> This event is only available with [V3 webhook payloads](/docs/setting-up-triggers/subscribing-to-events#webhook-payload-versions). New organizations use V3 by default.

Add `composio.connected_account.expired` to the subscription's `enabled_events`:

```bash
curl -X POST https://backend.composio.dev/api/v3.1/webhook_subscriptions \
  -H "X-API-KEY: <your-composio-api-key>" \
  -H "Content-Type: application/json" \
  -d '{
    "webhook_url": "https://example.com/webhook",
    "enabled_events": [
      "composio.trigger.message",
      "composio.connected_account.expired"
    ]
  }'
```

When a connection expires, Composio sends a webhook with the connected account details:

```json
{
  "id": "evt_847cdfcd-d219-4f18-a6dd-91acd42ca94a",
  "type": "composio.connected_account.expired",
  "metadata": {
    "project_id": "pr_your-project-id",
    "org_id": "ok_your-org-id"
  },
  "data": {
    "id": "ca_your-connected-account-id",
    "toolkit": { "slug": "gmail" },
    "auth_config": {
      "id": "ac_your-auth-config-id",
      "auth_scheme": "OAUTH2"
    },
    "status": "EXPIRED",
    "status_reason": "OAuth refresh token expired"
  },
  "timestamp": "2026-02-06T12:00:00.000Z"
}
```

Route on `type` to handle expiry alongside trigger events:

**Python:**

```python
from composio import Composio, WebhookEventType

composio = Composio()

@app.post("/webhook")
async def webhook_handler(request: Request):
    payload = await request.json()
    event_type = payload.get("type")

    if event_type == WebhookEventType.CONNECTION_EXPIRED:
        account_id = payload["data"]["id"]
        toolkit = payload["data"]["toolkit"]["slug"]

        # Look up the user and send them a re-auth link
        session = composio.create(user_id=lookup_user(account_id))
        connection_request = session.authorize(toolkit)
        notify_user(connection_request.redirect_url)

    elif event_type == WebhookEventType.TRIGGER_MESSAGE:
        # Handle trigger events
        pass

    return {"status": "ok"}
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio();
type NextApiRequest = { body: any };
type NextApiResponse = { status: (code: number) => { json: (data: any) => void } };
declare function lookupUser(accountId: string): string;
declare function notifyUser(url: string): void;
export default async function webhookHandler(req: NextApiRequest, res: NextApiResponse) {
  const payload = req.body;

  if (payload.type === 'composio.connected_account.expired') {
    const accountId = payload.data.id;
    const toolkit = payload.data.toolkit.slug;

    // Look up the user and send them a re-auth link
    const session = await composio.create(lookupUser(accountId));
    const connectionRequest = await session.authorize(toolkit);
    if (connectionRequest.redirectUrl) {
      notifyUser(connectionRequest.redirectUrl);
    }

  } else if (payload.type === 'composio.trigger.message') {
    // Handle trigger events
  }

  res.status(200).json({ status: 'ok' });
}
```

> Always [verify webhook signatures](/docs/setting-up-triggers/subscribing-to-events#verifying-signatures) before processing events in production.

# Endpoints [#endpoints]

| Method | Path | Endpoint |
| --- | --- | --- |
| `POST` | `/api/v3.1/webhook_subscriptions` | [Create webhook subscription](/reference/api-reference/webhook-subscriptions/postWebhookSubscriptions) |
| `GET` | `/api/v3.1/webhook_subscriptions` | [List webhook subscriptions](/reference/api-reference/webhook-subscriptions/getWebhookSubscriptions) |
| `GET` | `/api/v3.1/webhook_subscriptions/{id}` | [Get webhook subscription](/reference/api-reference/webhook-subscriptions/getWebhookSubscriptionsById) |
| `PATCH` | `/api/v3.1/webhook_subscriptions/{id}` | [Update webhook subscription](/reference/api-reference/webhook-subscriptions/patchWebhookSubscriptionsById) |
| `DELETE` | `/api/v3.1/webhook_subscriptions/{id}` | [Delete webhook subscription](/reference/api-reference/webhook-subscriptions/deleteWebhookSubscriptionsById) |
| `POST` | `/api/v3.1/webhook_subscriptions/{id}/rotate_secret` | [Rotate webhook secret](/reference/api-reference/webhook-subscriptions/postWebhookSubscriptionsByIdRotateSecret) |
| `GET` | `/api/v3.1/webhook_subscriptions/event_types` | [List available event types](/reference/api-reference/webhook-subscriptions/getWebhookSubscriptionsEventTypes) |

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

