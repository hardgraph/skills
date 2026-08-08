# Triggers (/reference/sdk-reference/python/triggers)

# Methods [#methods]

## set\_webhook\_subscription() [#set_webhook_subscription]

Create or update the project webhook subscription used for webhook delivery.  If a subscription already exists, the first subscription is updated. Otherwise a new subscription is created. By default this subscribes to V3 trigger message events.

```python
def set_webhook_subscription(webhook_url: str, enabled_events: Sequence[str | None] = ..., version: Union[WebhookVersion, str] = ...) -> WebhookSubscription
```

**Parameters**

| Name              | Type                         |
| ----------------- | ---------------------------- |
| `webhook_url`     | `str`                        |
| `enabled_events?` | `Sequence[str \| None]`      |
| `version?`        | `Union[WebhookVersion, str]` |

**Returns**

`WebhookSubscription`

**Example**

```python
composio.triggers.set_webhook_subscription(
    webhook_url=f"{APP_URL}/webhooks/composio",
)
```

***

## get\_type() [#get_type]

Get a trigger type by its slug Uses the global toolkit version provided when initializing composio instance to fetch trigger for specific toolkit version

```python
def get_type(slug: str) -> TriggersTypeRetrieveResponse
```

**Parameters**

| Name   | Type  |
| ------ | ----- |
| `slug` | `str` |

**Returns**

`TriggersTypeRetrieveResponse` — The trigger type

***

## list\_active() [#list_active]

List all active triggers

```python
def list_active(trigger_ids: list[str | None] = ..., trigger_names: list[str | None] = ..., auth_config_ids: list[str | None] = ..., connected_account_ids: list[str | None] = ..., show_disabled: bool | None = ..., limit: int | None = ..., cursor: str | None = ...)
```

**Parameters**

| Name                     | Type                |
| ------------------------ | ------------------- |
| `trigger_ids?`           | `list[str \| None]` |
| `trigger_names?`         | `list[str \| None]` |
| `auth_config_ids?`       | `list[str \| None]` |
| `connected_account_ids?` | `list[str \| None]` |
| `show_disabled?`         | `bool \| None`      |
| `limit?`                 | `int \| None`       |
| `cursor?`                | `str \| None`       |

***

## list() [#list]

List all the trigger types.

```python
def list(cursor: str | None = ..., limit: int | None = ..., toolkit_slugs: list[str | None] = ...)
```

**Parameters**

| Name             | Type                |
| ---------------- | ------------------- |
| `cursor?`        | `str \| None`       |
| `limit?`         | `int \| None`       |
| `toolkit_slugs?` | `list[str \| None]` |

***

## create() [#create]

Create a trigger instance

```python
def create(slug: str, user_id: str | None = ..., connected_account_id: str | None = ..., trigger_config: Dict[str, Any | None] = ...) -> trigger_instance_upsert_response.TriggerInstanceUpsertRes...
```

**Parameters**

| Name                    | Type                     |
| ----------------------- | ------------------------ |
| `slug`                  | `str`                    |
| `user_id?`              | `str \| None`            |
| `connected_account_id?` | `str \| None`            |
| `trigger_config?`       | `Dict[str, Any \| None]` |

**Returns**

`trigger_instance_upsert_response.TriggerInstanceUpsertRes...` — The trigger instance

***

## subscribe() [#subscribe]

Subscribe to a trigger and receive trigger events.

```python
def subscribe(timeout: float = ...) -> TriggerSubscription
```

**Parameters**

| Name       | Type    |
| ---------- | ------- |
| `timeout?` | `float` |

**Returns**

`TriggerSubscription` — The trigger subscription handler.

***

## verify\_webhook() [#verify_webhook]

Verify an incoming webhook payload and signature.  This method validates that the webhook request is authentic by: 1. Validating the webhook timestamp is within the tolerance window 2. Verifying the HMAC-SHA256 signature using the correct algorithm 3. Parsing the payload and detecting the webhook version (V1, V2, or V3)

```python
def verify_webhook(id: str, payload: str, secret: str, signature: str, timestamp: str, tolerance: int = ...) -> VerifyWebhookResult
```

**Parameters**

| Name         | Type  |
| ------------ | ----- |
| `id`         | `str` |
| `payload`    | `str` |
| `secret`     | `str` |
| `signature`  | `str` |
| `timestamp`  | `str` |
| `tolerance?` | `int` |

**Returns**

`VerifyWebhookResult` — VerifyWebhookResult containing version, normalized payload, and raw payload :raises WebhookSignatureVerificationError: If the signature verification fails :raises WebhookPayloadError: If the payload cannot be parsed or is invalid

**Example**

```python
# In a Flask webhook handler
@app.route('/webhook', methods=['POST'])
def webhook():
    try:
        result = composio.triggers.verify_webhook(
            id=request.headers.get('webhook-id', ''),
            payload=request.get_data(as_text=True),
            signature=request.headers.get('webhook-signature', ''),
            timestamp=request.headers.get('webhook-timestamp', ''),
            secret=os.environ['COMPOSIO_WEBHOOK_SECRET'],
        )

        # Process the verified payload
        print(f"Version: {result['version']}")
        print(f"Received trigger: {result['payload']['trigger_slug']}")
        return 'OK', 200
    except WebhookSignatureVerificationError:
        return 'Unauthorized', 401
```

***

## parse() [#parse]

Parse an incoming webhook request into a typed, normalized trigger payload.  Pass a framework request object, or pass `body=` and `headers=` explicitly. When `verify_secret` is provided, the SDK verifies the webhook signature before returning the normalized trigger payload. When it is omitted, the SDK parses the body without verifying the signature.  `request` may be any object exposing the request body and headers, such as a Flask, Django, or FastAPI request. The body is read from `.body` (or `.data` / `.get_data()`), and the headers are read from `.headers`. Because this SDK is synchronous, async frameworks must pass an already-read raw body, for example via `body=await request.body()`.

```python
def parse(request: Any = ..., body: Union[str, bytes, Mapping[str, Any], None] = ..., headers: Union[Mapping[str, Any], None] = ..., verify_secret: Union[str, None, Omit] = ..., tolerance: int = ...) -> VerifyWebhookResult
```

**Parameters**

| Name             | Type                                         |
| ---------------- | -------------------------------------------- |
| `request?`       | `Any`                                        |
| `body?`          | `Union[str, bytes, Mapping[str, Any], None]` |
| `headers?`       | `Union[Mapping[str, Any], None]`             |
| `verify_secret?` | `Union[str, None, Omit]`                     |
| `tolerance?`     | `int`                                        |

**Returns**

`VerifyWebhookResult` — VerifyWebhookResult containing version, normalized payload, and raw payload :raises ValidationError: If `verify_secret` is empty, or is set but signature headers are missing :raises WebhookSignatureVerificationError: If signature verification fails :raises WebhookPayloadError: If the payload cannot be parsed

**Example**

```python
# Flask: verify the signature
@app.route('/webhooks/composio', methods=['POST'])
def webhook():
    try:
        result = composio.triggers.parse(
            request,
            verify_secret=os.environ['COMPOSIO_WEBHOOK_SECRET'],
        )
        print(f"Trigger: {result['payload']['trigger_slug']}")
        print(f"Event data: {result['payload']['payload']}")
        return 'OK', 200
    except exceptions.WebhookSignatureVerificationError:
        return 'Unauthorized', 401

# FastAPI: parse without verifying after reading the async body
@app.post('/webhooks/composio')
async def webhook(request: Request):
    raw = await request.body()
    result = composio.triggers.parse(body=raw, headers=request.headers)
    return {'trigger': result['payload']['trigger_slug']}
```

***

[View source](https://github.com/composiohq/composio/blob/next/python/composio/core/models/triggers.py#L936)

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

