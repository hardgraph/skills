# TypeScript Custom Provider (/docs/providers/custom-providers/typescript)

A **provider** adapts Composio tools to the format your AI framework expects. Write one, and any framework can call Composio's 1000+ tools. This guide shows you how to build your own in TypeScript.

# Provider architecture [#provider-architecture]

A provider does three things:

* **Transforms tool format**: converts Composio tools into the shape your AI platform expects.
* **Executes tools**: runs tool calls and returns results.
* **Adds platform helpers**: exposes convenience methods specific to your platform.

There are two kinds, depending on whether the target platform runs its own agent loop:

| Type            | When to use                                                  | Examples           |
| --------------- | ------------------------------------------------------------ | ------------------ |
| **Non-agentic** | The platform has no agency of its own. You drive the loop.   | OpenAI             |
| **Agentic**     | The platform runs its own agent loop and calls tools itself. | LangChain, AutoGPT |

Both extend `BaseProvider`:

```
BaseProvider (Abstract)
├── BaseNonAgenticProvider (Abstract)
│   └── OpenAIProvider (Concrete)
│   └── [Your Custom Non-Agentic Provider] (Concrete)
└── BaseAgenticProvider (Abstract)
    └── [Your Custom Agentic Provider] (Concrete)
```

# Non-agentic provider [#non-agentic-provider]

A non-agentic provider extends `BaseNonAgenticProvider`. You supply a `name`, `wrapTool`, and `wrapTools`, and call the built-in `executeTool` when you're ready to run a tool.

```typescript
import { BaseNonAgenticProvider, Tool } from '@composio/core';

// Define your tool format
interface MyAITool {
  name: string;
  description: string;
  parameters: {
    type: string;
    properties: Record<string, unknown>;
    required?: string[];
  };
}

// Define your tool collection format
type MyAIToolCollection = MyAITool[];

// Create your provider
export class MyAIProvider extends BaseNonAgenticProvider {
  // Required: Unique provider name for telemetry
  readonly name = 'my-ai-platform';

  // Required: Method to transform a single tool
  override wrapTool(tool: Tool): MyAITool {
    return {
      name: tool.slug,
      description: tool.description || '',
      parameters: {
        type: 'object',
        properties: tool.inputParameters?.properties || {},
        required: tool.inputParameters?.required || [],
      },
    };
  }

  // Required: Method to transform a collection of tools
  override wrapTools(tools: Tool[]): MyAIToolCollection {
    return tools.map(tool => this.wrapTool(tool));
  }

  // Optional: Custom helper methods for your AI platform
  async executeMyAIToolCall(
    userId: string,
    toolCall: {
      name: string;
      arguments: Record<string, unknown>;
    }
  ): Promise<string> {
    // Use the built-in executeTool method
    const result = await this.executeTool(toolCall.name, {
      userId,
      arguments: toolCall.arguments,
    });

    return JSON.stringify(result.data);
  }
}
```

# Agentic provider [#agentic-provider]

An **agentic provider** extends `BaseAgenticProvider`. The difference from the non-agentic case: `wrapTool` and `wrapTools` receive an `executeToolFn`, which you embed in each tool so the framework's agent can run the tool itself.

```typescript
import { BaseAgenticProvider, Tool, ExecuteToolFn } from '@composio/core';

// Define your tool format
interface AgentTool {
  name: string;
  description: string;
  execute: (args: Record<string, unknown>) => Promise<unknown>;
  schema: Record<string, unknown>;
}

// Define your tool collection format
interface AgentToolkit {
  tools: AgentTool[];
  createAgent: (config: Record<string, unknown>) => unknown;
}

// Create your provider
export class MyAgentProvider extends BaseAgenticProvider {
  // Required: Unique provider name for telemetry
  readonly name = 'my-agent-platform';

  // Required: Method to transform a single tool with execute function
  override wrapTool(tool: Tool, executeToolFn: ExecuteToolFn): AgentTool {
    return {
      name: tool.slug,
      description: tool.description || '',
      schema: tool.inputParameters || {},
      execute: async (args: Record<string, unknown>) => {
        const result = await executeToolFn(tool.slug, args);
        if (!result.successful) {
          throw new Error(result.error || 'Tool execution failed');
        }
        return result.data;
      },
    };
  }

  // Required: Method to transform a collection of tools with execute function
  override wrapTools(tools: Tool[], executeToolFn: ExecuteToolFn): AgentToolkit {
    const agentTools = tools.map(tool => this.wrapTool(tool, executeToolFn));

    return {
      tools: agentTools,
      createAgent: config => {
        // Create an agent using the tools
        return {
          run: async (prompt: string) => {
            // Implementation depends on your agent framework
            console.log(`Running agent with prompt: ${prompt}`);
            // The agent would use the tools.execute method to run tools
          },
        };
      },
    };
  }

  // Optional: Custom helper methods for your agent platform
  async runAgent(agentToolkit: AgentToolkit, prompt: string): Promise<unknown> {
    const agent = agentToolkit.createAgent({});
    return await agent.run(prompt);
  }
}
```

# Use your provider [#use-your-provider]

Pass an instance to `Composio` via the `provider` option. Every tool you fetch comes back in your custom format.

```typescript
import { Composio } from '@composio/core';
import { MyAIProvider } from './my-ai-provider';

// Create your provider instance
const myProvider = new MyAIProvider();

// Initialize Composio with your provider
const composio = new Composio({
  apiKey: 'your-composio-api-key',
  provider: myProvider,
});

// Get tools - they will be transformed by your provider
const tools = await composio.tools.get('default', {
  toolkits: ['github'],
});

// Use the tools with your AI platform
console.log(tools); // These will be in your custom format
```

# Provider state and context [#provider-state-and-context]

A provider is a class, so it can hold state. Use the constructor for config, and instance fields for caches or counters.

```typescript
export class StatefulProvider extends BaseNonAgenticProvider {
  readonly name = 'stateful-provider';

  // Provider state
  private requestCount = 0;
  private toolCache = new Map<string, any>();
  private config: ProviderConfig;

  constructor(config: ProviderConfig) {
    super();
    this.config = config;
  }

  override wrapTool(tool: Tool): ProviderTool {
    this.requestCount++;

    // Use the provider state/config
    const enhancedTool = {
      // Transform the tool
      name: this.config.useUpperCase ? tool.slug.toUpperCase() : tool.slug,
      description: tool.description,
      schema: tool.inputParameters,
    };

    // Cache the transformed tool
    this.toolCache.set(tool.slug, enhancedTool);

    return enhancedTool;
  }

  override wrapTools(tools: Tool[]): ProviderToolCollection {
    return tools.map(tool => this.wrapTool(tool));
  }

  // Custom methods that use provider state
  getRequestCount(): number {
    return this.requestCount;
  }

  getCachedTool(slug: string): ProviderTool | undefined {
    return this.toolCache.get(slug);
  }
}
```

# Advanced: compose providers [#advanced-compose-providers]

You don't have to start from a base class. Extend an existing provider to add behavior like analytics or retries, and call `super` to reuse its logic.

```typescript
import { OpenAIProvider } from '@composio/openai';

// Extend the OpenAI provider with custom functionality
export class EnhancedOpenAIProvider extends OpenAIProvider {
  // Add properties
  private analytics = {
    toolCalls: 0,
    errors: 0,
  };

  // Override methods to add functionality
  override async executeToolCall(userId, tool, options, modifiers) {
    this.analytics.toolCalls++;

    try {
      // Call the parent implementation
      const result = await super.executeToolCall(userId, tool, options, modifiers);
      return result;
    } catch (error) {
      this.analytics.errors++;
      throw error;
    }
  }

  // Add new methods
  getAnalytics() {
    return this.analytics;
  }

  async executeWithRetry(userId, tool, options, modifiers, maxRetries = 3) {
    let attempts = 0;
    let lastError;

    while (attempts < maxRetries) {
      try {
        return await this.executeToolCall(userId, tool, options, modifiers);
      } catch (error) {
        lastError = error;
        attempts++;
        await new Promise(resolve => setTimeout(resolve, 1000 * attempts));
      }
    }

    throw lastError;
  }
}
```

# Best practices [#best-practices]

* **Keep providers focused**: each provider should target one platform.
* **Handle errors gracefully**: catch and transform errors from tool execution.
* **Follow platform conventions**: adopt the naming and structure of the target platform.
* **Cache transformed tools**: reuse wrapped tools instead of rebuilding them.
* **Add helper methods**: expose convenience methods for common platform operations.
* **Document your provider**: describe its features and usage.
* **Set a meaningful `name`**: it's used for telemetry insights.

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

