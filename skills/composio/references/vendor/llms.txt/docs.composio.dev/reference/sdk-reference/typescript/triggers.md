# Triggers (/reference/sdk-reference/typescript/triggers)

# Usage [#usage]

Access this class through the `composio.triggers` property:

```typescript
const composio = new Composio({ apiKey: 'your-api-key' });
const result = await composio.triggers.list();
```

# Methods [#methods]

## create() [#create]

Create a new trigger instance for a user
If the connected account id is not provided, the first connected account for the user and toolkit will be used

```typescript
async create(userId: string, slug: string, body?: TriggerInstanceUpsertParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                          | Description                                   |
| ----------------- | ----------------------------- | --------------------------------------------- |
| `userId`          | `string`                      | The user id of the trigger instance           |
| `slug`            | `string`                      | The slug of the trigger instance              |
| `body?`           | `TriggerInstanceUpsertParams` | The parameters to create the trigger instance |
| `requestOptions?` | `ComposioRequestOptions`      |                                               |

**Returns**

`Promise` — The created trigger instance

***

## delete() [#delete]

Delete a trigger instance

```typescript
async delete(triggerId: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                      |
| ----------------- | ------------------------ | -------------------------------- |
| `triggerId`       | `string`                 | The slug of the trigger instance |
| `requestOptions?` | `ComposioRequestOptions` |                                  |

**Returns**

`Promise`

***

## disable() [#disable]

Disable a trigger instance

```typescript
async disable(triggerId: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                    |
| ----------------- | ------------------------ | ------------------------------ |
| `triggerId`       | `string`                 | The id of the trigger instance |
| `requestOptions?` | `ComposioRequestOptions` |                                |

**Returns**

`Promise` — The updated trigger instance

***

## enable() [#enable]

Enable a trigger instance

```typescript
async enable(triggerId: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                    |
| ----------------- | ------------------------ | ------------------------------ |
| `triggerId`       | `string`                 | The id of the trigger instance |
| `requestOptions?` | `ComposioRequestOptions` |                                |

**Returns**

`Promise` — The updated trigger instance

***

## getType() [#gettype]

Retrieve a trigger type by its slug for the provided version of the app
Use the global toolkit versions param when initializing composio to pass a toolkitversion

```typescript
async getType(slug: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                  |
| ----------------- | ------------------------ | ---------------------------- |
| `slug`            | `string`                 | The slug of the trigger type |
| `requestOptions?` | `ComposioRequestOptions` |                              |

**Returns**

`Promise` — The trigger type object

***

## listActive() [#listactive]

Fetch list of all the active triggers

```typescript
async listActive(query?: TriggerInstanceListActiveParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                              | Description                                          |
| ----------------- | --------------------------------- | ---------------------------------------------------- |
| `query?`          | `TriggerInstanceListActiveParams` | The query parameters to filter the trigger instances |
| `requestOptions?` | `ComposioRequestOptions`          |                                                      |

**Returns**

`Promise` — List of trigger instances

**Example**

```typescript
const triggers = await triggers.listActive({
  authConfigIds: ['123'],
  connectedAccountIds: ['456'],
});
```

***

## listEnum() [#listenum]

Fetches the list of all the available trigger enums

This method is used by the CLI where filters are not required.

```typescript
async listEnum(requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     |
| ----------------- | ------------------------ |
| `requestOptions?` | `ComposioRequestOptions` |

**Returns**

`Promise`

***

## listTypes() [#listtypes]

List all the trigger types

```typescript
async listTypes(query?: TriggersTypeListParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                      |
| ----------------- | ------------------------ | ------------------------------------------------ |
| `query?`          | `TriggersTypeListParams` | The query parameters to filter the trigger types |
| `requestOptions?` | `ComposioRequestOptions` |                                                  |

**Returns**

`Promise` — The list of trigger types

***

## parse() [#parse]

Parse an incoming webhook HTTP request into a typed, normalized trigger payload.

Dump the incoming request in and get back the parsed Composio trigger event.
When `verifySecret` is provided, the request signature is verified before the
payload is returned (delegating to verifyWebhook); without it, the body
is parsed without verification.

The `request` may be either a Fetch API `Request` (Next.js App Router, Hono,
Remix) or a plain `{ body, headers }` object (Express with `express.raw`,
Next.js Pages Router `req`). The signature headers (`webhook-id`,
`webhook-timestamp`, `webhook-signature`) are read case-insensitively.

```typescript
async parse(request: WebhookRequestLike, options?: ParseWebhookOptions): Promise
```

**Parameters**

| Name       | Type                  | Description                       |
| ---------- | --------------------- | --------------------------------- |
| `request`  | `WebhookRequestLike`  | The incoming webhook HTTP request |
| `options?` | `ParseWebhookOptions` | Parse options                     |

**Returns**

`Promise` — The parsed (and optionally verified) webhook payload

**Example**

```typescript
// Express with express.raw (verify the signature)
app.post('/webhooks/composio', express.raw({ type: 'application/json' }), async (req, res) => {
  try {
    const result = await composio.triggers.parse(req, {
      verifySecret: process.env.COMPOSIO_WEBHOOK_SECRET,
    });
    console.log('Trigger:', result.payload.triggerSlug);
    console.log('Event data:', result.payload.payload);
    res.sendStatus(200);
  } catch (error) {
    res.sendStatus(401);
  }
});

// Express without verifying (parse only)
app.post('/webhooks/composio', express.raw({ type: 'application/json' }), async (req, res) => {
  const result = await composio.triggers.parse(req);
  console.log('Trigger:', result.payload.triggerSlug);
  res.sendStatus(200);
});
```

```typescript
// Next.js App Router (Request) — verify the signature
export async function POST(request: Request) {
  try {
    const result = await composio.triggers.parse(request, {
      verifySecret: process.env.COMPOSIO_WEBHOOK_SECRET,
    });
    console.log('Trigger:', result.payload.triggerSlug);
    console.log('Event data:', result.payload.payload);
    return new Response('OK', { status: 200 });
  } catch (error) {
    return new Response('Unauthorized', { status: 401 });
  }
}

// Next.js App Router — parse only (no verification)
export async function POST(request: Request) {
  const result = await composio.triggers.parse(request);
  console.log('Trigger:', result.payload.triggerSlug);
  return new Response('OK', { status: 200 });
}
```

***

## setWebhookSubscription() [#setwebhooksubscription]

Create or update the project webhook subscription used for webhook delivery.

If a subscription already exists, the first subscription is updated. Otherwise a new
subscription is created. By default this subscribes to V3 trigger message events.

```typescript
async setWebhookSubscription(params: SetWebhookSubscriptionParams): Promise
```

**Parameters**

| Name     | Type                           |
| -------- | ------------------------------ |
| `params` | `SetWebhookSubscriptionParams` |

**Returns**

`Promise`

**Example**

```typescript
await composio.triggers.setWebhookSubscription({
  webhookUrl: `${APP_URL}/webhooks/composio`,
});
```

***

## subscribe() [#subscribe]

Subscribe to all the triggers

```typescript
async subscribe(fn: (_data: IncomingTriggerPayload) => void, filters: TriggerSubscribeParams): Promise<void>
```

**Parameters**

| Name      | Type                                      | Description                                     |
| --------- | ----------------------------------------- | ----------------------------------------------- |
| `fn`      | `(_data: IncomingTriggerPayload) => void` | The function to call when a trigger is received |
| `filters` | `TriggerSubscribeParams`                  | The filters to apply to the triggers            |

**Returns**

`Promise<void>`

**Example**

```typescript
triggers.subscribe((data) => {
  console.log(data);
}, );
```

***

## unsubscribe() [#unsubscribe]

Unsubscribe from all the triggers

```typescript
async unsubscribe(): Promise<void>
```

**Returns**

`Promise<void>`

**Example**

```typescript
composio.trigger.subscribe((data) => {
  console.log(data);
});

await triggers.unsubscribe();
```

***

## update() [#update]

Update an existing trigger instance

```typescript
async update(triggerId: string, body: TriggerInstanceManageUpdateParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                                | Description                                   |
| ----------------- | ----------------------------------- | --------------------------------------------- |
| `triggerId`       | `string`                            | The Id of the trigger instance                |
| `body`            | `TriggerInstanceManageUpdateParams` | The parameters to update the trigger instance |
| `requestOptions?` | `ComposioRequestOptions`            |                                               |

**Returns**

`Promise` — The updated trigger instance response

***

## verifyWebhook() [#verifywebhook]

Verify an incoming webhook payload and signature.

This method validates that the webhook request is authentic by:

1. Verifying the HMAC-SHA256 signature matches the payload using the correct signing format
2. Optionally checking that the webhook timestamp is within the tolerance window

The signature is computed as: `HMAC-SHA256(${webhookId}.${webhookTimestamp}.${payload}, secret)`
and is expected in the format: `v1,base64EncodedSignature`

```typescript
async verifyWebhook(params: VerifyWebhookParams): Promise
```

**Parameters**

| Name     | Type                  | Description                 |
| -------- | --------------------- | --------------------------- |
| `params` | `VerifyWebhookParams` | The verification parameters |

**Returns**

`Promise` — The verified and parsed webhook payload with version information

**Example**

```typescript
// In an Express.js webhook handler
app.post('/webhook', express.raw({ type: 'application/json' }), async (req, res) => {
  try {
    const result = await composio.triggers.verifyWebhook({
      payload: req.body.toString(),
      signature: req.headers['webhook-signature'] as string,
      webhookId: req.headers['webhook-id'] as string,
      webhookTimestamp: req.headers['webhook-timestamp'] as string,
      secret: process.env.COMPOSIO_WEBHOOK_SECRET!,
    });

    // Process the verified payload
    console.log('Webhook version:', result.version);
    console.log('Received trigger:', result.payload.triggerSlug);
    res.status(200).send('OK');
  } catch (error) {
    console.error('Webhook verification failed:', error);
    res.status(401).send('Unauthorized');
  }
});
```

***

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

