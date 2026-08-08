# Composio (/reference/sdk-reference/typescript/composio)

# Constructor [#constructor]

## constructor() [#constructor-1]

Creates a new instance of the Composio SDK.

The constructor initializes the SDK with the provided configuration options,
sets up the API client, and initializes all core models (tools, toolkits, etc.).

```typescript
constructor(config?: ComposioConfig): Composio
```

**Parameters**

| Name      | Type             | Description                                |
| --------- | ---------------- | ------------------------------------------ |
| `config?` | `ComposioConfig` | Configuration options for the Composio SDK |

**Returns**

`Composio`

**Example**

```typescript
// Initialize with default configuration
const composio = new Composio();

// Initialize with custom API key and base URL
const composio = new Composio({
  apiKey: 'your-api-key',
  baseURL: 'https://api.composio.dev'
});

// Initialize with custom provider
const composio = new Composio({
  apiKey: 'your-api-key',
  provider: new CustomProvider()
});
```

***

# Properties [#properties]

| Name                                                                   | Type                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Description                                                                      |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `authConfigs`                                                          | `AuthConfigs`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Manage authentication configurations for toolkits                                |
| `connectedAccounts`                                                    | `ConnectedAccounts`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Manage authenticated connections                                                 |
| `create`                                                               | `(userId: string, config: \{ authConfigs?: Record<string, string>; connectedAccounts?: Record<string, string \| ...[]>; experimental?: \{ assistivePrompt?: \{ userTimezone?: string \}; customToolkits?: CustomToolkit[]; customTools?: CustomTool[] \}; manageConnections?: boolean \| \{ callbackUrl?: string; enable?: boolean; waitForConnections?: boolean \}; mcp?: boolean; multiAccount?: \{ enable: boolean; maxAccountsPerToolkit?: number; requireExplicitSelection?: boolean \}; preload?: \{ tools?: ...[] \| 'all' \}; sandbox?: \{ autoOffloadThreshold?: number; enable: boolean; enableProxyExecution?: boolean; sandboxSize?: 'standard' \| 'medium' \| 'large' \| 'xlarge' \}; sessionPreset?: 'direct_tools'; tags?: ... \| ... \| ... \| ...[] \| \{ disable?: ...[]; enable?: ...[] \}; toolkits?: string[] \| \{ disable: ...[] \} \| \{ enable: ...[] \}; tools?: Record<string, ...[] \| \{ enable: ... \} \| \{ disable: ... \} \| \{ tags: ... \}>; workbench?: \{ autoOffloadThreshold?: number; enable: boolean; enableProxyExecution?: boolean; sandboxSize?: 'standard' \| 'medium' \| 'large' \| 'xlarge' \} \} & \{ mcp: true \}, requestOptions: ComposioRequestOptions) => Promise` | Creates a new tool router session for a user.                                    |
| Use `sessionPreset: SessionPreset.DIRECT_TOOLS` when all needed tools  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| should be exposed directly; see `ToolRouterCreateSessionConfig`.       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| `experimental`                                                         | `Experimental`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Experimental SDK methods whose shape may change in future releases.              |
| Prefer domain-specific mounts (for example                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| `composio.connectedAccounts.updateAcl(...)`) when available; this      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| namespace keeps compatibility aliases while APIs are experimental.     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| Stateless experimental factories (e.g. `experimental_createTool`) stay |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| at the top level.                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                  |
| `files`                                                                | `Files`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Upload and download files                                                        |
| `mcp`                                                                  | `MCP`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Model Context Protocol server management.                                        |
| `provider`                                                             | `TProvider`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | The tool provider instance used for wrapping tools in framework-specific formats |
| `sessions`                                                             | `Sessions`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Create and reuse Composio sessions.                                              |

Prefer `composio.sessions.create(...)` for new code. The top-level
`composio.create(...)` method is kept as an alias. |
\| `toolkits` | `Toolkits` | Retrieve toolkit metadata and authorize user connections |
\| `toolRouter` | `ToolRouter` | Legacy alias for `composio.sessions`. |
\| `tools` | `Tools` | List, retrieve, and execute tools |
\| `triggers` | `Triggers` | Manage webhook triggers and event subscriptions |
\| `use` | `(id: string, options: \{ customToolkits?: CustomToolkit[]; customTools?: CustomTool[]; mcp: true \}, requestOptions: ComposioRequestOptions) => Promise` | Use an existing tool router session |

# Methods [#methods]

## createSession() (deprecated) [#createsession-deprecated]

> **Deprecated**: Will be removed in a future version of the SDK. Instead, construct a new
instance directly with the headers you need: `new Composio(\{ ...existingConfig, defaultHeaders \})`.
For one-off overrides, pass per-call `requestOptions` where supported.

Creates a new instance of the Composio SDK with custom request options while preserving the existing configuration.
This method is particularly useful when you need to:

* Add custom headers for specific requests
* Track request contexts with unique identifiers
* Override default request behavior for a subset of operations

The new instance inherits all configuration from the parent instance (apiKey, baseURL, provider, etc.)
but allows you to specify custom request options that will be used for all API calls made through this session.

```typescript
createSession(options?: { headers?: ComposioRequestHeaders }): Composio
```

**Parameters**

| Name       | Type                                     |
| ---------- | ---------------------------------------- |
| `options?` | `\{ headers?: ComposioRequestHeaders \}` |

**Returns**

`Composio` — A new Composio instance with the custom request options applied.

**Example**

```typescript
// Create a base Composio instance
const composio = new Composio({
  apiKey: 'your-api-key'
});

// Create a session with request tracking headers
const composioWithCustomHeaders = composio.createSession({
  headers: {
    'x-request-id': '1234567890',
    'x-correlation-id': 'session-abc-123',
    'x-custom-header': 'custom-value'
  }
});

// Use the session for making API calls with the custom headers
await composioWithCustomHeaders.tools.list();
```

***

## flush() [#flush]

Flush any pending telemetry and wait for it to complete.

In Node.js-compatible environments, telemetry is automatically flushed on process exit.
However, in environments like Cloudflare Workers that don't support process exit events,
you should call this method manually to ensure all telemetry is sent.

```typescript
async flush(): Promise<void>
```

**Returns**

`Promise<void>` — A promise that resolves when all pending telemetry has been sent.

**Example**

```typescript
// In a Cloudflare Worker, use ctx.waitUntil to ensure telemetry is flushed
export default {
  async fetch(request: Request, env: Env, ctx: ExecutionContext) {
    const composio = new Composio({ apiKey: env.COMPOSIO_API_KEY });

    // Do your work...
    const result = await composio.tools.execute(...);

    // Ensure telemetry flushes before worker terminates
    ctx.waitUntil(composio.flush());

    return new Response(JSON.stringify(result));
  }
};
```

***

## getClient() [#getclient]

Get the Composio SDK client.

```typescript
getClient(): ComposioClient
```

**Returns**

`ComposioClient` — The Composio API client.

***

## getConfig() [#getconfig]

Get the configuration SDK is initialized with.

Returns a frozen shallow clone — the SDK has already snapshotted
configuration values such as `dangerouslyAllowAutoUploadDownloadFiles`,
`fileUploadDirs`, and `fileDownloadDir` into its internal models, so
mutating the live config object would silently no-op. Freezing makes
that contract visible at the call site instead of letting the mutation
appear successful.

```typescript
getConfig(): Readonly
```

**Returns**

`Readonly` — The frozen configuration
the SDK is initialized with.

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

