# Pi (/docs/providers/pi)

The Pi provider adapts Composio tools for [`@earendil-works/pi-coding-agent`](https://www.npmjs.com/package/@earendil-works/pi-coding-agent). A Pi session can search tools, manage connections, execute tools, and run the remote sandbox. Unlike the other providers, `PiProvider` ships its own hook interface to intercept, allow, deny, or rewrite every helper call before the model sees the result.

> The Pi provider ships from `@composio/experimental` for TypeScript projects.

# Dynamic session helpers [#dynamic-session-helpers]

For most Pi apps, expose the dynamic helper tools. They let the model discover exact Composio tool slugs before it executes, request missing connections, and run sandbox commands when you enable them.

**Install**

**Configure API keys**

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
```
**Create a Composio session and Pi tools**

```typescript
// @noErrors
import { Composio } from '@composio/core';
import { PiProvider, createPiComposioSystemPrompt } from '@composio/experimental';
import {
  createAgentSession,
  DefaultResourceLoader,
  getAgentDir,
  SessionManager,
} from '@earendil-works/pi-coding-agent';

const composio = new Composio({
  apiKey: process.env.COMPOSIO_API_KEY,
  provider: new PiProvider(),
});

const composioSession = await composio.sessions.create('user_123', {
  toolkits: ['github', 'gmail'],
  manageConnections: {
    enable: true,
    callbackUrl: 'https://your-app.example.com/auth/callback',
  },
  sandbox: { enable: true },
});

const composioTools = composio.provider.createSessionTools({
  sessionId: composioSession.sessionId,
  search: composioSession.search.bind(composioSession),
  execute: composioSession.execute.bind(composioSession),
  callbackUrl: 'https://your-app.example.com/auth/callback',
  includeWorkbenchTools: true,
  connections: {
    getToolkitStates: toolkits => composioSession.toolkits({ toolkits }),
    authorizeToolkit: (toolkit, options) => composioSession.authorize(toolkit, options),
  },
  hooks: {
    execute: (ctx, next) => {
      if (ctx.request.toolSlug === 'COMPOSIO_MANAGE_CONNECTIONS') {
        return ctx.deny('Use composio_manage_connections instead.');
      }
      return next();
    },
    onAuthLink: async ctx => {
      await sendConnectionLinkToUser(ctx.url);
      return { message: 'Connection link sent out-of-band.' };
    },
  },
});

const loader = new DefaultResourceLoader({
  cwd: process.cwd(),
  agentDir: getAgentDir(),
  systemPromptOverride: () =>
    createPiComposioSystemPrompt(composioSession.sessionId, {
      includeWorkbenchTools: true,
    }),
});
await loader.reload();

const { session: piSession } = await createAgentSession({
  cwd: process.cwd(),
  resourceLoader: loader,
  sessionManager: SessionManager.inMemory(process.cwd()),
  customTools: composioTools,
  tools: [
    'composio_search_tools',
    'composio_manage_connections',
    'composio_execute_tool',
    'composio_remote_workbench',
    'composio_remote_bash',
  ],
});

await piSession.prompt('Find my open GitHub issues and summarize the blockers.');
```
The provider creates these Pi tools:

* `composio_search_tools` searches Composio for exact tool slugs and schemas.
* `composio_manage_connections` checks connection state and initiates auth for missing toolkits.
* `composio_execute_tool` executes an exact Composio tool slug.
* `composio_remote_workbench` runs Python in the Composio sandbox, and requires `includeWorkbenchTools: true`.
* `composio_remote_bash` runs short bash commands in the sandbox filesystem, and requires `includeWorkbenchTools: true`.

You can rename any of these helpers through the `names` option on `createSessionTools`, and the constants live on `PI_COMPOSIO_SESSION_TOOL_NAMES`.

# Hooks [#hooks]

Hooks are the Pi provider's distinctive feature: middleware that wraps each helper so you control what runs and what the model sees. Pass a `hooks` object to `createSessionTools`. Each hook is `(ctx, next)`: `await next()` runs the default behavior, returning a value replaces what the model sees, and `ctx.deny(reason)` blocks the call. `ctx.request` is mutable; `ctx.context` is read-only.

```typescript
// @noErrors
const composioTools = composio.provider.createSessionTools({
  sessionId: composioSession.sessionId,
  search: composioSession.search.bind(composioSession),
  execute: composioSession.execute.bind(composioSession),
  hooks: {
    search: (ctx, next) => {
      ctx.request.toolkits = ctx.request.toolkits?.map(toolkit =>
        toolkit === 'slack' ? 'slackbot' : toolkit
      );
      return next();
    },
    execute: async (ctx, next) => {
      if (ctx.request.toolSlug.startsWith('COMPOSIO_')) {
        return ctx.deny('Meta tools are blocked.');
      }

      const result = await next();
      const file = await saveLargeOutput(result);
      return file ? { message: `Output saved to ${file}` } : result;
    },
    remoteBash: (ctx, next) => {
      if (ctx.request.command.includes('rm -rf')) {
        return ctx.deny('Destructive bash commands are blocked.');
      }
      return next();
    },
    onAuthLink: async (ctx, next) => {
      await sendConnectionLinkToUser(ctx.url);
      return shouldShowLinkToModel(ctx) ? next() : { message: 'Connection link sent out-of-band.' };
    },
  },
});
```

Available hooks, each keyed on `PiSessionHooks`:

| Hook                | Wraps                                                    |
| ------------------- | -------------------------------------------------------- |
| `search`            | Tool discovery; rewrite `query` or `toolkits`            |
| `manageConnections` | Connection checks and auth                               |
| `execute`           | Tool execution; rewrite `toolSlug`, `args`, or `account` |
| `remoteWorkbench`   | The remote Python helper                                 |
| `remoteBash`        | The remote bash helper                                   |
| `onAuthLink`        | Any auth link found in a result                          |

Every `ctx.request` is fully typed, so your editor surfaces the exact fields. `ctx.deny` is also exported as `denyPiToolCall(reason)`.

# Next [#next]

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

