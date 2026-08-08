# Python Custom Provider (/docs/providers/custom-providers/python)

A **custom provider** adapts Composio tools to the format your AI framework expects, so you can use 1000+ tools with a platform Composio doesn't ship support for. This guide shows you how to build one in Python.

# Provider architecture [#provider-architecture]

A provider does two things:

1. **Tool format transformation**: converts Composio tools into the format your AI platform understands.
2. **Platform-specific compatibility**: adds helper methods for executing tool calls and handling responses.

There are two kinds of providers:

| Type            | Use it for                                                                | Examples          |
| --------------- | ------------------------------------------------------------------------- | ----------------- |
| **Non-agentic** | Platforms that don't have their own agency. You drive the tool-call loop. | OpenAI, Anthropic |
| **Agentic**     | Platforms that run their own agent loop and call tools themselves.        | LangChain, CrewAI |

Each type extends a different abstract base class:

```
BaseProvider (Abstract)
├── NonAgenticProvider (Abstract)
│   └── OpenAIProvider (Concrete)
│   └── AnthropicProvider (Concrete)
│   └── [Your Custom Non-Agentic Provider] (Concrete)
└── AgenticProvider (Abstract)
    └── LangchainProvider (Concrete)
    └── [Your Custom Agentic Provider] (Concrete)
```

# Quick start [#quick-start]

Scaffold a working provider with the `make create-provider` script:

```bash
# Create a non-agentic provider
make create-provider name=myprovider

# Create an agentic provider
make create-provider name=myagent agentic=true

# Write to a custom output directory
make create-provider name=myprovider output=/path/to/custom/dir

# Combine options
make create-provider name=myagent agentic=true output=/my/custom/path
```

This generates a provider in `python/providers/<provider-name>/` (or your `output` directory) with a `pyproject.toml`, a provider template, a demo script, a `README`, and type annotations. The template runs as-is. You implement the tool transformation for your platform, and you can keep your provider in your own repository.

The generated structure:

```
python/providers/<provider-name>/
├── README.md                    # Documentation and usage examples
├── pyproject.toml              # Project configuration
├── setup.py                    # Setup script for pip compatibility
├── <provider-name>_demo.py     # Demo script showing usage
└── composio_<provider-name>/   # Package directory
    ├── __init__.py             # Package initialization
    ├── provider.py             # Provider implementation
    └── py.typed                # PEP 561 type marker
```

From there:

1. Navigate to the provider directory: `cd python/providers/<provider-name>`.
2. Install in development mode: `uv pip install -e .`.
3. Implement your provider logic in `composio_<provider-name>/provider.py`.
4. Test with the demo script: `python <provider-name>_demo.py`.

# Creating a non-agentic provider [#creating-a-non-agentic-provider]

A non-agentic provider extends `NonAgenticProvider` and implements two methods: `wrap_tool` (transform one tool) and `wrap_tools` (transform a collection). Add helper methods to execute tool calls in your platform's format.

```python
from typing import List, Optional, Sequence, TypeAlias
from composio.core.provider import NonAgenticProvider
from composio.types import Tool, Modifiers, ToolExecutionResponse

# Define your tool format
class MyAITool:
    def __init__(self, name: str, description: str, parameters: dict):
        self.name = name
        self.description = description
        self.parameters = parameters

# Define your tool collection format
MyAIToolCollection: TypeAlias = List[MyAITool]

# Create your provider
class MyAIProvider(NonAgenticProvider[MyAITool, MyAIToolCollection], name="my-ai-platform"):
    """Custom provider for My AI Platform"""

    def wrap_tool(self, tool: Tool) -> MyAITool:
        """Transform a single tool to platform format"""
        return MyAITool(
            name=tool.slug,
            description=tool.description or "",
            parameters={
                "type": "object",
                "properties": tool.input_parameters.get("properties", {}),
                "required": tool.input_parameters.get("required", [])
            }
        )

    def wrap_tools(self, tools: Sequence[Tool]) -> MyAIToolCollection:
        """Transform a collection of tools"""
        return [self.wrap_tool(tool) for tool in tools]

    # Optional: Custom helper methods for your AI platform
    def execute_my_ai_tool_call(
        self,
        user_id: str,
        tool_call: dict,
        modifiers: Optional[Modifiers] = None
    ) -> ToolExecutionResponse:
        """Execute a tool call in your platform's format

        Example usage:
        result = my_provider.execute_my_ai_tool_call(
            user_id="default",
            tool_call={"name": "GITHUB_STAR_REPO", "arguments": {"owner": "composiohq", "repo": "composio"}}
        )
        """
        # Use the built-in execute_tool method
        return self.execute_tool(
            slug=tool_call["name"],
            arguments=tool_call["arguments"],
            modifiers=modifiers,
            user_id=user_id
        )
```

# Creating an agentic provider [#creating-an-agentic-provider]

An agentic provider extends `AgenticProvider`. Because the platform calls tools itself, `wrap_tool` receives an `execute_tool` function that you wrap into a callable the framework can invoke directly.

```python
from typing import Callable, Dict, List, Sequence
from composio.core.provider import AgenticProvider, AgenticProviderExecuteFn
from composio.types import Tool
from my_provider import AgentTool

# Import the Tool/Function class that represents a callable tool for your framework
# Optionally define your custom tool format below
# class AgentTool:
#     def __init__(self, name: str, description: str, execute: Callable, schema: dict):
#         self.name = name
#         self.description = description
#         self.execute = execute
#         self.schema = schema

# Define your tool collection format (typically a List)
AgentToolCollection: TypeAlias = List[AgentTool]

# Create your provider
class MyAgentProvider(AgenticProvider[AgentTool, List[AgentTool]], name="my-agent-platform"):
    """Custom provider for My Agent Platform"""

    def wrap_tool(self, tool: Tool, execute_tool: AgenticProviderExecuteFn) -> AgentTool:
        """Transform a single tool with execute function"""
        def execute_wrapper(**kwargs) -> Dict:
            result = execute_tool(tool.slug, kwargs)
            if not result.get("successful", False):
                raise Exception(result.get("error", "Tool execution failed"))
            return result.get("data", {})

        return AgentTool(
            name=tool.slug,
            description=tool.description or "",
            execute=execute_wrapper,
            schema=tool.input_parameters
        )

    def wrap_tools(
        self,
        tools: Sequence[Tool],
        execute_tool: AgenticProviderExecuteFn
    ) -> AgentToolCollection:
        """Transform a collection of tools with execute function"""
        return [self.wrap_tool(tool, execute_tool) for tool in tools]
```

# Using your custom provider [#using-your-custom-provider]

Pass an instance of your provider to `Composio`, and every tool you fetch comes back in your format.

## Non-agentic provider [#non-agentic-provider]

You drive the loop: fetch tools, send them to the platform, then hand the response back to the provider to execute the calls.

```python
from composio import Composio
from composio_myai import MyAIProvider
from myai import MyAIClient  # Your AI platform's client

# Initialize tools
myai_client = MyAIClient()
composio = Composio(provider=MyAIProvider())

# Define task
task = "Star a repo composiohq/composio on GitHub"

# Get GitHub tools that are pre-configured
tools = composio.tools.get(user_id="default", toolkits=["GITHUB"])

# Get response from your AI platform (example)
response = myai_client.chat.completions.create(
    model="your-model",
    tools=tools,  # These are in your platform's format
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": task},
    ],
)
print(response)

# Execute the function calls
result = composio.provider.handle_tool_calls(response=response, user_id="default")
print(result)
```

## Agentic provider [#agentic-provider]

The framework runs the loop. Fetch tools, attach them to an agent, and the agent calls them itself.

```python
import asyncio
from agents import Agent, Runner
from composio_myagent import MyAgentProvider
from composio import Composio

# Initialize Composio toolset
composio = Composio(provider=MyAgentProvider())

# Get all the tools
tools = composio.tools.get(
    user_id="default",
    tools=["GITHUB_STAR_A_REPOSITORY_FOR_THE_AUTHENTICATED_USER"],
)

# Create an agent with the tools
agent = Agent(
    name="GitHub Agent",
    instructions="You are a helpful assistant that helps users with GitHub tasks.",
    tools=tools,
)

# Run the agent
async def main():
    result = await Runner.run(
        starting_agent=agent,
        input=(
            "Star the repository composiohq/composio on GitHub. If done "
            "successfully, respond with 'Action executed successfully'"
        ),
    )
    print(result.final_output)

asyncio.run(main())
```

# Best practices [#best-practices]

* **Keep providers focused**: each provider integrates with one platform.
* **Handle errors gracefully**: catch and transform errors from tool execution.
* **Follow platform conventions**: adopt the naming and structure of the target platform.
* **Use type annotations**: lean on Python's typing for IDE support and documentation.
* **Cache transformed tools**: store transformed tools when it helps performance.
* **Add helper methods**: provide convenient methods for common platform operations.
* **Document your provider**: include docstrings and usage examples.
* **Set a meaningful provider name**: the `name` parameter is used for telemetry and debugging.

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

