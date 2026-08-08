# MCP (/reference/sdk-reference/typescript/mcp)

> **Deprecated**: Use a session's MCP endpoint instead: `composio.create(userId, \{ mcp: true \})`
returns a session that exposes `session.mcp.url` / `session.mcp.headers`. MCP is now
opt-in per session; the standalone `composio.mcp` server-management API is kept only for
backwards compatibility. Prefer the session MCP endpoint; do not generate new code against
`composio.mcp`. See [https://docs.composio.dev/docs/sessions-via-mcp](https://docs.composio.dev/docs/sessions-via-mcp)

# Usage [#usage]

Access this class through the `composio.mcp` property:

```typescript
const composio = new Composio({ apiKey: 'your-api-key' });
const result = await composio.mcp.list();
```

# Properties [#properties]

| Name     | Type       |
| -------- | ---------- |
| `client` | `Composio` |

# Methods [#methods]

## create() [#create]

Create a new MCP configuration.

```typescript
async create(name: string, mcpConfig: MCPConfigCreationParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                      |
| ----------------- | ------------------------- |
| `name`            | `string`                  |
| `mcpConfig`       | `MCPConfigCreationParams` |
| `requestOptions?` | `ComposioRequestOptions`  |

**Returns**

`Promise` — Created server details with instance getter

**Example**

```typescript
const server = await composio.mcpConfig.create("personal-mcp-server", {
  toolkits: ["github", "slack"],
  allowedTools: ["GMAIL_FETCH_EMAILS", "SLACK_SEND_MESSAGE"],
  manuallyManageConnections: false
 }
});

const server = await composio.mcpConfig.create("personal-mcp-server", {
  toolkits: [{ toolkit: "gmail", authConfigId: "ac_243434343" }],
  allowedTools: ["GMAIL_FETCH_EMAILS"],
  manuallyManageConnections: false
 }
});
```

***

## delete() [#delete]

Delete an MCP server configuration permanently

```typescript
async delete(serverId: string, requestOptions?: ComposioRequestOptions): Promise<{ id: string; deleted: boolean }>
```

**Parameters**

| Name              | Type                     | Description                                       |
| ----------------- | ------------------------ | ------------------------------------------------- |
| `serverId`        | `string`                 | The unique identifier of the MCP server to delete |
| `requestOptions?` | `ComposioRequestOptions` |                                                   |

**Returns**

`Promise<\{ id: string; deleted: boolean \}>` — Confirmation object with server ID and deletion status

**Example**

```typescript
// Delete an MCP server by ID
const result = await composio.experimental.mcp.delete("mcp_12345");

if (result.deleted) {
  console.log(`Server ${result.id} has been successfully deleted`);
} else {
  console.log(`Failed to delete server ${result.id}`);
}

// Example with error handling
try {
  const result = await composio.experimental.mcp.delete("mcp_12345");
  console.log("Deletion successful:", result);
} catch (error) {
  console.error("Failed to delete MCP server:", error.message);
}

// Delete and verify from list
await composio.experimental.mcp.delete("mcp_12345");
const servers = await composio.experimental.mcp.list({});
const serverExists = servers.items.some(server => server.id === "mcp_12345");
console.log("Server still exists:", serverExists); // Should be false
```

***

## generate() [#generate]

Get server URLs for an existing MCP server.
The response is wrapped according to the provider's specifications.

```typescript
async generate(userId: string, mcpConfigId: string, options?: MCPGetInstanceParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                                                    |
| ----------------- | ------------------------ | ------------------------------------------------------------------------------ |
| `userId`          | `string`                 | \{string} external user id from your database for whom you want the server for |
| `mcpConfigId`     | `string`                 | \{string} config id of the MCPConfig for which you want to create a server for |
| `options?`        | `MCPGetInstanceParams`   | \{object} additional options                                                   |
| `requestOptions?` | `ComposioRequestOptions` |                                                                                |

**Returns**

`Promise`

**Example**

```typescript
import { Composio } from "@composio/code";

const composio = new Composio();
const mcp = await composio.experimental.mcp.generate("default", "<mcp_config_id>");
```

***

## get() [#get]

Retrieve detailed information about a specific MCP server by its ID

```typescript
async get(serverId: string, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                         |
| ----------------- | ------------------------ | --------------------------------------------------- |
| `serverId`        | `string`                 | The unique identifier of the MCP server to retrieve |
| `requestOptions?` | `ComposioRequestOptions` |                                                     |

**Returns**

`Promise` — Complete MCP server details including configuration, tools, and metadata

**Example**

```typescript
// Get a specific MCP server by ID
const server = await composio.experimental.mcp.get("mcp_12345");

console.log(server.name); // "My Personal MCP Server"
console.log(server.allowedTools); // ["GITHUB_CREATE_ISSUE", "SLACK_SEND_MESSAGE"]
console.log(server.toolkits); // ["github", "slack"]
console.log(server.serverInstanceCount); // 3

// Access setup commands for different clients
console.log(server.commands.claude); // Claude setup command
console.log(server.commands.cursor); // Cursor setup command
console.log(server.commands.windsurf); // Windsurf setup command

// Use the MCP URL for direct connections
const mcpUrl = server.MCPUrl;
```

***

## list() [#list]

List the MCP servers with optional filtering and pagination

```typescript
async list(options: MCPListParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                      |
| ----------------- | ------------------------ | -------------------------------- |
| `options`         | `MCPListParams`          | Filtering and pagination options |
| `requestOptions?` | `ComposioRequestOptions` |                                  |

**Returns**

`Promise` — Paginated list of MCP servers with metadata

**Example**

```typescript
// List all MCP servers
const allServers = await composio.experimental.mcp.list({});

// List with pagination
const pagedServers = await composio.experimental.mcp.list({
  page: 2,
  limit: 5
});

// Filter by toolkit
const githubServers = await composio.experimental.mcp.list({
  toolkits: ['github', 'slack']
});

// Filter by name
const namedServers = await composio.experimental.mcp.list({
  name: 'personal'
});
```

***

## update() [#update]

Update an existing MCP server configuration with new settings

```typescript
async update(serverId: string, config: MCPUpdateParams, requestOptions?: ComposioRequestOptions): Promise
```

**Parameters**

| Name              | Type                     | Description                                       |
| ----------------- | ------------------------ | ------------------------------------------------- |
| `serverId`        | `string`                 | The unique identifier of the MCP server to update |
| `config`          | `MCPUpdateParams`        | Update configuration parameters                   |
| `requestOptions?` | `ComposioRequestOptions` |                                                   |

**Returns**

`Promise` — Updated MCP server configuration with all details

**Example**

```typescript
// Update server name only
const updatedServer = await composio.experimental.mcp.update("mcp_12345", {
  name: "My Updated MCP Server"
});

// Update toolkits and tools
const serverWithNewTools = await composio.experimental.mcp.update("mcp_12345", {
  toolkits: [
    {
      toolkit: "github",
      authConfigId: "auth_abc123",
      allowedTools: ["GITHUB_CREATE_ISSUE", "GITHUB_LIST_REPOS"]
    },
    {
      toolkit: "slack",
      authConfigId: "auth_xyz789",
      allowedTools: ["SLACK_SEND_MESSAGE", "SLACK_LIST_CHANNELS"]
    }
  ]
});

// Update connection management setting
const serverWithManualAuth = await composio.experimental.mcp.update("mcp_12345", {
  name: "Manual Auth Server",
  manuallyManageConnections: true
});

// Complete update example
const fullyUpdatedServer = await composio.experimental.mcp.update("mcp_12345", {
  name: "Production MCP Server",
  toolkits: [
    {
      toolkit: "gmail",
      authConfigId: "auth_gmail_prod",
    }
  ],
  allowedTools: ["GMAIL_SEND_EMAIL", "GMAIL_FETCH_EMAILS"]
  manuallyManageConnections: false
});

console.log("Updated server:", fullyUpdatedServer.name);
console.log("New tools:", fullyUpdatedServer.allowedTools);
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

