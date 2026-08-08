# Eve (/docs/providers/eve)

The eve provider adapts Composio tools for the [eve](https://github.com/vercel/eve) agent framework. With `EveProvider` registered, `session.tools()` returns eve-native `defineTool`s, so an eve agent gets the Tool Router meta-tools and any preloaded custom toolkits from one call. The provider also ships a `(ctx, next)` hook interface to intercept, allow, deny, or rewrite meta-tool calls before the model sees the result.

> The eve provider is experimental and currently ships from `@composio/experimental` for TypeScript
projects.

# Usage [#usage]

eve owns the agent loop and discovers tools from files, so you register the provider on the Composio client and expose the session's tools from a file under `agent/tools/`.

**Install**

```bash
npm install @composio/core @composio/experimental eve @ai-sdk/openai
```

This walkthrough uses OpenAI directly as one concrete model provider. OpenAI is only an example:
you can use any model provider supported by eve by installing its AI SDK package and configuring
the corresponding credential.

**Configure credentials**

```txt title=".env.local"
COMPOSIO_API_KEY=xxxxxxxxx
OPENAI_API_KEY=xxxxxxxxx
```
`COMPOSIO_API_KEY` authenticates Composio tools. `OPENAI_API_KEY` authenticates the model call
directly with OpenAI; this setup does not route through Vercel AI Gateway.

**Configure the eve model**

```typescript title="agent/agent.ts"
import { openai } from '@ai-sdk/openai';
import { defineAgent } from 'eve';

export default defineAgent({
  model: openai('gpt-5.4-mini'),
});
```
To use another provider, replace `@ai-sdk/openai`, `OPENAI_API_KEY`, and `openai(...)` with the
equivalent package, credential, and model factory supported by eve. Passing a provider model object
calls that provider directly; an eve string model ID such as `openai/gpt-5.4-mini` uses Vercel AI
Gateway instead.

**Register the provider and create a session**

```typescript title="agent/session.ts"
// @noErrors
import { Composio } from '@composio/core';
import { EveProvider } from '@composio/experimental/eve';

const composio = new Composio({ provider: new EveProvider() });

export const session = composio.sessions.create('user_123', {
  toolkits: ['github', 'hackernews'], // optional: scope the session to specific apps
});
```
**Expose the tools to eve**

```typescript title="agent/tools/composio.ts"
// @noErrors
import { defineComposioTools } from '@composio/experimental/eve';
import { session } from '../session';

export default defineComposioTools(session);
```
`defineComposioTools(session)` returns a `step.started` dynamic resolver and memoizes `session.tools()`. It resolves per step because the wrapped `execute` holds a live function eve keeps only for step-scoped tools. The fetch is cached per resolved Composio session, and transient failures are retried on the next step. For a multi-user channel, pass a resolver instead: `defineComposioTools((ctx) => sessionFor(ctx.session.auth.current?.principalId))`.

Your agent now has the Tool Router meta-tools (`COMPOSIO_SEARCH_TOOLS`, `COMPOSIO_MULTI_EXECUTE_TOOL`, `COMPOSIO_MANAGE_CONNECTIONS`) plus any preloaded custom toolkits, with auth handled in chat.

> eve's `defineTool` takes plain JSON Schema, so `EveProvider` passes Composio's `inputParameters`
straight through. It does not convert to a zod schema, which would trip eve's dynamic-tool
normalizer.

# Hooks [#hooks]

Pass a `hooks` object to the constructor to wrap the Tool Router meta-tools. Each hook is `(ctx, next)`: `await next()` runs the default behavior, returning a value replaces what the model sees, and `ctx.deny(reason)` blocks the call. `ctx.request` is mutable; `ctx.context` is read-only.

```typescript
// @noErrors
import { EveProvider } from '@composio/experimental/eve';

const provider = new EveProvider({
  hooks: {
    search: (ctx, next) => {
      ctx.request.args.toolkits = ['github'];
      return next();
    },
    remoteBash: async (ctx, next) => {
      if (String(ctx.request.args.command ?? '').includes('rm -rf')) {
        return ctx.deny('Destructive commands are blocked.');
      }
      return next();
    },
    onAuthLink: async (ctx, next) => {
      await sendConnectionLinkToUser(ctx.url);
      return next();
    },
  },
});
```
Available hooks, each keyed on `EveProviderHooks`:

| Hook                | Wraps                                                     |
| ------------------- | --------------------------------------------------------- |
| `search`            | `COMPOSIO_SEARCH_TOOLS`                                   |
| `manageConnections` | `COMPOSIO_MANAGE_CONNECTIONS`                             |
| `execute`           | `COMPOSIO_MULTI_EXECUTE_TOOL` and `COMPOSIO_EXECUTE_TOOL` |
| `remoteWorkbench`   | `COMPOSIO_REMOTE_WORKBENCH`                               |
| `remoteBash`        | `COMPOSIO_REMOTE_BASH_TOOL`                               |
| `onAuthLink`        | Any auth link found in a result                           |

`ctx.request` carries the meta-tool's raw `{ slug, args }`, and `ctx.deny` is also exported as `denyEveToolCall(reason)`.

## Require approval [#require-approval]

Map Composio tools onto eve's durable approval flow with `needsApproval`. The callback receives the original Composio tool plus eve's approval context:

```typescript
// @noErrors
import { EveProvider, requireApprovalForTools } from '@composio/experimental/eve';

const provider = new EveProvider({
  needsApproval: requireApprovalForTools('LOCAL_IMESSAGE_SEND'),
});
```

When this returns `true`, eve pauses before execution and asks the user to approve the call. `requireApprovalForTools` protects both direct calls and matching entries inside `COMPOSIO_MULTI_EXECUTE_TOOL`. Use an exact slug allowlist for side-effecting tools rather than approving every tool in a toolkit.

# Next [#next]

- [iMessage on eve](/examples/imessage-agent): A full example: a custom toolkit for local iMessage plus the eve provider, on one session.

- [What is a session?](/docs/how-composio-works): How sessions scope users, tools, and auth, and how to reuse them across requests.

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

