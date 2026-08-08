# Review pull requests in a sandbox you own (/examples/local-sandbox-pr-reviewer)

Composio usually runs your tools for you. A **local sandbox** is for the times you need to run them yourself: your filesystem, your shell, your security boundary. You still get [managed auth](/docs/authentication) and 1000+ apps; you just keep the code execution.

This example builds a GitHub PR reviewer that does exactly that: it clones a pull request into a sandbox *you* own, runs the repo's real checks there, and posts one grounded comment. The sandbox here is E2B, but E2B is just the sample. The same pattern works with your own VM, container, Kubernetes job, or internal sandbox service.

It comes down to a handful of Composio pieces:

1. **A local sandbox session** is a [Composio session](/docs/how-composio-works) with [code execution turned off](/docs/configuring-sessions#disabling-the-sandbox). Composio still does [discovery](/docs/how-composio-works#meta-tools) and auth; it just won't run code for you.
2. **The helper contract** is what comes back: a Python helper exposing the same [`run_composio_tool`, `invoke_llm`, and `web_search`](/docs/sandbox/remote) tools Composio's managed sandbox runs for you, plus the `env` it needs. You inject it into your sandbox and the agent calls it.
3. **Your sandbox is the boundary.** Tool *execution* happens in a box you control. E2B is the replaceable sample runner; the contract it honors is the real interface.

> **The sandbox holds your project API key**: The `env` that `experimental_createLocalWorkbenchSession` returns includes your **project** `COMPOSIO_API_KEY`, and you inject that `env` into the sandbox. Anything running there can read it, including the untrusted PR code you clone and build. Treat the sandbox as your trust boundary: run it on infrastructure you control, give the reviewer a key scoped to only what it needs, and rotate the key if a run could have leaked it.

Below you build the host orchestration from scratch: a bare client first, then a piece at a time up to the full run loop, then a browse of the real source. You bring a Composio API key and a place to run code. Composio brings the tools.

# Setup [#setup]

You need a [Composio API key](https://dashboard.composio.dev?utm_source=docs\&utm_medium=content\&utm_campaign=examples-local-sandbox-pr-reviewer), an OpenAI API key for the reviewer agent, a GitHub connection for your `COMPOSIO_USER_ID`, and [Bun](https://bun.sh).

**No sandbox provider? Use the E2B sample runner**

The host writes the Composio helper into a sandbox and runs the agent there, so it needs *somewhere* to run code. This example ships an [E2B](https://e2b.dev) runner in `src/sandbox/e2b.ts` so you can run it today with just an `E2B_API_KEY`. E2B is a hosted sandbox provider: that key provisions an isolated microVM to run the agent in, so you don't have to stand up a VM or container yourself. It's still real infrastructure, just E2B's to manage rather than yours.

E2B is deliberately isolated to that one file. To run on your own VM, container, or CI worker, replace `createE2bSandbox` with anything that honors the same contract: create a directory, write `helperSource` into it, pass `env` to the process, stream stdout and stderr back, and tear down on your schedule.

```bash
bun add @composio/core @composio/experimental e2b @openai/agents
```

Connect GitHub once for the user id you'll review as, then keep that same id for the review run:

```bash
bun run connect
```

# Build the host [#build-the-host]

`src/runner.ts` is the host: it owns orchestration, never tool execution. It starts as a bare Composio client and grows into the full run loop, one concept at a time. Each diff below is exactly what that concept adds.

## Create the Composio client [#create-the-composio-client]

The whole thing acts as one stable user, against the connections they own. Start there.

## Check the GitHub connection [#check-the-github-connection]

A local sandbox still leans on Composio for auth and [tool discovery](/docs/how-composio-works#meta-tools); only code execution moves to your side. So before booting any infrastructure, confirm this user actually has [GitHub connected](/docs/authentication), and hand them a connect link if not.

## Create the local sandbox session [#create-the-local-sandbox-session]

The core of the integration. You create a [Composio session](/docs/configuring-sessions#creating-a-session) yourself with code execution off (`workbench.enable: false`, so Composio will not run code for you), then hand that session to `experimental_createLocalWorkbenchSession`. The helper validates the session is local (it errors if the session has the remote workbench enabled, because the managed workbench and a local sandbox can't both run for one session) and returns the pieces you run yourself: a `helperSource` (a Python helper with `run_composio_tool`, `invoke_llm`, and `web_search`) and the `env` that helper needs to reach Composio from inside your box.

## Start your sandbox, inject the helper [#start-your-sandbox-inject-the-helper]

Boot a box you control, write `helperSource` into it as `composio_helper.py`, and pass `env` to the process. That helper is the *only* Composio-specific thing your sandbox has to carry. E2B is the sample runner; swap it for anything that honors the same contract.

## Run the reviewer and stream output [#run-the-reviewer-and-stream-output]

Run the agent inside the sandbox and stream its output back. Whenever the agent calls `run_composio_tool`, the helper routes that GitHub action back through Composio under this user's connection. Tool *execution* happens in your box; discovery and auth stay managed.

# The whole project [#the-whole-project]

The file above is the spine. The real project rounds it out with a CLI, a smoke/dry-run path, the E2B runner behind the sandbox contract, the reviewer agent and its review policy, and the `composio_helper.py` the helper source compiles to. Here's a slice of the actual source, with the Composio touch-points highlighted. Browse the tree, read the files:

> The local PR reviewer browser is a documentation snapshot; a public repository is not available.

# Run it [#run-it]

Dry-run first to validate your input with no credentials, network calls, or sandbox startup, then run it for real:

```bash
bun run review -- --repo ComposioHQ/composio --pr 123 --dry-run
bun run review -- --repo ComposioHQ/composio --pr 123
```

The host opens a local sandbox session, boots the sandbox, and runs the repo's real checks inside it, then posts one grounded comment, or nothing if it can't build the PR.

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

