# RemoteFile (/reference/sdk-reference/typescript/remote-file)

# Usage [#usage]

Access this class through the `composio.remoteFile` property:

```typescript
const composio = new Composio({ apiKey: 'your-api-key' });
const result = await composio.remoteFile.list();
```

# Properties [#properties]

| Name                 | Type     | Description                                              |
| -------------------- | -------- | -------------------------------------------------------- |
| `downloadUrl`        | `string` | Presigned URL for downloading the file                   |
| `expiresAt`          | `string` | ISO 8601 timestamp when the download URL expires         |
| `mountRelativePath`  | `string` | Relative path within the mount (e.g. "report.pdf")       |
| `sandboxMountPrefix` | `string` | Absolute mount path inside the sandbox (e.g. /mnt/files) |

# Methods [#methods]

## blob() [#blob]

Fetches the file content as a Blob.

```typescript
async blob(): Promise
```

**Returns**

`Promise` — The file content as a Blob

***

## buffer() [#buffer]

Fetches the file content as a buffer.

```typescript
async buffer(): Promise
```

**Returns**

`Promise` — The file content as a Uint8Array

***

## save() [#save]

Downloads and saves the file to the local filesystem.
Requires a Node.js runtime with file system support (not available in Cloudflare Workers/Edge).

```typescript
async save(path?: string): Promise<string>
```

**Parameters**

| Name    | Type     | Description                                                                                                           |
| ------- | -------- | --------------------------------------------------------------------------------------------------------------------- |
| `path?` | `string` | Local path to save the file. If omitted, saves to the Composio temp directory using the filename from the mount path. |

**Returns**

`Promise<string>` — The absolute path where the file was saved

***

## text() [#text]

Fetches the file content as UTF-8 text.

```typescript
async text(): Promise<string>
```

**Returns**

`Promise<string>` — The file content as a string

***

## parse() [#parse]

Parses an API response (snake\_case) and returns a RemoteFile instance.

```typescript
parse(data: unknown): RemoteFile
```

**Parameters**

| Name   | Type      | Description                            |
| ------ | --------- | -------------------------------------- |
| `data` | `unknown` | Raw API response with snake\_case keys |

**Returns**

`RemoteFile` — A RemoteFile instance

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

