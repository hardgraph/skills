# Local sandbox (/docs/sandbox/local)

A **local sandbox** runs your agent's code in your own infrastructure instead of Composio's hosted [remote sandbox](/docs/sandbox/remote), so code execution never leaves your security boundary.

# When to use it [#when-to-use-it]

Reach for a local sandbox when:

* **Sensitive code or data.** The agent runs untrusted code or touches data that can't leave your infrastructure.
* **You already run sandboxes.** You'd rather run agent code in your own VM, container, or CI worker than a hosted runtime.
* **You need your own filesystem and shell.** The task installs packages, runs a build, or shells out to local tools.

If none of that applies, use the [remote sandbox](/docs/sandbox/remote). It's the same helper surface with no infrastructure to run.

# Create a local sandbox session [#create-a-local-sandbox-session]

A local sandbox session is a [session](/docs/how-composio-works) created with the remote sandbox turned off. Set `workbench.enable: false`, then pass the session to `experimental_createLocalWorkbenchSession` (from the `@composio/experimental` package), which returns the two pieces you run yourself.

```typescript
import { Composio } from '@composio/core';
import { experimental_createLocalWorkbenchSession } from '@composio/experimental/workbench';

const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });

// Create the session with the remote sandbox disabled, so code runs in your box.
const session = await composio.create('user_123', {
  toolkits: ['github'],
  workbench: { enable: false },
});

const { helperSource, env } = await experimental_createLocalWorkbenchSession(composio, session);
```

You get back two things:

| Field          | What it is                                                                                                                                            |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `helperSource` | A Python helper, as source you write into your sandbox (for example as `composio_helper.py`). It exposes the in-sandbox tool surface the agent calls. |
| `env`          | The environment variables that helper needs to reach Composio from inside your box. Pass them to the process you run the agent in.                    |

`experimental_createLocalWorkbenchSession` validates that the session is local: it throws if the session has the remote workbench enabled, because the remote sandbox and a local one can't both run for a single session. The session you pass in must have `workbench.enable: false`.

> The API ships under the `experimental_` prefix and the `@composio/experimental/workbench` entry point, so the surface may change in a future release.

# The helper contract [#the-helper-contract]

`helperSource` is a Python module your agent imports. It exposes the same helpers the [remote sandbox](/docs/sandbox/remote#built-in-helpers) runs, with the same signatures, so the agent calls tools without knowing that execution is local:

```python
from composio_helper import run_composio_tool, invoke_llm, web_search

response, error = run_composio_tool(
    "GITHUB_GET_A_PULL_REQUEST",
    {"owner": "composiohq", "repo": "composio", "pull_number": 1},
)
```

Every `run_composio_tool` call routes back through the Tool Router under the session's connections, so auth and discovery stay managed.

Most of the workbench helpers run in a local sandbox today. Only the file helpers, which depend on the managed `/mnt/files` mount, are remote-only:

| Helper               | What it does                                                                       | Local sandbox |
| -------------------- | ---------------------------------------------------------------------------------- | ------------- |
| `run_composio_tool`  | Execute any Composio tool                                                          | ✅             |
| `invoke_llm`         | Call an LLM                                                                        | ✅             |
| `web_search`         | Search the web                                                                     | ✅             |
| `proxy_execute`      | Call a toolkit API directly when no tool exists (needs a proxy-execute-scoped key) | ✅             |
| `upload_local_file`  | Upload generated files to storage                                                  | ❌             |
| `smart_file_extract` | Extract text from PDFs and images                                                  | ❌             |

If your agent needs one of the file helpers, use the [remote sandbox](/docs/sandbox/remote) for now.

# Security: the sandbox is your boundary [#security-the-sandbox-is-your-boundary]

The helper reaches Composio with your project `COMPOSIO_API_KEY`, injected into the sandbox through `env`. Any code or output in the sandbox can read it, and that key acts across every connection on the project.

> The sandbox is your security boundary. Isolate it like anything else holding a project credential, and rotate `COMPOSIO_API_KEY` if a run could have leaked it.

# Next [#next]

- [What is a session?](/docs/how-composio-works): How sessions scope tools, auth, and sandbox state to a user

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

