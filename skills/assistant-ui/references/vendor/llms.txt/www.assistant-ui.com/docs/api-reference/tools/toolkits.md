# Toolkits
URL: /docs/api-reference/tools/toolkits

Define model-facing tools and compose them into named toolkits registered with an assistant-ui runtime scope.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A `Toolkit` is a named map of model-facing tool definitions. The `Tools` resource installs a toolkit into an assistant subtree, registering each tool with the model context and each `render` component with the tool-call renderer scope.

Use these APIs when you want a tool's availability to follow your runtime or provider tree. The older component-scoped registration APIs are deprecated; see [Migrating Tools to Toolkits](/docs/migrations/toolkit-tools).

## API Reference

### tool

Defines a model tool with its argument schema, execution behavior, and optional model-output conversion.

This helper keeps reusable tool definitions type-checked and convenient to export for a [Toolkit](/docs/api-reference/tools/toolkits#toolkit) registered with [Tools](/docs/api-reference/tools/toolkits#tools). When `parameters` is a Standard Schema (for example Zod v4), its input type is inferred for `execute`, `streamCall`, and model-output callbacks. Plain JSON Schema objects do not carry TypeScript input types, so provide generic arguments or annotate callbacks when you need precise args for JSON Schema.

```
import { z } from "zod";

const getWeather = tool({
  type: "frontend",
  description: "Get the weather for a city.",
  parameters: z.object({ city: z.string() }),
  execute: async ({ city }) => `Sunny in ${city}`,
});
```

- `tool`: `Tool<StandardSchemaInput<TSchema>, TResult> & { parameters: TSchema; }`

  - `streamCall?`: `ToolStreamCallFunction<TArgs, TResult>` (deprecated: Experimental, API may change.)
  - `display?`: `ToolDisplay` — How this tool's UI is presented relative to the chain-of-thought trace. Defaults to \`"inline"\` (folded into the chain-of-thought grouping). Set \`"standalone"\` to surface the tool call on its own. \`human\` tools are always \`"standalone"\` and cannot opt out.
  - `unstable_backendDefault?`: `Tool<StandardSchemaInput<TSchema>, TResult> & { parameters: TSchema; }["unstable_backendDefault"]`
    - `parameters?`: `boolean`
  - `overwrite?`: `boolean` — Replaces an already-registered same-priority tool with the same name instead of throwing. Discouraged escape hatch — overwriting a same-priority tool is usually a design smell; prefer distinct names or priorities.
  - `type?`: `"mcp"` — Tools loaded from an MCP server by a server adapter.
  - `description?`: `undefined` — Natural-language description shown to the model when selecting tools.
  - `parameters`: `undefined` — Schema for the arguments the model must provide when calling the tool.
  - `disabled?`: `boolean` — Prevents the tool from being exposed to the model while true.
  - `execute?`: `undefined` — Executes the tool after the model provides valid arguments.
  - `toModelOutput?`: `undefined` — Converts the execution result into model-visible output.
  - `experimental_onSchemaValidationError?`: `undefined` — Handles invalid tool arguments when schema validation fails.
  - `providerOptions?`: `undefined`

### ToolDefinition

Tool definition accepted by the React tool registry.

Extends the core tool contract with tool-call display options. Human tools rely on `render` to collect input from the user. Frontend tools execute in the browser and require either `render` or `renderText` for their progress and result. Backend tools execute server-side and may omit a renderer.

- `streamCall?`: `ToolStreamCallFunction<TArgs, TResult>` (deprecated: Experimental, API may change.)
- `display?`: `ToolDisplay` — How this tool's UI is presented relative to the chain-of-thought trace. Defaults to \`"inline"\` (folded into the chain-of-thought grouping). Set \`"standalone"\` to surface the tool call on its own. \`human\` tools are always \`"standalone"\` and cannot opt out.
- `unstable_backendDefault?`: `ToolDefinition["unstable_backendDefault"]`
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
- `render?`: `ToolCallMessagePartComponent<TArgs, TResult>`

### Toolkit

Named collection of tools exposed to the assistant model.

Keys are the tool names the model receives and uses in tool calls.

```
const toolkit = defineToolkit({
  get_weather: {
    type: "frontend",
    description: "Get the weather for a city.",
    parameters: weatherSchema,
    execute: async ({ city }: { city: string }) => fetchWeather(city),
    render: WeatherToolUI,
  },
});
```

```
type Toolkit = Record<string, ToolDefinition<any, any>>;
```

### Tools

- `0`: `Tools props["0"]`

  - `toolkit?`: `Toolkit` — Tools to expose to the model and optional renderers to install.

  - `mcpApp?`: `ResourceElement<McpAppResourceOutput>` — Optional MCP app resource whose tools should be merged into context.

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }`

- `push`: `(...items: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => number`

- `concat`: `{ (...items: ConcatArray<{ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }>[]): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; (...items: ({ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; } | ConcatArray<{ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }>)[]): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]`

- `shift`: `() => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }`

- `slice`: `(start?: number, end?: number) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]`

- `sort`: `(compareFn?: ((a: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, b: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }) => number) | undefined) => [{ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }]`

- `splice`: `{ (start: number, deleteCount?: number): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; (start: number, deleteCount: number, ...items: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; }`

- `unshift`: `(...items: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => number`

- `indexOf`: `(searchElement: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => value is S, thisArg?: any): S[]; (predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; }`

- `reduce`: `{ (callbackfn: (previousValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentIndex: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }; (callbackfn: (previousValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentIndex: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, initialValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }; <U>(callbackfn: (previousValue: U, currentValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentIndex: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentIndex: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }; (callbackfn: (previousValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentIndex: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, initialValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }; <U>(callbackfn: (previousValue: U, currentValue: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, currentIndex: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, obj: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, obj: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; } | undefined; }`

- `findIndex`: `(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, obj: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, start?: number, end?: number) => [{ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }]`

- `copyWithin`: `(target: number, start: number, end?: number) => [{ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }]`

- `entries`: `() => ArrayIterator<[number, { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<{ toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }>`

- `includes`: `(searchElement: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }`

- `findLast`: `{ <S>(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; } | undefined; }`

- `findLastIndex`: `(predicate: (value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, index: number, array: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]`

- `toSorted`: `(compareFn?: ((a: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }, b: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }) => number) | undefined) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; (start: number, deleteCount?: number): { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]; }`

- `with`: `(index: number, value: { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }) => { toolkit?: Toolkit; mcpApp?: ResourceElement<McpAppResourceOutput> | undefined; }[]`

### defineMcpToolkit

Defines MCP server tools as a spreadable toolkit fragment. Pass a raw `McpServerConfig` for always-on servers, or `{ server, disabled, tools }` when a server should stay configured but not expose all tools for the current request.

- `definition`: `McpToolkitDefinition`

### defineToolkit

Toolkit authoring helper. Accepts the permissive ToolkitDefinition (a generative `backend` tool may carry its server `execute`) and types the result as the canonical [Toolkit](/docs/api-reference/tools/toolkits#toolkit).

In a `"use generative"` file, the compiler strips the wrapper per build so it can split schemas, renderers, and executors across the client/server boundary. Outside generative compilation, it returns the toolkit unchanged and can be used for plain frontend/backend/human toolkit objects.

- `_definition`: ``{ [K in keyof TArgsByName]: (Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "frontend"; description?: string | undefined; parameters: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]>; disabled?: boolean; execute?: ToolExecuteFunction<TArgsByName[K], TResultByName[K]>; toModelOutput?: ToolModelOutputFunction<TArgsByName[K], TResultByName[K]>; experimental_onSchemaValidationError?: (args: unknown, context: ToolExecutionContext) => TResultByName[K] | Promise<TResultByName[K]>; providerOptions?: ProviderOptions; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: ((args: NoInfer<TArgsByName[K]>, context: ToolExecutionContext) => TResultByName[K] | Promise<TResultByName[K]>) | undefined; } & { toModelOutput?: ToolModelOutputFunction<NoInfer<TArgsByName[K]>, NoInfer<TResultByName[K]>> | undefined; } & { experimental_onSchemaValidationError?: ((args: unknown, context: ToolExecutionContext) => NoInfer<TResultByName[K]> | Promise<NoInfer<TResultByName[K]>>) | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }) | (Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "backend"; description?: string | undefined; parameters?: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined; disabled?: boolean; execute?: ToolExecuteFunction<TArgsByName[K], TResultByName[K]>; toModelOutput?: ToolModelOutputFunction<TArgsByName[K], TResultByName[K]>; experimental_onSchemaValidationError?: (args: unknown, context: ToolExecutionContext) => TResultByName[K] | Promise<TResultByName[K]>; providerOptions?: ProviderOptions; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: ((args: NoInfer<TArgsByName[K]>, context: ToolExecutionContext) => TResultByName[K] | Promise<TResultByName[K]>) | undefined; } & { toModelOutput?: ToolModelOutputFunction<NoInfer<TArgsByName[K]>, NoInfer<TResultByName[K]>> | undefined; } & { experimental_onSchemaValidationError?: ((args: unknown, context: ToolExecutionContext) => NoInfer<TResultByName[K]> | Promise<NoInfer<TResultByName[K]>>) | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }) | (Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "human"; description?: string | undefined; parameters: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]>; disabled?: boolean; display?: "standalone"; execute?: undefined; toModelOutput?: undefined; experimental_onSchemaValidationError?: undefined; providerOptions?: ProviderOptions; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: undefined | undefined; } & { toModelOutput?: undefined | undefined; } & { experimental_onSchemaValidationError?: undefined | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }) | (Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "provider"; providerId: `${string}.${string}`; parameters?: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined; args: Record<string, unknown>; supportsDeferredResults?: boolean; description?: undefined; disabled?: boolean; execute?: undefined; toModelOutput?: undefined; experimental_onSchemaValidationError?: undefined; providerOptions?: ProviderOptions; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: undefined | undefined; } & { toModelOutput?: undefined | undefined; } & { experimental_onSchemaValidationError?: undefined | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }) | (Omit<Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "frontend"; description?: string | undefined; parameters: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]>; disabled?: boolean; execute?: ToolExecuteFunction<TArgsByName[K], TResultByName[K]>; toModelOutput?: ToolModelOutputFunction<TArgsByName[K], TResultByName[K]>; experimental_onSchemaValidationError?: (args: unknown, context: ToolExecutionContext) => TResultByName[K] | Promise<TResultByName[K]>; providerOptions?: ProviderOptions; }, "type"> & { type?: undefined; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: ((args: NoInfer<TArgsByName[K]>, context: ToolExecutionContext) => TResultByName[K] | Promise<TResultByName[K]>) | undefined; } & { toModelOutput?: ToolModelOutputFunction<NoInfer<TArgsByName[K]>, NoInfer<TResultByName[K]>> | undefined; } & { experimental_onSchemaValidationError?: ((args: unknown, context: ToolExecutionContext) => NoInfer<TResultByName[K]> | Promise<NoInfer<TResultByName[K]>>) | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }) | (Omit<Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "human"; description?: string | undefined; parameters: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]>; disabled?: boolean; display?: "standalone"; execute?: undefined; toModelOutput?: undefined; experimental_onSchemaValidationError?: undefined; providerOptions?: ProviderOptions; }, "type"> & { type?: undefined; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: undefined | undefined; } & { toModelOutput?: undefined | undefined; } & { experimental_onSchemaValidationError?: undefined | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }) | (Omit<Omit<{ streamCall?: ToolStreamCallFunction<TArgsByName[K], TResultByName[K]>; display?: "inline" | "standalone"; unstable_backendDefault?: { parameters?: boolean; }; overwrite?: boolean; } & { type: "provider"; providerId: `${string}.${string}`; parameters?: JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined; args: Record<string, unknown>; supportsDeferredResults?: boolean; description?: undefined; disabled?: boolean; execute?: undefined; toModelOutput?: undefined; experimental_onSchemaValidationError?: undefined; providerOptions?: ProviderOptions; }, "type"> & { type?: undefined; }, "type" | "execute" | "streamCall" | "toModelOutput" | "experimental_onSchemaValidationError"> & { type?: never; } & { execute?: undefined | undefined; } & { toModelOutput?: undefined | undefined; } & { experimental_onSchemaValidationError?: undefined | undefined; } & { streamCall?: ((reader: ToolCallReader<TArgsByName[K], unknown>, context: ToolExecutionContext) => void) | undefined; } & { render?: ToolCallMessagePartComponent<TArgsByName[K], TResultByName[K]> | undefined; renderText?: ToolCallText<TArgsByName[K], TResultByName[K]> | undefined; } & { parameters: NonNullable<JSONSchema7 | StandardSchemaV1<TArgsByName[K], TArgsByName[K]> | undefined>; }); }``

### externalTool

Marks a generative toolkit entry as an externally executed backend tool.

Use this when another system (for example a backend route or LangGraph node) already defines and executes the tool, but assistant-ui should render its tool calls. The use-generative compiler omits `execute: externalTool()` entries from the server build and keeps a `type: "backend"` renderer on the client build.

```
function externalTool(): never;
```

### humanTool

Marks a tool as **human-in-the-loop**: the agent pauses and the UI (`render`) supplies the result instead of code. Use it as the tool's `execute`:

```
confirm: { execute: humanTool(), render: (props) => <Confirm {...props} /> }
```

Unlike [defineToolkit](/docs/api-reference/tools/toolkits#definetoolkit), it has **no runtime implementation**: a `"use generative"` compiler (e.g. `@assistant-ui/next` or `@assistant-ui/vite`) detects `execute: humanTool()`, drops it, and stamps the tool `type: "human"`. Reaching it at runtime means the module wasn't compiled (used outside a `"use generative"` file), so it throws.

```
function humanTool(): never;
```

### providerTool

Marks a tool as provider-executed. The use-generative compiler converts `execute: providerTool(...)` into a `type: "provider"` tool entry.

- `_config`: `ProviderToolConfig<Record<string, unknown>>`

  - `args`: `Record<string, unknown>` — Provider-specific configuration for this tool.
  - `parameters?`: `StandardSchemaV1<TArgs> | JSONSchema7` — Schema used by adapters for validation and type plumbing.
  - `providerOptions?`: `ProviderOptions`
  - `providerId`: `` `${string}.${string}` `` — Provider-defined tool identifier, e.g. \`openai.web\_search\_preview\`.
  - `supportsDeferredResults?`: `boolean` — Whether provider results may arrive after the originating response turn.

### stubTool

Marks a generative toolkit entry as a frontend tool whose executor will be supplied by `useAuiToolOverrides(...)`.

`stubTool()` has no runtime implementation. It must be used inside a `"use generative"` toolkit file so the compiler can strip it.

```
function stubTool(): never;
```

### hitl

> [!warn]
>
> **Deprecated.** Use [humanTool](/docs/api-reference/tools/toolkits#humantool).

```
const hitl: typeof humanTool;
```

### hitlTool

> [!warn]
>
> **Deprecated.** Use [humanTool](/docs/api-reference/tools/toolkits#humantool).

```
const hitlTool: typeof humanTool;
```