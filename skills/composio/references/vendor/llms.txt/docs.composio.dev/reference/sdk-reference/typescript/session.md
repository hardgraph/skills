# Session (/reference/sdk-reference/typescript/session)

# Properties [#properties]

| Name            | Type                                                                         | Description                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `configVersion` | `number`                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                    |
| `experimental`  | `SessionExperimental`                                                        |                                                                                                                                                                                                                                                                                                                                                                                                    |
| `mcp`           | `\{ headers?: Record<string, string>; type: 'http' \| 'sse'; url: string \}` | Hosted MCP endpoint (`session.mcp.url` / `session.mcp.headers`). Exists on every session at runtime, but only surfaced in the type when the session is created with `\{ mcp: true \}` (which returns `Session`); the default `SessionWithoutMcp` omits `mcp`, so MCP is an explicit opt-in. See [https://docs.composio.dev/docs/sessions-via-mcp](https://docs.composio.dev/docs/sessions-via-mcp) |
| `preload`       | `Preload`                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                    |
| `sandbox`       | `Workbench`                                                                  | Resolved sandbox (code-execution) config returned by the API. `enable` defaults to `true` server-side.                                                                                                                                                                                                                                                                                             |
| `sessionId`     | `string`                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                    |
| `warnings`      | `Warning[]`                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                    |

# Methods [#methods]

## authorize() [#authorize]

Initiate an authorization flow for a toolkit.
Returns a ConnectionRequest with a redirect URL for the user.

Pass `experimental: { accountType: 'SHARED', aclConfigForShared }` to
create a SHARED connection with a per-user ACL in one flow. Default
behaviour (omit the block) creates a PRIVATE connection.

Experimental — shape may change in future releases.

`aclConfigForShared` is validated against the same caps as
`composio.connectedAccounts.link()` (≤1000 entries per list, each
`userId` 1..256 characters). Invalid input throws `ValidationError`
at the SDK boundary.

```typescript
async authorize(toolkit: string, options?: { callbackUrl?: string; alias?: string; experimental?: { accountType?: ConnectedAccountType; aclConfigForShared?: ConnectedAccountAclConfig; }; }, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                                                                                                                                                    |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `toolkit`         | `string`                                                                                                                                                |
| `options?`        | `\{ callbackUrl?: string; alias?: string; experimental?: \{ accountType?: ConnectedAccountType; aclConfigForShared?: ConnectedAccountAclConfig; \}; \}` |
| `requestOptions?` | `ComposioRequestOptions`                                                                                                                                |

**Returns**

`Promise`

***

## customToolkits() [#customtoolkits]

List all custom toolkits registered in this session.
Returns toolkits with their tools showing final slugs.

```typescript
customToolkits(): RegisteredCustomToolkit[]
```

**Returns**

`RegisteredCustomToolkit[]` — Array of registered custom toolkits

***

## customTools() [#customtools]

List all custom tools registered in this session.
Returns tools with their final slugs, schemas, and resolved toolkit.

```typescript
customTools(options?: { toolkit?: string }): RegisteredCustomTool[]
```

**Parameters**

| Name       | Type                     |
| ---------- | ------------------------ |
| `options?` | `\{ toolkit?: string \}` |

**Returns**

`RegisteredCustomTool[]` — Array of registered custom tools

***

## delete() [#delete]

Delete this session.

Deleted sessions immediately stop being retrievable or executable. Deleting
an already-deleted session surfaces the backend 404.

```typescript
async delete(requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     |
| ----------------- | ------------------------ |
| `requestOptions?` | `ComposioRequestOptions` |

**Returns**

`Promise`

***

## execute() [#execute]

Execute a tool within the session.

For custom tools, accepts the original slug (e.g. "GREP") or the
full slug (e.g. "LOCAL\_GREP"). Custom tools are executed in-process;
remote tools are sent to the Composio backend.

```typescript
async execute(toolSlug: string, arguments_?: Record<string, unknown>, options?: ToolRouterSessionExecuteOptions, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                              | Description                |
| ----------------- | --------------------------------- | -------------------------- |
| `toolSlug`        | `string`                          | The tool slug to execute   |
| `arguments_?`     | `Record<string, unknown>`         | Optional tool arguments    |
| `options?`        | `ToolRouterSessionExecuteOptions` | Optional execution options |
| `requestOptions?` | `ComposioRequestOptions`          |                            |

**Returns**

`Promise` — The tool execution result

***

## proxyExecute() [#proxyexecute]

Proxy an API call through Composio's auth layer using the session's connected account.
The backend resolves the connected account from the toolkit within the session.

```typescript
async proxyExecute(params: SessionProxyExecuteParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                        | Description                                                                      |
| ----------------- | --------------------------- | -------------------------------------------------------------------------------- |
| `params`          | `SessionProxyExecuteParams` | Proxy request parameters (toolkit, endpoint, method, body, headers/query params) |
| `requestOptions?` | `ComposioRequestOptions`    |                                                                                  |

**Returns**

`Promise` — The proxied API response with status, data, headers

***

## search() [#search]

Search for tools by semantic use case.
Returns relevant tools for the given query with schemas and guidance.

```typescript
async search(params: { query: string; toolkits?: string[]; }, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                                        |
| ----------------- | ------------------------------------------- |
| `params`          | `\{ query: string; toolkits?: string[]; \}` |
| `requestOptions?` | `ComposioRequestOptions`                    |

**Returns**

`Promise`

***

## toolkits() [#toolkits]

Query the connection state of toolkits in the session.
Supports pagination and filtering by toolkit slugs.

```typescript
async toolkits(options?: ToolRouterToolkitsOptions, requestOptions?: ComposioRequestOptions): Promise<{ cursor: string | undefined; items: { connection?: { authConfig?: ... | ...; connectedAccount?: { id: ...; status: ... }; isActive: boolean }; isNoAuth: boolean; logo?: string; name: string; slug: string }[]; totalPages: number }>
```

**Parameters**

| Name              | Type                        |
| ----------------- | --------------------------- |
| `options?`        | `ToolRouterToolkitsOptions` |
| `requestOptions?` | `ComposioRequestOptions`    |

**Returns**

`Promise<\{ cursor: string \| undefined; items: \{ connection?: \{ authConfig?: ... \| ...; connectedAccount?: \{ id: ...; status: ... \}; isActive: boolean \}; isNoAuth: boolean; logo?: string; name: string; slug: string \}[]; totalPages: number \}>`

***

## tools() [#tools]

Get the tools available in the session, formatted for your AI framework.
Requires a provider to be configured in the Composio constructor.

When custom tools are bound to the session, execution of COMPOSIO\_MULTI\_EXECUTE\_TOOL
is intercepted: local tools are executed in-process, remote tools are sent to the backend.

```typescript
async tools(modifiers?: SessionMetaToolOptions, requestOptions?: ComposioRequestOptions): Promise>
```

**Parameters**

| Name              | Type                     |
| ----------------- | ------------------------ |
| `modifiers?`      | `SessionMetaToolOptions` |
| `requestOptions?` | `ComposioRequestOptions` |

**Returns**

`Promise>`

***

## update() [#update]

Partially update the session configuration.
Only the fields provided will be changed; omitted fields are preserved.
Mutates this session's `configVersion`, `preload`, and `warnings` in-place.

```typescript
async update(config: ToolRouterUpdateSessionConfig, requestOptions?: ComposioRequestOptions): Promise<void>
```

**Parameters**

| Name              | Type                            |
| ----------------- | ------------------------------- |
| `config`          | `ToolRouterUpdateSessionConfig` |
| `requestOptions?` | `ComposioRequestOptions`        |

**Returns**

`Promise<void>`

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

