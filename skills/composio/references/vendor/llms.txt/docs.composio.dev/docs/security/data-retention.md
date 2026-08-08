# Data retention (/docs/security/data-retention)

Composio stores audit logs for tool executions so you can observe and debug your agents. You control whether the request and response payloads for each call are stored.

# Customer data vs. your end users' data [#customer-data-vs-your-end-users-data]

Throughout this guide, *you* are the Composio customer. *Your end users* are the people your agent acts on behalf of. Each end user is identified by the `user_id` you provide. Composio stores this value as supplied and uses it to associate connected accounts, sessions, and execution logs.

Tool requests and responses may contain additional end-user data. The **Log storage** setting controls whether these payloads are retained; it does not remove the `user_id` or other audit metadata.

# What Composio stores [#what-composio-stores]

Composio creates logs for tool executions and trigger events. These logs contain:

* For tool executions, the toolkit and action, execution status, connection and auth-config IDs, supplied `user_id`, timing, and runtime/source.
* By default, the **request arguments** and **response data** for each tool execution.
* For trigger events, similar audit metadata and, by default, the trigger payloads.

# How long logs are retained [#how-long-logs-are-retained]

Tool execution logs and trigger event logs are retained for up to **one year**, after which they are automatically deleted.

# How long files are available [#how-long-files-are-available]

When a tool uses or returns a file (an attachment, image, document, export, and so on), Composio stages it in temporary object storage and shares it through a presigned URL. The URL is valid for **1 hour by default**. You can configure this URL TTL per project in **Project Settings**, up to 24 hours.

Presigned URL expiry and file deletion are separate. When the URL expires, the link stops resolving, but that does not delete the underlying object. Files staged for tool execution are automatically deleted from temporary object storage after **24 hours**.

Workbench storage has two separate lifecycles:

* Files written to `/mnt/files` are backed by temporary object storage and are automatically deleted after **24 hours**.
* Other sandbox files, variables, and runtime state are temporary and may be cleared after approximately **12 hours** of inactivity.

# Choose what we store: the Log storage setting [#choose-what-we-store-the-log-storage-setting]

You control whether call payloads are stored for each project in **Settings → General → Log storage**:

* **Store all logs** (default) — Composio stores the full request and response payloads in the execution log.
* **Don't store data** — Composio does not store the request arguments or response data from your tool calls. It keeps only the audit record: which tool ran, when it ran, whether it succeeded, the relevant IDs, and timing information.

With **Don't store data**, Composio keeps an audit trail but does not retain your tool-call payloads (including your end users' data) in its logs. This setting also applies to [Proxy Execute](/docs/extending-sessions/proxy-execute). If Composio cannot retrieve your project's log-storage setting, Proxy Execute does not store the request or response payload.

<img alt="Log storage set to &#x22;Don't store data&#x22; in Project Settings, General" src="__img0" />

> To stop storing payloads, open the Dashboard, choose your project, go to **Settings → General → Log storage**, and select **Don't store data**.

## Changing the setting affects new calls [#changing-the-setting-affects-new-calls]

You can switch between the two log-storage options at any time. Each change applies only to new calls:

* Selecting **Don't store data** stops Composio from storing payloads for new calls. It does **not** delete payloads that were already stored: existing logs stay available until they age out under the one-year retention window.
* Selecting **Store all logs** again resumes storing payloads for new calls. It does **not** backfill the period while payload storage was disabled.

Payloads for calls made while storage was disabled were never stored and cannot be recovered later.

# Where your data goes [#where-your-data-goes]

Choosing **Don't store data** stops Composio from persisting your payloads, but data may still pass through the following locations during execution:

* The **destination provider** that receives the tool call.
* **Composio's execution infrastructure** while the call runs.
* **Temporary object storage** for files used or returned by tool calls. Presigned URL expiry is configurable per project, while the underlying staged file is deleted after 24 hours. See [How long files are available](#how-long-files-are-available).
* Your configured **trigger destinations**, which receive trigger events in real time.
* An isolated third-party **code sandbox**, if you use Workbench or remote code execution. Ordinary Python or shell execution does not inherently send code or sandbox files to an LLM provider. Model-provider processing occurs only in the cases described below.

## When Workbench uses an LLM [#when-workbench-uses-an-llm]

When a task requires advanced processing, the agent or MCP client may use Workbench and call `invoke_llm` from the submitted code—for example, to summarize, analyze, extract, or generate content. Only the information passed to `invoke_llm` is sent to the model provider; sandbox files and previous results are not sent unless the submitted code includes them.

There is one separate case: if submitted Python contains a syntax error and automatic repair is enabled, Composio may send the code and error details to the configured model provider to fix it before execution. Valid Python and ordinary shell commands do not trigger this repair.

So "Don't store data" controls what Composio retains, not whether data is processed during execution. For the current list of our sub-processors and our data-handling terms, see the [Composio Trust Center](https://trust.composio.dev/subprocessors).

# Stronger guarantees [#stronger-guarantees]

If your organization needs a contractual zero-data-retention arrangement or shorter retention windows, [contact sales](https://composio.dev/contact?utm_source=docs). For our data-handling policies and sub-processor list, see the [Composio Trust Center](https://trust.composio.dev).

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

