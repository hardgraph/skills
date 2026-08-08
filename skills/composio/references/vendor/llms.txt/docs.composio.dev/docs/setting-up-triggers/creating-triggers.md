# Creating triggers (/docs/setting-up-triggers/creating-triggers)

A trigger watches for one event (like `GITHUB_COMMIT_EVENT`) on one user's connected account. Create one, and events start flowing to your [subscription or webhook URL](/docs/setting-up-triggers/subscribing-to-events). For the bigger picture, see [Triggers](/docs/triggers).

The user needs a [connected account](/docs/authentication) for the toolkit you want to monitor. See [Authentication](/docs/authentication) if you haven't set that up.

# Inspect the trigger type [#inspect-the-trigger-type]

Each trigger type declares the config it needs. Check it before you create, so you pass the right fields.

**Python:**

```python
from composio import Composio

composio = Composio()

trigger_type = composio.triggers.get_type("GITHUB_COMMIT_EVENT")
print(trigger_type.config)
# {"properties": {"owner": {...}, "repo": {...}}, "required": ["owner", "repo"]}
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();

const triggerType = await composio.triggers.getType('GITHUB_COMMIT_EVENT');
console.log(triggerType.config);
// {"properties": {"owner": {...}, "repo": {...}}, "required": ["owner", "repo"]}
```

# Create the trigger [#create-the-trigger]

Pass the user, the trigger slug, and the config the type requires.

**Python:**

```python
from composio import Composio

composio = Composio()
user_id = "user-id-123435"

# The user needs a connected account for this toolkit before this runs.
# Set it up first. See /docs/authentication.
trigger = composio.triggers.create(
    slug="GITHUB_COMMIT_EVENT",
    user_id=user_id,
    trigger_config={"owner": "your-repo-owner", "repo": "your-repo-name"},
)
print(f"Trigger created: {trigger.trigger_id}")
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio();
const userId = 'user-id-123435';

// The user needs a connected account for this toolkit before this runs.
// Set it up first. See /docs/authentication.
const trigger = await composio.triggers.create(userId, 'GITHUB_COMMIT_EVENT', {
  triggerConfig: { owner: 'your-repo-owner', repo: 'your-repo-name' },
});
console.log(`Trigger created: ${trigger.triggerId}`);
```

You only pass a `user_id`. Composio resolves that user's connected account for the toolkit automatically.

## Targeting a specific connected account [#targeting-a-specific-connected-account]

If a user has more than one connected account for the toolkit, Composio uses the first active connection for the user and the trigger's toolkit. Pass a connected account ID to pick exactly which account the trigger watches.

**Python:**

```python
trigger = composio.triggers.create(
    slug="GITHUB_COMMIT_EVENT",
    user_id=user_id,
    connected_account_id="ca_def456",
    trigger_config={"owner": "your-repo-owner", "repo": "your-repo-name"},
)
```

**TypeScript:**

```typescript
import { Composio } from '@composio/core';
const composio = new Composio();
const userId = 'user-id-123435';
const trigger = await composio.triggers.create(userId, 'GITHUB_COMMIT_EVENT', {
  connectedAccountId: 'ca_def456',
  triggerConfig: { owner: 'your-repo-owner', repo: 'your-repo-name' },
});
```

Trigger instances default to the `'latest'` toolkit version. If you parse payloads against a fixed schema, [pin a version](/docs/tools-direct/toolkit-versioning#choosing-between-latest-and-a-pinned-version) at SDK initialization.

That's it. The trigger is active. Next, [receive its events](/docs/setting-up-triggers/subscribing-to-events).

# Next [#next]

- [Receiving events](/docs/setting-up-triggers/subscribing-to-events): Get trigger events locally with the SDK or in production over your webhook URL

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

