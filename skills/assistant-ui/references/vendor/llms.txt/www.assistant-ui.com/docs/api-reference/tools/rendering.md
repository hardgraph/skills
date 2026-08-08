# Tool Rendering
URL: /docs/api-reference/tools/rendering

Register React renderers for assistant-ui tool calls, tool results, and model data parts.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### DataRenderers

- `length`: `0`
- `toString`: `() => string`
- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`
- `pop`: `() => undefined`
- `push`: `(...items: never[]) => number`
- `concat`: `{ (...items: ConcatArray<never>[]): never[]; (...items: ConcatArray<never>[]): never[]; }`
- `join`: `(separator?: string) => string`
- `reverse`: `() => never[]`
- `shift`: `() => undefined`
- `slice`: `(start?: number, end?: number) => never[]`
- `sort`: `(compareFn?: ((a: never, b: never) => number) | undefined) => []`
- `splice`: `{ (start: number, deleteCount?: number): never[]; (start: number, deleteCount: number, ...items: never[]): never[]; }`
- `unshift`: `(...items: never[]) => number`
- `indexOf`: `(searchElement: never, fromIndex?: number) => number`
- `lastIndexOf`: `(searchElement: never, fromIndex?: number) => number`
- `every`: `{ <S>(predicate: (value: never, index: number, array: never[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: never, index: number, array: never[]) => unknown, thisArg?: any): boolean; }`
- `some`: `(predicate: (value: never, index: number, array: never[]) => unknown, thisArg?: any) => boolean`
- `forEach`: `(callbackfn: (value: never, index: number, array: never[]) => void, thisArg?: any) => void`
- `map`: `<U>(callbackfn: (value: never, index: number, array: never[]) => U, thisArg?: any) => U[]`
- `filter`: `{ <S>(predicate: (value: never, index: number, array: never[]) => value is S, thisArg?: any): S[]; (predicate: (value: never, index: number, array: never[]) => unknown, thisArg?: any): never[]; }`
- `reduce`: `{ (callbackfn: (previousValue: never, currentValue: never, currentIndex: number, array: never[]) => never): never; (callbackfn: (previousValue: never, currentValue: never, currentIndex: number, array: never[]) => never, initialValue: never): never; <U>(callbackfn: (previousValue: U, currentValue: never, currentIndex: number, array: never[]) => U, initialValue: U): U; }`
- `reduceRight`: `{ (callbackfn: (previousValue: never, currentValue: never, currentIndex: number, array: never[]) => never): never; (callbackfn: (previousValue: never, currentValue: never, currentIndex: number, array: never[]) => never, initialValue: never): never; <U>(callbackfn: (previousValue: U, currentValue: never, currentIndex: number, array: never[]) => U, initialValue: U): U; }`
- `find`: `{ <S>(predicate: (value: never, index: number, obj: never[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: never, index: number, obj: never[]) => unknown, thisArg?: any): undefined; }`
- `findIndex`: `(predicate: (value: never, index: number, obj: never[]) => unknown, thisArg?: any) => number`
- `fill`: `(value: never, start?: number, end?: number) => []`
- `copyWithin`: `(target: number, start: number, end?: number) => []`
- `entries`: `() => ArrayIterator<[number, never]>`
- `keys`: `() => ArrayIterator<number>`
- `values`: `() => ArrayIterator<never>`
- `includes`: `(searchElement: never, fromIndex?: number) => boolean`
- `flatMap`: `<U, This>(callback: (this: This, value: never, index: number, array: never[]) => U | readonly U[], thisArg?: This | undefined) => U[]`
- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`
- `at`: `(index: number) => undefined`
- `findLast`: `{ <S>(predicate: (value: never, index: number, array: never[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: never, index: number, array: never[]) => unknown, thisArg?: any): undefined; }`
- `findLastIndex`: `(predicate: (value: never, index: number, array: never[]) => unknown, thisArg?: any) => number`
- `toReversed`: `() => never[]`
- `toSorted`: `(compareFn?: ((a: never, b: never) => number) | undefined) => never[]`
- `toSpliced`: `{ (start: number, deleteCount: number, ...items: never[]): never[]; (start: number, deleteCount?: number): never[]; }`
- `with`: `(index: number, value: never) => never[]`

### getMcpAppFromToolPart

Returns MCP app metadata for a tool-call part that points at a `ui://` resource.

Returns `undefined` when the part has no MCP app metadata or the metadata does not reference an assistant-ui MCP app resource.

- `part`: `ToolPartLike`

  - `mcp?`: `ToolCallMessagePartMcpMetadata` — MCP app metadata associated with this tool call, when present.

    - `app?`: `McpAppMetadata`

      - `resourceUri`: `string`
      - `mimeType?`: `string`
      - `visibility?`: `readonly ("model" | "app")[]`
      - `serverId?`: `string` — Routable server identity emitted by the agent stack when multiple MCP servers back one agent; for

### makeAssistantDataUI

Creates a React component that registers a named data-part renderer when rendered.

```
type AssistantDataUIProps = {
  /** Data part name this renderer handles. */
  name: string;
  /** Component rendered for matching data message parts. */
  render: DataMessagePartComponent<T>;
};

const makeAssistantDataUI: <T = any>(dataUI: AssistantDataUIProps<T>) => AssistantDataUI;
```

### McpAppRenderer

- `0`: `McpAppRendererOptions`

  - `host`: `ResourceElement<McpAppsHost>` — Provides the data-plane operations the widget can request (\`loadResource\`, \`callTool\`, \`readResource\`, \`listResources\`). Use \`McpAppsRemoteHost({ url })\` for the default HTTP-route convention.

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `sandbox?`: `McpAppSandboxConfig` — Sandbox + container styling. Passes through to SafeContentFrame.

    - `sandbox?`: `SandboxOption[]`
    - `useShadowDom?`: `boolean`
    - `enableBrowserCaching?`: `boolean`
    - `salt?`: `string`
    - `product?`: `string`
    - `className?`: `string`
    - `style?`: `CSSProperties`
    - `unsafeDocumentWrite?`: `boolean`

  - `maxHeight?`: `number` — Upper bound (in pixels) applied to the widget-driven auto-resize height. Defaults to 800.

  - `hostInfo?`: `McpAppHostInfo` — Identifies the host to the widget in the \`ui/initialize\` response.

    - `name`: `string`
    - `version`: `string`

  - `hostContext?`: `McpAppHostContext` — Delivered to the widget on initialize and pushed via \`notifications/host\_context/changed\` on change.

    - `theme?`: `"light" | "dark"`
    - `displayMode?`: `McpAppDisplayMode`
    - `availableDisplayModes?`: `McpAppDisplayMode[]`

  - `fallback?`: `ReactNode` — Rendered when no MCP app is on the part, or while load is in flight / failed (unless overridden).

  - `loadingFallback?`: `ReactNode` — Rendered while the resource is loading. Defaults to \`fallback\`.

  - `errorFallback?`: `ReactNode | ((error: Error) => ReactNode)` — Rendered when the resource load rejects. Defaults to \`fallback\`.

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => McpAppRendererOptions`

- `push`: `(...items: McpAppRendererOptions[]) => number`

- `concat`: `{ (...items: ConcatArray<McpAppRendererOptions>[]): McpAppRendererOptions[]; (...items: (McpAppRendererOptions | ConcatArray<McpAppRendererOptions>)[]): McpAppRendererOptions[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => McpAppRendererOptions[]`

- `shift`: `() => McpAppRendererOptions`

- `slice`: `(start?: number, end?: number) => McpAppRendererOptions[]`

- `sort`: `(compareFn?: ((a: McpAppRendererOptions, b: McpAppRendererOptions) => number) | undefined) => [options: McpAppRendererOptions]`

- `splice`: `{ (start: number, deleteCount?: number): McpAppRendererOptions[]; (start: number, deleteCount: number, ...items: McpAppRendererOptions[]): McpAppRendererOptions[]; }`

- `unshift`: `(...items: McpAppRendererOptions[]) => number`

- `indexOf`: `(searchElement: McpAppRendererOptions, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: McpAppRendererOptions, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => value is S, thisArg?: any): S[]; (predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => unknown, thisArg?: any): McpAppRendererOptions[]; }`

- `reduce`: `{ (callbackfn: (previousValue: McpAppRendererOptions, currentValue: McpAppRendererOptions, currentIndex: number, array: McpAppRendererOptions[]) => McpAppRendererOptions): McpAppRendererOptions; (callbackfn: (previousValue: McpAppRendererOptions, currentValue: McpAppRendererOptions, currentIndex: number, array: McpAppRendererOptions[]) => McpAppRendererOptions, initialValue: McpAppRendererOptions): McpAppRendererOptions; <U>(callbackfn: (previousValue: U, currentValue: McpAppRendererOptions, currentIndex: number, array: McpAppRendererOptions[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: McpAppRendererOptions, currentValue: McpAppRendererOptions, currentIndex: number, array: McpAppRendererOptions[]) => McpAppRendererOptions): McpAppRendererOptions; (callbackfn: (previousValue: McpAppRendererOptions, currentValue: McpAppRendererOptions, currentIndex: number, array: McpAppRendererOptions[]) => McpAppRendererOptions, initialValue: McpAppRendererOptions): McpAppRendererOptions; <U>(callbackfn: (previousValue: U, currentValue: McpAppRendererOptions, currentIndex: number, array: McpAppRendererOptions[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: McpAppRendererOptions, index: number, obj: McpAppRendererOptions[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: McpAppRendererOptions, index: number, obj: McpAppRendererOptions[]) => unknown, thisArg?: any): McpAppRendererOptions | undefined; }`

- `findIndex`: `(predicate: (value: McpAppRendererOptions, index: number, obj: McpAppRendererOptions[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: McpAppRendererOptions, start?: number, end?: number) => [options: McpAppRendererOptions]`

- `copyWithin`: `(target: number, start: number, end?: number) => [options: McpAppRendererOptions]`

- `entries`: `() => ArrayIterator<[number, McpAppRendererOptions]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<McpAppRendererOptions>`

- `includes`: `(searchElement: McpAppRendererOptions, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => McpAppRendererOptions`

- `findLast`: `{ <S>(predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => unknown, thisArg?: any): McpAppRendererOptions | undefined; }`

- `findLastIndex`: `(predicate: (value: McpAppRendererOptions, index: number, array: McpAppRendererOptions[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => McpAppRendererOptions[]`

- `toSorted`: `(compareFn?: ((a: McpAppRendererOptions, b: McpAppRendererOptions) => number) | undefined) => McpAppRendererOptions[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: McpAppRendererOptions[]): McpAppRendererOptions[]; (start: number, deleteCount?: number): McpAppRendererOptions[]; }`

- `with`: `(index: number, value: McpAppRendererOptions) => McpAppRendererOptions[]`

### McpAppsRemoteHost

- `0`: `McpAppsRemoteHostOptions`

  - `url`: `string`
  - `fetch?`: `typeof fetch`
  - `headers?`: `Record<string, string> | (() => Record<string, string> | Promise<Record<string, string>>)`

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => McpAppsRemoteHostOptions`

- `push`: `(...items: McpAppsRemoteHostOptions[]) => number`

- `concat`: `{ (...items: ConcatArray<McpAppsRemoteHostOptions>[]): McpAppsRemoteHostOptions[]; (...items: (McpAppsRemoteHostOptions | ConcatArray<McpAppsRemoteHostOptions>)[]): McpAppsRemoteHostOptions[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => McpAppsRemoteHostOptions[]`

- `shift`: `() => McpAppsRemoteHostOptions`

- `slice`: `(start?: number, end?: number) => McpAppsRemoteHostOptions[]`

- `sort`: `(compareFn?: ((a: McpAppsRemoteHostOptions, b: McpAppsRemoteHostOptions) => number) | undefined) => [options: McpAppsRemoteHostOptions]`

- `splice`: `{ (start: number, deleteCount?: number): McpAppsRemoteHostOptions[]; (start: number, deleteCount: number, ...items: McpAppsRemoteHostOptions[]): McpAppsRemoteHostOptions[]; }`

- `unshift`: `(...items: McpAppsRemoteHostOptions[]) => number`

- `indexOf`: `(searchElement: McpAppsRemoteHostOptions, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: McpAppsRemoteHostOptions, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => value is S, thisArg?: any): S[]; (predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any): McpAppsRemoteHostOptions[]; }`

- `reduce`: `{ (callbackfn: (previousValue: McpAppsRemoteHostOptions, currentValue: McpAppsRemoteHostOptions, currentIndex: number, array: McpAppsRemoteHostOptions[]) => McpAppsRemoteHostOptions): McpAppsRemoteHostOptions; (callbackfn: (previousValue: McpAppsRemoteHostOptions, currentValue: McpAppsRemoteHostOptions, currentIndex: number, array: McpAppsRemoteHostOptions[]) => McpAppsRemoteHostOptions, initialValue: McpAppsRemoteHostOptions): McpAppsRemoteHostOptions; <U>(callbackfn: (previousValue: U, currentValue: McpAppsRemoteHostOptions, currentIndex: number, array: McpAppsRemoteHostOptions[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: McpAppsRemoteHostOptions, currentValue: McpAppsRemoteHostOptions, currentIndex: number, array: McpAppsRemoteHostOptions[]) => McpAppsRemoteHostOptions): McpAppsRemoteHostOptions; (callbackfn: (previousValue: McpAppsRemoteHostOptions, currentValue: McpAppsRemoteHostOptions, currentIndex: number, array: McpAppsRemoteHostOptions[]) => McpAppsRemoteHostOptions, initialValue: McpAppsRemoteHostOptions): McpAppsRemoteHostOptions; <U>(callbackfn: (previousValue: U, currentValue: McpAppsRemoteHostOptions, currentIndex: number, array: McpAppsRemoteHostOptions[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: McpAppsRemoteHostOptions, index: number, obj: McpAppsRemoteHostOptions[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: McpAppsRemoteHostOptions, index: number, obj: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any): McpAppsRemoteHostOptions | undefined; }`

- `findIndex`: `(predicate: (value: McpAppsRemoteHostOptions, index: number, obj: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: McpAppsRemoteHostOptions, start?: number, end?: number) => [options: McpAppsRemoteHostOptions]`

- `copyWithin`: `(target: number, start: number, end?: number) => [options: McpAppsRemoteHostOptions]`

- `entries`: `() => ArrayIterator<[number, McpAppsRemoteHostOptions]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<McpAppsRemoteHostOptions>`

- `includes`: `(searchElement: McpAppsRemoteHostOptions, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => McpAppsRemoteHostOptions`

- `findLast`: `{ <S>(predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any): McpAppsRemoteHostOptions | undefined; }`

- `findLastIndex`: `(predicate: (value: McpAppsRemoteHostOptions, index: number, array: McpAppsRemoteHostOptions[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => McpAppsRemoteHostOptions[]`

- `toSorted`: `(compareFn?: ((a: McpAppsRemoteHostOptions, b: McpAppsRemoteHostOptions) => number) | undefined) => McpAppsRemoteHostOptions[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: McpAppsRemoteHostOptions[]): McpAppsRemoteHostOptions[]; (start: number, deleteCount?: number): McpAppsRemoteHostOptions[]; }`

- `with`: `(index: number, value: McpAppsRemoteHostOptions) => McpAppsRemoteHostOptions[]`

### useAssistantDataUI

Registers a renderer for named `data` message parts while the component is mounted.

- `dataUI`: `AssistantDataUIProps<any> | null`

  - `name`: `string` — Data part name this renderer handles.
  - `render`: `DataMessagePartComponent<T>` — Component rendered for matching data message parts.

### makeAssistantToolUI

> [!warn]
>
> **Deprecated.** Put `render`/`renderText` on the matching toolkit entry, or use `MessagePrimitive.Parts` inline tool render overrides for per-message UI. See <https://assistant-ui.com/docs/migrations/toolkit-tools>.

Creates a React component that registers a tool-call renderer when rendered.

Use this to package reusable display components for tools whose definitions are registered elsewhere.

```
type AssistantToolUIProps = {
  /** Name of the tool whose calls should use this renderer. */
  toolName: string;
  /** Component rendered for matching tool-call message parts. */
  render: ToolCallMessagePartComponent<TArgs, TResult>;
  /**
   * How the UI is presented relative to the chain-of-thought trace. Set
   * `"standalone"` to surface it on its own (e.g. human-in-the-loop or
   * generative UI for a backend/MCP tool). Defaults to `"inline"`.
   */
  display?: "standalone" | "inline";
};

const makeAssistantToolUI: <TArgs, TResult>(tool: AssistantToolUIProps<TArgs, TResult>) => AssistantToolUI;
```

### useAssistantToolUI

> [!warn]
>
> **Deprecated.** Put `render`/`renderText` on the matching toolkit entry, or use `MessagePrimitive.Parts` inline tool render overrides for per-message UI. See <https://assistant-ui.com/docs/migrations/toolkit-tools>.

Registers a tool-call renderer while the component is mounted.

This only affects rendering. Pair it with [Tools](/docs/api-reference/tools/toolkits#tools) or a backend tool registry to expose the actual tool definition to the model.

- `tool`: `AssistantToolUIProps<any, any> | null`

  - `toolName`: `string` — Name of the tool whose calls should use this renderer.
  - `render`: `ToolCallMessagePartComponent<TArgs, TResult>` — Component rendered for matching tool-call message parts.
  - `display?`: `"standalone" | "inline"` — How the UI is presented relative to the chain-of-thought trace. Set \`"standalone"\` to surface it on its own (e.g. human-in-the-loop or generative UI for a backend/MCP tool). Defaults to \`"inline"\`.