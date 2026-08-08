# Google (/docs/providers/google)

The Google provider formats Composio tools for [Gemini](https://ai.google.dev/) and the [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/). Pick the tab that matches your setup.

### gemini

In Python, the Gemini provider (`composio_gemini`) wraps Composio tools as typed callables, and the `google-genai` SDK's Automatic Function Calling executes tool calls inside the chat loop for you. In TypeScript, the Google provider transforms Composio tools into Gemini function declarations, and you run the loop: execute each call with `composio.provider.executeToolCall`, feed the result back, and repeat until the model replies with text.

**Install**

**Python:**

**TypeScript:**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-google) and `GOOGLE_API_KEY` with your [Google API key](https://aistudio.google.com/apikey).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
GOOGLE_API_KEY=xxxxxxxxx
```
**Create session and run**

**Python:**

```python
from composio import Composio
from composio_gemini import GeminiProvider
from google import genai
from google.genai import types

composio = Composio(provider=GeminiProvider())
client = genai.Client()

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()

config = types.GenerateContentConfig(tools=tools)
chat = client.chats.create(model="gemini-3-pro-preview", config=config)

# Automatic Function Calling executes tool calls inside the chat loop
response = chat.send_message(
    "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'"
)
print(response.text)
```
**TypeScript:**

```typescript
import { Composio } from '@composio/core';
import { GoogleProvider } from '@composio/google';
import { GoogleGenAI, type Part } from '@google/genai';

const composio = new Composio({
    provider: new GoogleProvider(),
});
const ai = new GoogleGenAI({ apiKey: process.env.GOOGLE_API_KEY! });

// Create a session for your user
const session = await composio.create("user_123");
const tools = await session.tools();

const chat = ai.chats.create({
    model: 'gemini-3-pro-preview',
    config: {
        tools: [{ functionDeclarations: tools }],
    },
});

let response = await chat.sendMessage({
    message: "Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'",
});

// Agentic loop: keep executing tool calls until the model responds with text
while (response.functionCalls && response.functionCalls.length > 0) {
    const parts: Part[] = [];
    for (const fc of response.functionCalls) {
        const result = await composio.provider.executeToolCall("user_123", {
            name: fc.name || '',
            args: (fc.args || {}) as Record<string, unknown>,
        });
        parts.push({
            functionResponse: {
                id: fc.id,
                name: fc.name,
                response: JSON.parse(result),
            },
        });
    }
    response = await chat.sendMessage({ message: parts });
}

console.log(response.text);
```
### adk

The Google ADK provider transforms Composio tools into ADK's `FunctionTool` format. Unlike Gemini function calling, ADK runs the tool loop for you: hand the tools to an `Agent`, and the `Runner` executes calls and continues until the agent produces a final response. ADK integration is Python-only.

**Install**

**Configure API Keys**

> Set `COMPOSIO_API_KEY` with your API key from [Settings](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=docs-providers-google) and `GOOGLE_API_KEY` with your [Google API key](https://aistudio.google.com/apikey).

```txt title=".env"
COMPOSIO_API_KEY=xxxxxxxxx
GOOGLE_API_KEY=xxxxxxxxx
```
**Create session and run**

```python
from composio import Composio
from composio_google_adk import GoogleAdkProvider
from google.adk.agents import Agent
from google.adk.runners import Runner
from google.adk.sessions import InMemorySessionService
from google.genai import types

composio = Composio(provider=GoogleAdkProvider())

# Create a session for your user
session = composio.create(user_id="user_123")
tools = session.tools()

agent = Agent(
    name="email_agent",
    model="gemini-3-pro-preview",
    instruction="You are an AI agent that sends emails using Gmail.",
    tools=tools,
)

session_service = InMemorySessionService()
adk_session = session_service.create_session_sync(
    app_name="email_agent",
    user_id="user_123",
    session_id="session_1",
)
runner = Runner(
    agent=agent,
    app_name="email_agent",
    session_service=session_service,
)

content = types.Content(
    role="user",
    parts=[types.Part(text="Send an email to john@example.com with the subject 'Hello' and body 'Hello from Composio!'")],
)

events = runner.run(user_id="user_123", session_id="session_1", new_message=content)
for event in events:
    if event.is_final_response() and event.content and event.content.parts:
        print(event.content.parts[0].text)
```

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

