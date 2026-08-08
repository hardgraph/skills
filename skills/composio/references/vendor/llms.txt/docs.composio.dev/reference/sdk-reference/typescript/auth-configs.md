# AuthConfigs (/reference/sdk-reference/typescript/auth-configs)

# Usage [#usage]

Access this class through the `composio.authConfigs` property:

```typescript
const composio = new Composio({ apiKey: 'your-api-key' });
const result = await composio.authConfigs.list();
```

# Methods [#methods]

## create() [#create]

Create a new auth config

```typescript
async create(toolkit: string, options: CreateAuthConfigParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                            |
| ----------------- | ------------------------ | -------------------------------------- |
| `toolkit`         | `string`                 | Unique identifier of the toolkit       |
| `options`         | `CreateAuthConfigParams` | Options for creating a new auth config |
| `requestOptions?` | `ComposioRequestOptions` |                                        |

**Returns**

`Promise` — Created auth config

**Example**

```typescript
const authConfig = await authConfigs.create('my-toolkit', {
  type: AuthConfigTypes.CUSTOM,
  name: 'My Custom Auth Config',
  authScheme: AuthSchemeTypes.API_KEY,
  credentials: {
    apiKey: '1234567890',
  },
});
```

***

## delete() [#delete]

Deletes an authentication configuration.

This method permanently removes an auth config from the Composio platform.
This action cannot be undone and will prevent any connected accounts that use
this auth config from functioning.

```typescript
async delete(nanoid: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                        |
| ----------------- | ------------------------ | -------------------------------------------------- |
| `nanoid`          | `string`                 | The unique identifier of the auth config to delete |
| `requestOptions?` | `ComposioRequestOptions` |                                                    |

**Returns**

`Promise` — The deletion response

**Example**

```typescript
// Delete an auth config
await composio.authConfigs.delete('auth_abc123');
```

***

## disable() [#disable]

Disables an authentication configuration.

This is a convenience method that calls updateStatus with 'DISABLED'.
When disabled, the auth config cannot be used to create new connected accounts
or authenticate with third-party services, but existing connections may continue to work.

```typescript
async disable(nanoid: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                         |
| ----------------- | ------------------------ | --------------------------------------------------- |
| `nanoid`          | `string`                 | The unique identifier of the auth config to disable |
| `requestOptions?` | `ComposioRequestOptions` |                                                     |

**Returns**

`Promise` — The updated auth config details

**Example**

```typescript
// Disable an auth config
await composio.authConfigs.disable('auth_abc123');
```

***

## enable() [#enable]

Enables an authentication configuration.

This is a convenience method that calls updateStatus with 'ENABLED'.
When enabled, the auth config can be used to create new connected accounts
and authenticate with third-party services.

```typescript
async enable(nanoid: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                        |
| ----------------- | ------------------------ | -------------------------------------------------- |
| `nanoid`          | `string`                 | The unique identifier of the auth config to enable |
| `requestOptions?` | `ComposioRequestOptions` |                                                    |

**Returns**

`Promise` — The updated auth config details

**Example**

```typescript
// Enable an auth config
await composio.authConfigs.enable('auth_abc123');
```

***

## get() [#get]

Retrieves a specific authentication configuration by its ID.

This method fetches detailed information about a single auth config
and transforms the response to the SDK's standardized format.

```typescript
async get(nanoid: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                          |
| ----------------- | ------------------------ | ---------------------------------------------------- |
| `nanoid`          | `string`                 | The unique identifier of the auth config to retrieve |
| `requestOptions?` | `ComposioRequestOptions` |                                                      |

**Returns**

`Promise` — The auth config details

**Example**

```typescript
// Get an auth config by ID
const authConfig = await composio.authConfigs.get('auth_abc123');
console.log(authConfig.name); // e.g., 'GitHub Auth'
console.log(authConfig.toolkit.slug); // e.g., 'github'
```

***

## list() [#list]

Lists authentication configurations based on provided filter criteria.

This method retrieves auth configs from the Composio API, transforms them to the SDK format,
and supports filtering by various parameters.

```typescript
async list(query?: AuthConfigListParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                          |
| ----------------- | ------------------------ | ---------------------------------------------------- |
| `query?`          | `AuthConfigListParams`   | Optional query parameters for filtering auth configs |
| `requestOptions?` | `ComposioRequestOptions` |                                                      |

**Returns**

`Promise` — A paginated list of auth configurations

**Example**

```typescript
// List all auth configs
const allConfigs = await composio.authConfigs.list();

// List auth configs for a specific toolkit
const githubConfigs = await composio.authConfigs.list({
  toolkit: 'github'
});

// Search auth configs by name or id
const searchedConfigs = await composio.authConfigs.list({
  search: 'github',
  showDisabled: true
});

// List Composio-managed auth configs
const managedConfigs = await composio.authConfigs.list({
  isComposioManaged: true
});
```

***

## update() [#update]

Updates an existing authentication configuration.

This method allows you to modify properties of an auth config such as credentials,
scopes, or tool restrictions. The update type (custom or default) determines which
fields can be updated.

```typescript
async update(nanoid: string, data: AuthConfigUpdateParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                                    |
| ----------------- | ------------------------ | -------------------------------------------------------------- |
| `nanoid`          | `string`                 | The unique identifier of the auth config to update             |
| `data`            | `AuthConfigUpdateParams` | The data to update, which can be either custom or default type |
| `requestOptions?` | `ComposioRequestOptions` |                                                                |

**Returns**

`Promise` — The updated auth config

**Example**

```typescript
// Update a custom auth config with new credentials
const updatedConfig = await composio.authConfigs.update('auth_abc123', {
  type: 'custom',
  credentials: {
    apiKey: 'new-api-key-value'
  }
});

// Update a default auth config with new scopes
const updatedConfig = await composio.authConfigs.update('auth_abc123', {
  type: 'default',
  scopes: ['read:user', 'repo']
});
```

***

## updateStatus() [#updatestatus]

Updates the status of an authentication configuration.

This method allows you to enable or disable an auth config. When disabled,
the auth config cannot be used to create new connected accounts or authenticate
with third-party services.

```typescript
async updateStatus(status: 'ENABLED' | 'DISABLED', nanoid: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                      | Description                                 |
| ----------------- | ------------------------- | ------------------------------------------- |
| `status`          | `'ENABLED' \| 'DISABLED'` | The status to set ('ENABLED' or 'DISABLED') |
| `nanoid`          | `string`                  | The unique identifier of the auth config    |
| `requestOptions?` | `ComposioRequestOptions`  |                                             |

**Returns**

`Promise` — The updated auth config details

**Example**

```typescript
// Disable an auth config
await composio.authConfigs.updateStatus('DISABLED', 'auth_abc123');

// Enable an auth config
await composio.authConfigs.updateStatus('ENABLED', 'auth_abc123');
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

