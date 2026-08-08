# OpenAI (/docs/providers/openai)

The OpenAI provider formats Composio tools for OpenAI's function-calling and executes the tool calls the model returns. It works three ways:

* The [Responses API](https://platform.openai.com/docs/api-reference/responses), the recommended way to build agentic flows, where you run the tool-call loop yourself.
* The [Chat Completions API](https://platform.openai.com/docs/api-reference/chat), the classic message-based interface, where you also run the loop.
* The [Agents SDK](https://openai.github.io/openai-agents-python/), where the SDK runs the loop and executes Composio tools for you.

The OpenAI provider is the default provider for the Composio SDK, so you get it without configuring anything. Pick the tab that matches your integration.

### responses

The `OpenAIResponsesProvider` transforms Composio tools into OpenAI's function-calling format for the Responses API, then executes the tool calls the model returns and shapes the results into `function_call_output` items you feed back in.

**Install**

**Python:**

**TypeScript:**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-openai) and `OPENAI_API_KEY` with your [OpenAI API key](https://platform.openai.com/api-keys).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
OPENAI_API_KEY=xxxxxxxxx
```
**Create session and run**

The [Responses API](https://platform.openai.com/docs/api-reference/responses) is the recommended way to build agentic flows with OpenAI. You pass `previous_response_id` on each turn so the model keeps the prior context, and you send back only the new `function_call_output` items.

**Python:**

```python
import json
from openai import OpenAI
from composio import Composio
from composio_openai import OpenAIResponsesProvider

composio = Composio(provider=OpenAIResponsesProvider())
client = OpenAI()

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()

response = client.responses.create(
    model="gpt-5.2",
    tools=tools,
    input=[
        {
            "role": "user",
            "content": "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"
        }
    ]
)

# Agentic loop: keep executing tool calls until the model responds with text
while True:
    tool_calls = [o for o in response.output if o.type == "function_call"]
    if not tool_calls:
        break
    results = composio.provider.handle_tool_calls(response=response, user_id="user_123")
    response = client.responses.create(
        model="gpt-5.2",
        tools=tools,
        previous_response_id=response.id,
        input=[
            {"type": "function_call_output", "call_id": tool_calls[i].call_id, "output": json.dumps(result)}
            for i, result in enumerate(results)
        ]
    )

# Print final response
for item in response.output:
    if item.type == "message":
        print(item.content[0].text)
```
**TypeScript:**

```typescript
import OpenAI from 'openai';
import { Composio } from '@composio/core';
import { OpenAIResponsesProvider } from '@composio/openai';

const composio = new Composio({
    provider: new OpenAIResponsesProvider(),
});
const client = new OpenAI();

// Create a session for your user
const session = await composio.create("user_123");
const tools = await session.tools();

let response = await client.responses.create({
    model: "gpt-5.2",
    tools: tools,
    input: [
        {
            role: "user",
            content: "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"
        },
    ],
});

// Agentic loop: keep executing tool calls until the model responds with text
while (true) {
    const toolCalls = response.output.filter((o) => o.type === "function_call");
    if (toolCalls.length === 0) break;

    const results = await composio.provider.handleToolCalls("user_123", response.output);
    response = await client.responses.create({
        model: "gpt-5.2",
        tools: tools,
        previous_response_id: response.id,
        input: results.map((result, i) => ({
            type: "function_call_output" as const,
            call_id: toolCalls[i].call_id,
            output: JSON.stringify(result),
        })),
    });
}

// Print final response
for (const item of response.output) {
    if (item.type === "message") {
        const block = item.content[0];
        if (block.type === "output_text") {
            console.log(block.text);
        }
    }
}
```
### chat

The `OpenAIProvider` targets the Chat Completions API and is the default provider used by the Composio SDK when you do not specify one.

**Install**

**Python:**

**TypeScript:**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-openai) and `OPENAI_API_KEY` with your [OpenAI API key](https://platform.openai.com/api-keys).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
OPENAI_API_KEY=xxxxxxxxx
```
**Create session and run**

The [Chat Completions API](https://platform.openai.com/docs/api-reference/chat) generates a model response from a list of messages. You keep the full message list yourself and append each assistant message and its `tool` results before the next call.

**Python:**

```python
import json
from openai import OpenAI
from composio import Composio
from composio_openai import OpenAIProvider

composio = Composio(provider=OpenAIProvider())
client = OpenAI()

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()

messages = [
    {"role": "user", "content": "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"}
]

response = client.chat.completions.create(
    model="gpt-5.2",
    tools=tools,
    messages=messages,
)

# Agentic loop: keep executing tool calls until the model responds with text
while response.choices[0].message.tool_calls:
    results = composio.provider.handle_tool_calls(response=response, user_id="user_123")
    messages.append(response.choices[0].message)
    for i, tc in enumerate(response.choices[0].message.tool_calls):
        messages.append({
            "role": "tool",
            "tool_call_id": tc.id,
            "content": json.dumps(results[i]),
        })
    response = client.chat.completions.create(
        model="gpt-5.2",
        tools=tools,
        messages=messages,
    )

print(response.choices[0].message.content)
```
**TypeScript:**

```typescript
import OpenAI from 'openai';
import { Composio } from '@composio/core';
import { OpenAIProvider } from '@composio/openai';

const composio = new Composio({
    provider: new OpenAIProvider(),
});
const client = new OpenAI();

// Create a session for your user
const session = await composio.create("user_123");
const tools = await session.tools();

const messages: OpenAI.Chat.ChatCompletionMessageParam[] = [
    {
        role: "user",
        content: "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"
    },
];

let response = await client.chat.completions.create({
    model: "gpt-5.2",
    tools: tools,
    messages: messages,
});

// Agentic loop: keep executing tool calls until the model responds with text
while (response.choices[0].message.tool_calls) {
    const results = await composio.provider.handleToolCalls("user_123", response);
    messages.push(response.choices[0].message);
    for (const [i, tc] of response.choices[0].message.tool_calls.entries()) {
        messages.push({
            role: "tool",
            tool_call_id: tc.id,
            content: JSON.stringify(results[i]),
        });
    }
    response = await client.chat.completions.create({
        model: "gpt-5.2",
        tools: tools,
        messages: messages,
    });
}

console.log(response.choices[0].message.content);
```
### agents

The `OpenAIAgentsProvider` transforms Composio tools into the Agents SDK tool format with execution built in, so the SDK runs the tool-call loop and you only define the agent and call `run`.

**Install**

**Python:**

**TypeScript:**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-openai) and `OPENAI_API_KEY` with your [OpenAI API key](https://platform.openai.com/api-keys).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
OPENAI_API_KEY=xxxxxxxxx
```
**Create session and run**

**Python:**

```python
import asyncio
from composio import Composio
from composio_openai_agents import OpenAIAgentsProvider
from agents import Agent, Runner

composio = Composio(provider=OpenAIAgentsProvider())

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()

agent = Agent(
    name="Email Agent",
    instructions="You are a helpful assistant.",
    tools=tools,
)

async def main():
    result = await Runner.run(
        starting_agent=agent,
        input="Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'",
    )
    print(result.final_output)

asyncio.run(main())
```
**TypeScript:**

```typescript
import { Composio } from "@composio/core";
import { OpenAIAgentsProvider } from "@composio/openai-agents";
import { Agent, run } from "@openai/agents";

const composio = new Composio({
  provider: new OpenAIAgentsProvider(),
});

// Create a session for your user
const session = await composio.create("user_123");
const tools = await session.tools();

const agent = new Agent({
  name: "Email Agent",
  instructions: "You are a helpful assistant.",
  tools,
});

const result = await run(
  agent,
  "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"
);

console.log(result.finalOutput);
```

# Provider specifics [#provider-specifics]

The OpenAI integration ships three providers, one per API surface:

* **`OpenAIResponsesProvider`** for the Responses API. `handleToolCalls` executes the tool calls and returns `function_call_output` items keyed by `call_id`. You pair it with `previous_response_id` so you only resend new outputs each turn.
* **`OpenAIProvider`** for the Chat Completions API. This is the SDK default, so `new Composio()` with no provider uses it. You keep the full message list and append each assistant message plus its `tool` results yourself.
* **`OpenAIAgentsProvider`** for the Agents SDK. Tools come with execution wired in, so the SDK runs the loop and you do not call `handleToolCalls` at all.

Use the Responses or Agents provider for new agentic flows; reach for Chat Completions when you are extending an existing Chat Completions codebase.

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

