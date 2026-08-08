# Sessions (/reference/sdk-reference/typescript/sessions)

# Usage [#usage]

Access this class through the `composio.sessions` property:

```typescript
const composio = new Composio({ apiKey: 'your-api-key' });
const result = await composio.sessions.list();
```

# Methods [#methods]

## create() [#create]

Creates a new tool router session for a user.
Use `sessionPreset: SessionPreset.DIRECT_TOOLS` when all needed tools
should be exposed directly; see `ToolRouterCreateSessionConfig`.

**Overload 1**

```typescript
async create(userId: string, config: ToolRouterCreateSessionConfig & { mcp: true }, requestOptions?: ComposioRequestOptions): Promise>
```

**Parameters**

| Name              | Type                                              | Description                                                             |
| ----------------- | ------------------------------------------------- | ----------------------------------------------------------------------- |
| `userId`          | `string`                                          | \{string} The user id to create the session for                         |
| `config`          | `ToolRouterCreateSessionConfig & \{ mcp: true \}` | \{ToolRouterCreateSessionConfig} The config for the tool router session |
| `requestOptions?` | `ComposioRequestOptions`                          |                                                                         |

**Returns**

`Promise>` — The tool router session

**Overload 2**

```typescript
async create(userId: string, config?: ToolRouterCreateSessionConfig, requestOptions?: ComposioRequestOptions): Promise>
```

**Parameters**

| Name              | Type                            | Description                                                             |
| ----------------- | ------------------------------- | ----------------------------------------------------------------------- |
| `userId`          | `string`                        | \{string} The user id to create the session for                         |
| `config?`         | `ToolRouterCreateSessionConfig` | \{ToolRouterCreateSessionConfig} The config for the tool router session |
| `requestOptions?` | `ComposioRequestOptions`        |                                                                         |

**Returns**

`Promise>` — The tool router session

**Example**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();

const session = await composio.sessions.create('user_123', {
  toolkits: ['gmail'],
  manageConnections: true,
  experimental: {
    customTools: [myCustomTool],
    customToolkits: [myToolkit],
  },
});
```

***

## delete() [#delete]

Delete a tool router session by ID.

Deleted sessions immediately stop being retrievable or executable. Deleting
a missing or already-deleted session surfaces the backend 404.

```typescript
async delete(id: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     |
| ----------------- | ------------------------ |
| `id`              | `string`                 |
| `requestOptions?` | `ComposioRequestOptions` |

**Returns**

`Promise`

***

## use() [#use]

Use an existing session

**Overload 1**

```typescript
async use(id: string, options: { customTools?: CustomTool[]; customToolkits?: CustomToolkit[]; mcp: true }, requestOptions?: ComposioRequestOptions): Promise>
```

**Parameters**

| Name              | Type                                                                            | Description                            |
| ----------------- | ------------------------------------------------------------------------------- | -------------------------------------- |
| `id`              | `string`                                                                        | \{string} The id of the session to use |
| `options`         | `\{ customTools?: CustomTool[]; customToolkits?: CustomToolkit[]; mcp: true \}` |                                        |
| `requestOptions?` | `ComposioRequestOptions`                                                        |                                        |

**Returns**

`Promise>` — The tool router session

**Overload 2**

```typescript
async use(id: string, options?: { customTools?: CustomTool[]; customToolkits?: CustomToolkit[]; mcp?: boolean }, requestOptions?: ComposioRequestOptions): Promise>
```

**Parameters**

| Name              | Type                                                                                | Description                            |
| ----------------- | ----------------------------------------------------------------------------------- | -------------------------------------- |
| `id`              | `string`                                                                            | \{string} The id of the session to use |
| `options?`        | `\{ customTools?: CustomTool[]; customToolkits?: CustomToolkit[]; mcp?: boolean \}` |                                        |
| `requestOptions?` | `ComposioRequestOptions`                                                            |                                        |

**Returns**

`Promise>` — The tool router session

**Example**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const id = 'session_123';
const session = await composio.sessions.use(id);

console.log(session.mcp.url);
console.log(session.mcp.headers);
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

