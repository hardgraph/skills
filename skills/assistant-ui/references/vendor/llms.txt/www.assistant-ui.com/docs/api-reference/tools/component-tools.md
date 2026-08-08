# Component Tools
URL: /docs/api-reference/tools/component-tools

Register assistant tools from mounted React components, scoped to the lifetime of part of the UI tree.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!warn]
>
> `makeAssistantTool` and `useAssistantTool` are **deprecated** compatibility APIs, documented here for reference only. New code should define a toolkit — see [Defining Tools](/docs/tools/defining-tools) and [Migrating Tools to Toolkits](/docs/migrations/toolkit-tools).

## API Reference

### makeAssistantTool

> [!warn]
>
> **Deprecated.** Use a toolkit with `Tools({ toolkit })` and register it via `useAui({ tools: Tools({ toolkit }) })` instead. See <https://assistant-ui.com/docs/migrations/toolkit-tools>.

Creates a React component that registers an assistant tool when rendered.

Use this when exporting reusable tool modules that can be included in JSX rather than calling [useAssistantTool](/docs/api-reference/tools/component-tools#useassistanttool) directly.

```
type AssistantToolProps = CoreAssistantToolProps<TArgs, TResult> & {
  /** Component used to render calls to this tool in assistant messages. */
  render?: ToolCallMessagePartComponent<TArgs, TResult> | undefined;
  /** Lightweight text rendered while a tool call is running or complete. */
  renderText?: ToolCallText<TArgs, TResult> | undefined;
};

const makeAssistantTool: <TArgs extends Record<string, unknown>, TResult>(tool: AssistantToolProps<TArgs, TResult>) => AssistantTool;
```

### useAssistantTool

> [!warn]
>
> **Deprecated.** Use a toolkit with `Tools({ toolkit })` and register it via `useAui({ tools: Tools({ toolkit }) })` instead. See <https://assistant-ui.com/docs/migrations/toolkit-tools>.

Registers a tool with the assistant model context while the component is mounted.

If `render` is provided, it is also installed as the renderer for matching tool-call message parts. The registration is removed automatically when the component unmounts or the tool definition changes.

Pass a referentially stable tool object, such as one declared at module scope or memoized with `useMemo`, to avoid re-registering on every render.

```
const weatherTool = {
  toolName: "get_weather",
  type: "frontend",
  description: "Get the weather for a city.",
  parameters: weatherSchema,
  execute: async ({ city }: { city: string }) => fetchWeather(city),
  render: WeatherToolUI,
} satisfies AssistantToolProps<{ city: string }, Weather>;

function WeatherToolRegistration() {
  useAssistantTool(weatherTool);
  return null;
}
```

- `tool`: `AssistantToolProps<TArgs, TResult>`

  - `streamCall?`: `ToolStreamCallFunction<TArgs, TResult>` (deprecated: Experimental, API may change.)

  - `display?`: `ToolDisplay` — How this tool's UI is presented relative to the chain-of-thought trace. Defaults to \`"inline"\` (folded into the chain-of-thought grouping). Set \`"standalone"\` to surface the tool call on its own. \`human\` tools are always \`"standalone"\` and cannot opt out.

  - `unstable_backendDefault?`: `AssistantToolProps["unstable_backendDefault"]`
    - `parameters?`: `boolean`

  - `overwrite?`: `boolean` — Replaces an already-registered same-priority tool with the same name instead of throwing. Discouraged escape hatch — overwriting a same-priority tool is usually a design smell; prefer distinct names or priorities.

  - `type?`: `"mcp"` — Tools loaded from an MCP server by a server adapter.

  - `description?`: `undefined` — Natural-language description shown to the model when selecting tools.

  - `parameters?`: `undefined` — Schema for the arguments the model must provide when calling the tool.

  - `disabled?`: `boolean` — Prevents the tool from being exposed to the model while true.

  - `execute?`: `undefined` — Executes the tool after the model provides valid arguments.

  - `toModelOutput?`: `undefined` — Converts the execution result into model-visible output.

  - `experimental_onSchemaValidationError?`: `undefined` — Handles invalid tool arguments when schema validation fails.

  - `providerOptions?`: `undefined`

  - `toolName`: `string`

  - `render?`: `unknown` — Component used to render calls to this tool in assistant messages.

  - `renderText?`: `ToolCallText<TArgs, TResult>` — Lightweight text rendered while a tool call is running or complete.

    - `running?`: `ToolCallRunningText<TArgs>`
    - `complete?`: `ToolCallCompleteText<TArgs, TResult>`