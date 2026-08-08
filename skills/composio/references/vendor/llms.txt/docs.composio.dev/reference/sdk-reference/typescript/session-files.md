# Session files (/reference/sdk-reference/typescript/session-files)

# Methods [#methods]

## delete() [#delete]

Deletes a file or directory at the specified path on the session's file mount.

Removes the file or directory from the virtual filesystem. Use with caution:
deletion is typically irreversible. Ensure the path exists and is intended for removal.

```typescript
async delete(remotePath: string, options?: ToolRouterSessionFilesMountDeleteOptions): Promise
```

**Parameters**

| Name         | Type                                       | Description                                               |
| ------------ | ------------------------------------------ | --------------------------------------------------------- |
| `remotePath` | `string`                                   | The path of the file or directory to delete on the mount. |
| `options?`   | `ToolRouterSessionFilesMountDeleteOptions` | Optional configuration for the delete operation.          |

**Returns**

`Promise` — Confirmation of deletion (implementation-specific).

**Example**

```typescript
const session = await composio.toolRouter.use('session_123');
await session.experimental.files.delete('/temp/cache.json');
```

```typescript
// Delete from a custom mount
await session.experimental.files.delete('/old-backup', {
  mountId: 'custom-mount',
});
```

***

## download() [#download]

Downloads a file from the session's file mount to the local filesystem.

Retrieves a file stored in the session's virtual filesystem (e.g., one produced
by a tool or previously uploaded) and saves it to the specified local path.

```typescript
async download(filePath: string, options?: ToolRouterSessionFilesMountDownloadOptions): Promise
```

**Parameters**

| Name       | Type                                         | Description                                                                                                                |
| ---------- | -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `filePath` | `string`                                     | The path of the file on the mount to download, or the local path where the file should be saved (implementation-specific). |
| `options?` | `ToolRouterSessionFilesMountDownloadOptions` | Optional configuration for the download.                                                                                   |

**Returns**

`Promise` — The downloaded file data or path (implementation-specific).

**Example**

```typescript
const session = await composio.toolRouter.use('session_123');
const result = await session.experimental.files.download('/output/report.pdf');
```

```typescript
// Download from a custom mount
await session.experimental.files.download('/exports/data.json', {
  mountId: 'custom-mount',
});
```

***

## list() [#list]

Lists files and directories at the specified path on the session's file mount.

Use this to browse the virtual filesystem attached to the tool router session.
The path is relative to the mount root (e.g., `"/"` for root, `"/documents"` for a subdirectory).
Supports cursor-based pagination via `cursor` and `limit` options.

```typescript
async list(options?: ToolRouterSessionFilesMountListOptions): Promise
```

**Parameters**

| Name       | Type                                     | Description                                    |
| ---------- | ---------------------------------------- | ---------------------------------------------- |
| `options?` | `ToolRouterSessionFilesMountListOptions` | Optional configuration for the list operation. |

**Returns**

`Promise` — List of files with nextCursor for pagination.

**Example**

```typescript
const session = await composio.toolRouter.use('session_123');
const { items, nextCursor } = await session.experimental.files.list({ path: '/' });
```

```typescript
// Paginated listing
let result = await session.experimental.files.list({ path: '/', limit: 10 });
while (result.nextCursor) {
  result = await session.experimental.files.list({ path: '/', cursor: result.nextCursor, limit: 10 });
}
```

***

## upload() [#upload]

Uploads a file to the session's file mount.

Accepts a file path (local or URL), a native File object, or a raw buffer.
The file is stored in the virtual filesystem associated with the tool router session.
URL inputs require a Node.js or Bun runtime so the destination can be DNS-validated;
edge runtimes must fetch the file themselves and pass a File or ArrayBuffer.

```typescript
async upload(input: string | File | ArrayBuffer | Uint8Array, options?: ToolRouterSessionFilesMountUploadOptions): Promise
```

**Parameters**

| Name       | Type                                          | Description                                                            |              |
| ---------- | --------------------------------------------- | ---------------------------------------------------------------------- | ------------ |
| `input`    | `string \| File \| ArrayBuffer \| Uint8Array` | File path (string), native File, or raw buffer (ArrayBuffer            | Uint8Array). |
| `options?` | `ToolRouterSessionFilesMountUploadOptions`    | Optional configuration. When passing a buffer, remotePath is required. |              |

**Returns**

`Promise` — Metadata about the uploaded file.

**Example**

```typescript
// From file path (local or URL)
await session.experimental.files.upload('/path/to/report.pdf');
await session.experimental.files.upload('https://example.com/file.pdf');
```

```typescript
// From native File (e.g. from input[type=file])
await session.experimental.files.upload(fileInput.files[0]);
```

```typescript
// From raw buffer
await session.experimental.files.upload(buffer, { remotePath: 'data.json', mimetype: 'application/json' });
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

