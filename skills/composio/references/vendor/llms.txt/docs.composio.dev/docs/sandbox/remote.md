# Remote sandbox (/docs/sandbox/remote)

The **sandbox** is a persistent Python environment where your agent writes and executes code. It has programmatic access to all Composio tools, plus helper functions for calling LLMs, uploading files, and making API requests. State persists across calls within a [session](/docs/how-composio-works). Your agent runs code in it through the `COMPOSIO_REMOTE_WORKBENCH` meta tool, and shell commands through `COMPOSIO_REMOTE_BASH_TOOL`.

> **Renamed from workbench**: This feature used to be called the **workbench**. The preferred session config key is now `sandbox`, but `workbench` still works as a fully supported alias, in both SDKs and on the wire. It isn't deprecated, so existing code keeps running unchanged. The `COMPOSIO_REMOTE_WORKBENCH` and `COMPOSIO_REMOTE_BASH_TOOL` meta tools keep their names.

> **Composio doesn't run or replace your agent**: Your agent and its model stay entirely yours — Composio never proxies your LLM or runs an agent on your behalf. It discovers, authenticates, and executes tools. The one place Composio itself can call a model is *inside the sandbox*, and only when your code opts in — most directly through the [`invoke_llm`](#built-in-helpers) helper. That optional, sandbox-only usage is the sole source of any Composio-side **LLM tokens**. The sandbox is opt-in per [session](/docs/configuring-sessions) via `sandbox: { enable: true }` — don't enable it (or simply never call `invoke_llm`) and Composio uses no LLM tokens at all.

# Where it fits [#where-it-fits]

Use the sandbox when a task is too complex for individual tool calls. Your agent starts with [`SEARCH_TOOLS` to find the right tools, then uses `MULTI_EXECUTE`](/docs/how-composio-works#meta-tools) for straightforward calls. When the task involves bulk operations, data transformations, or multi-step logic, the agent reaches for `COMPOSIO_REMOTE_WORKBENCH` instead.

# What the sandbox provides [#what-the-sandbox-provides]

## Built-in helpers [#built-in-helpers]

These functions are pre-initialized in every sandbox, so your agent can call them without any setup:

| Helper               | What it does                                                                                          |
| -------------------- | ----------------------------------------------------------------------------------------------------- |
| `run_composio_tool`  | Execute any Composio tool (e.g., `GMAIL_SEND_EMAIL`, `SLACK_SEND_MESSAGE`) and get structured results |
| `invoke_llm`         | Call an LLM for classification, summarization, content generation, or data extraction                 |
| `upload_local_file`  | Upload generated files (reports, CSVs, images) to cloud storage and get a download URL                |
| `proxy_execute`      | Make direct API calls to connected services when no pre-built tool exists                             |
| `web_search`         | Search the web and return results for research or data enrichment                                     |
| `smart_file_extract` | Extract text from PDFs, images, and other file formats in the sandbox                                 |

## Libraries [#libraries]

The sandbox ships with common packages pre-installed: `pandas`, `numpy`, `matplotlib`, `Pillow`, `PyTorch`, and `reportlab`. Beyond these, the sandbox maintains a list of supported packages and their dependencies. If the agent uses a package that isn't already installed, the sandbox installs it automatically.

## Error correction [#error-correction]

The sandbox corrects common mistakes in the code your agent generates. For example, if a script accesses `result["apiKey"]` but the actual field name is `api_key`, the sandbox resolves the mismatch instead of failing.

## Persistent state [#persistent-state]

The sandbox runs as a persistent Jupyter notebook. Variables, imports, files, and in-memory state from one call are available in the next.

## Compute tier [#compute-tier]

Sandboxes default to `standard` (1 vCPU, 1 GB RAM). For heavier workloads (large dataframes, ML preprocessing, or big bulk operations), pick a larger tier when creating the session via `sandbox.sandboxSize` (TypeScript) or `sandbox.sandbox_size` (Python).

Available tiers:

* `standard` (1 vCPU, 1 GB RAM)
* `medium` (2 vCPU, 2 GB)
* `large` (4 vCPU, 4 GB)
* `xlarge` (8 vCPU, 8 GB)

Larger tiers require `@composio/core` ≥ `0.8.1` or `composio` ≥ `0.12.1`. See [Configuring sessions → Sandbox compute tier](/docs/configuring-sessions#sandbox-compute-tier) for examples.

> **Pricing:** Sandboxes are not billed today. Composio plans to begin billing for sandbox usage soon (metered by tier and runtime).

# Files and mounts [#files-and-mounts]

The sandbox has a persistent file mount at `/mnt/files/`. Code running in the sandbox reads and writes files there, and the mount survives sandbox restarts: changing the [compute tier](#compute-tier) recreates the sandbox and clears in-memory state, but `/mnt/files/` persists.

Move files between your app and the mount with `session.experimental.files`. Upload an input file and the agent reads it at `/mnt/files/<path>`. When the agent writes a result, download it from your app.

> The files API ships under `session.experimental`, so the surface may change in a future release.

**Python:**

```python
session = composio.create("user_123")

# Upload a local file; the sandbox sees it at /mnt/files/sales.csv
uploaded = session.experimental.files.upload("./sales.csv")
print(uploaded.sandbox_mount_prefix, uploaded.mount_relative_path)
# /mnt/files sales.csv

# After the agent writes a result in the sandbox, download it
report = session.experimental.files.download("/report.pdf")
report.save("./report.pdf")
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio({ apiKey: 'your_api_key' });
const session = await composio.create("user_123");

// Upload a local file; the sandbox sees it at /mnt/files/sales.csv
const uploaded = await session.experimental.files.upload("./sales.csv");
console.log(uploaded.sandboxMountPrefix, uploaded.mountRelativePath);
// /mnt/files sales.csv

// After the agent writes a result in the sandbox, download it
const report = await session.experimental.files.download("/report.pdf");
await report.save("./report.pdf");
```

The mount exposes four methods:

| Method                     | What it does                                                                |
| -------------------------- | --------------------------------------------------------------------------- |
| `upload(input, options?)`  | Upload from a local path, URL, `File`, or buffer. Returns a `RemoteFile`.   |
| `list(options?)`           | List files under `path` on the mount, with `cursor` and `limit` pagination. |
| `download(path, options?)` | Fetch a file from the mount as a `RemoteFile`.                              |
| `delete(path, options?)`   | Remove a file or directory from the mount.                                  |

A `RemoteFile` carries the file's bytes and a presigned `downloadUrl`. Read it with `text()` or `buffer()`, or write it to disk with `save(path)`. Its `expiresAt` is when that download link expires, not a TTL on the file: the mount itself has no expiry you set.

Every file lives on the session's default `files` mount, surfaced at `/mnt/files/`. Each method takes a `mountId` to address a mount by ID, but there's no SDK call to create custom mounts today, so you'll normally work with `files`.

# Next [#next]

- [Local sandbox](/docs/sandbox/local): Run the same tool calls in a sandbox you own, while Composio keeps managed auth and discovery.

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

