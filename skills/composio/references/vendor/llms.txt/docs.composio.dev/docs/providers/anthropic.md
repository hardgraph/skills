# Anthropic (/docs/providers/anthropic)

The Anthropic provider formats Composio tools for Claude and executes the tool calls Claude returns. It works two ways:

* The [Claude Messages API](https://docs.anthropic.com/en/api/messages), where you run the tool-call loop yourself.
* The [Claude Agent SDK](https://platform.claude.com/docs/en/agent-sdk/overview), where the SDK runs the loop and Composio tools are exposed as an in-process MCP server.

Pick the tab that matches your integration.

### messages

The `AnthropicProvider` transforms Composio tools into the format the Claude Messages API expects, then executes the tool calls Claude requests and shapes the results back into Messages API content blocks.

**Install**

**Python:**

**TypeScript:**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-anthropic) and `ANTHROPIC_API_KEY` with your [Anthropic API key](https://console.anthropic.com/settings/keys).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
ANTHROPIC_API_KEY=xxxxxxxxx
```
**Create session and run**

**Python:**

```python
import json
import anthropic
from composio import Composio
from composio_anthropic import AnthropicProvider

composio = Composio(provider=AnthropicProvider())
client = anthropic.Anthropic()

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()

messages = [
    {"role": "user", "content": "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"}
]

response = client.messages.create(
    model="claude-opus-4-6",
    max_tokens=4096,
    tools=tools,
    messages=messages,
)

# Agentic loop: keep executing tool calls until the model responds with text
while response.stop_reason == "tool_use":
    tool_use_blocks = [block for block in response.content if block.type == "tool_use"]
    results = composio.provider.handle_tool_calls(user_id="user_123", response=response)
    messages.append({"role": "assistant", "content": response.content})
    messages.append({
        "role": "user",
        "content": [
            {"type": "tool_result", "tool_use_id": tool_use_blocks[i].id, "content": json.dumps(result)}
            for i, result in enumerate(results)
        ]
    })
    response = client.messages.create(
        model="claude-opus-4-6",
        max_tokens=4096,
        tools=tools,
        messages=messages,
    )

# Print final response
for block in response.content:
    if block.type == "text":
        print(block.text)
```
**TypeScript:**

```typescript
import Anthropic from '@anthropic-ai/sdk';
import { Composio } from '@composio/core';
import { AnthropicProvider } from '@composio/anthropic';

const composio = new Composio({
    provider: new AnthropicProvider(),
});
const client = new Anthropic();

// Create a session for your user
const session = await composio.create("user_123");
const tools = await session.tools();

const messages: Anthropic.MessageParam[] = [
    {
        role: "user",
        content: "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"
    },
];

let response = await client.messages.create({
    model: "claude-opus-4-6",
    max_tokens: 4096,
    tools: tools,
    messages: messages,
});

// Agentic loop: keep executing tool calls until the model responds with text
while (response.stop_reason === "tool_use") {
    const toolResults = await composio.provider.handleToolCalls("user_123", response);
    messages.push({ role: "assistant", content: response.content });
    messages.push(...toolResults);
    response = await client.messages.create({
        model: "claude-opus-4-6",
        max_tokens: 4096,
        tools: tools,
        messages: messages,
    });
}

// Print final response
for (const block of response.content) {
    if (block.type === "text") {
        console.log(block.text);
    }
}
```
> `handleToolCalls` does the loop body for you. It extracts every `tool_use` block from the response, executes the matching Composio tools, and returns a ready-to-append array of Messages API content (a `user` message holding `tool_result` blocks). That is why the TypeScript example spreads the result straight into `messages` with `messages.push(...toolResults)`. In Python it returns the raw result list, which the example wraps into `tool_result` blocks by hand.

### agent-sdk

The `ClaudeAgentSDKProvider` exposes your Composio tools to the Claude Agent SDK as an in-process MCP server. You build the server with `create_sdk_mcp_server` (Python) or `createSdkMcpServer` (TypeScript), register it on the agent, and the SDK runs the tool-call loop for you.

**Install**

**Python:**

**TypeScript:**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-anthropic) and `ANTHROPIC_API_KEY` with your [Anthropic API key](https://console.anthropic.com/settings/keys).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
ANTHROPIC_API_KEY=xxxxxxxxx
```
**Create session and run**

**Python:**

```python
import asyncio
from composio import Composio
from composio_claude_agent_sdk import ClaudeAgentSDKProvider
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions, create_sdk_mcp_server

composio = Composio(provider=ClaudeAgentSDKProvider())

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()
tool_server = create_sdk_mcp_server(name="composio", version="1.0.0", tools=tools)

async def main():
    options = ClaudeAgentOptions(
        system_prompt="You are a helpful assistant",
        permission_mode="bypassPermissions",
        mcp_servers={"composio": tool_server},
    )

    async with ClaudeSDKClient(options=options) as client:
        await client.query("Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'")
        async for msg in client.receive_response():
            print(msg)

asyncio.run(main())
```
**TypeScript:**

```js
import { Composio } from '@composio/core';
import { ClaudeAgentSDKProvider } from '@composio/claude-agent-sdk';
import { createSdkMcpServer, query } from '@anthropic-ai/claude-agent-sdk';

const composio = new Composio({
    provider: new ClaudeAgentSDKProvider(),
});

// Create a session for your user
const session = await composio.create("user_123");
const tools = await session.tools();
const toolServer = createSdkMcpServer({
    name: "composio",
    version: "1.0.0",
    tools: tools,
});

for await (const content of query({
    prompt: "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'",
    options: {
        mcpServers: { composio: toolServer },
        permissionMode: "bypassPermissions",
    },
})) {
    if (content.type === "assistant") {
        console.log("Claude:", content.message);
    }
}
```

# Provider specifics [#provider-specifics]

A few things are specific to the Anthropic provider:

* **Tool caching.** Pass `cacheTools: true` to the constructor (`new AnthropicProvider({ cacheTools: true })`) to attach Anthropic's ephemeral `cache_control` to every tool definition and tool-result block. This lets Claude reuse cached tool schemas across requests and can cut prompt cost when you send the same large tool set on every turn.
* **`handleToolCalls` returns Messages API content, not raw strings.** On the TypeScript side it returns `Anthropic.Messages.MessageParam[]` (a `user` message of `tool_result` blocks) that you append directly to your message list. This is why the loop above does not build `tool_result` blocks by hand.
* **String-encoded tool inputs are handled for you.** Claude occasionally emits a tool's `input` as a JSON string instead of an object. The provider normalizes this before execution, so you do not have to parse it yourself.
* **`cacheTools` only.** The constructor takes no other options. There is no agentic execution in this provider; you run the loop (Messages API) or hand the loop to the Claude Agent SDK (Agent SDK tab).

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

