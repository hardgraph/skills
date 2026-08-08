# Utilities
URL: /docs/api-reference/utilities/miscellaneous

Miscellaneous @assistant-ui/react utilities for custom rendering, composition, and advanced assistant UI behavior.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### AssistantCloud

- `constructor?`: `(config: AssistantCloudConfig) => AssistantCloud`
- `threads?`: `AssistantCloudThreads`
- `projects?`: `AssistantCloudProjects`
- `auth?`: `{ tokens: AssistantCloudAuthTokens; }`
- `runs?`: `AssistantCloudRuns`
- `files?`: `AssistantCloudFiles`
- `telemetry?`: `AssistantCloudTelemetryConfig`

### AuiConfig

Builds a config for [AuiProvider](/docs/api-reference/context-providers/assistant-runtime-provider#auiprovider); the `config` prop only accepts configs built with this helper.

A config is plain data: it can be hoisted to module scope, created inline per render, or memoized — the provider never relies on config identity.

```
const aui = useAui();
const config = AuiConfig({
  message: Derived({
    source: "thread",
    query: { index: 0 },
    get: (aui) => aui.thread.message({ index: 0 }),
  }),
});

<AuiProvider extends={aui} config={config}>{children}</AuiProvider>;
```

- `thread?`: `ClientElement<"thread"> | DerivedElement<"thread">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `message?`: `ClientElement<"message"> | DerivedElement<"message">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `threads?`: `ClientElement<"threads"> | DerivedElement<"threads">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `threadListItem?`: `ClientElement<"threadListItem"> | DerivedElement<"threadListItem">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `part?`: `ClientElement<"part"> | DerivedElement<"part">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `composer?`: `ClientElement<"composer"> | DerivedElement<"composer">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `attachment?`: `ClientElement<"attachment"> | DerivedElement<"attachment">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `modelContext?`: `ClientElement<"modelContext"> | DerivedElement<"modelContext">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `suggestions?`: `ClientElement<"suggestions"> | DerivedElement<"suggestions">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `suggestion?`: `ClientElement<"suggestion"> | DerivedElement<"suggestion">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `chainOfThought?`: `ClientElement<"chainOfThought"> | DerivedElement<"chainOfThought">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `queueItem?`: `ClientElement<"queueItem"> | DerivedElement<"queueItem">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `tools?`: `ClientElement<"tools"> | DerivedElement<"tools">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `dataRenderers?`: `ClientElement<"dataRenderers"> | DerivedElement<"dataRenderers">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `interactables?`: `ClientElement<"interactables"> | DerivedElement<"interactables">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

- `unstable_interactables?`: `ClientElement<"unstable_interactables"> | DerivedElement<"unstable_interactables">`

  - `hook`: `(...args: any[]) => V`
  - `args`: `readonly unknown[]`
  - `key?`: `string | number`
  - `deps?`: `readonly unknown[]`

### ChainOfThoughtClient

- `0`: `ChainOfThoughtClient props["0"]`

  - `parts`: `readonly ChainOfThoughtPart[]`
  - `getMessagePart`: `(selector: { index: number }) => PartMethods`

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }`

- `push`: `(...items: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => number`

- `concat`: `{ (...items: ConcatArray<{ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }>[]): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; (...items: ({ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; } | ConcatArray<{ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }>)[]): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]`

- `shift`: `() => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }`

- `slice`: `(start?: number, end?: number) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]`

- `sort`: `(compareFn?: ((a: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, b: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }) => number) | undefined) => [{ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }]`

- `splice`: `{ (start: number, deleteCount?: number): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; (start: number, deleteCount: number, ...items: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; }`

- `unshift`: `(...items: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => number`

- `indexOf`: `(searchElement: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => value is S, thisArg?: any): S[]; (predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; }`

- `reduce`: `{ (callbackfn: (previousValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentIndex: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }; (callbackfn: (previousValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentIndex: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, initialValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }; <U>(callbackfn: (previousValue: U, currentValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentIndex: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentIndex: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }; (callbackfn: (previousValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentIndex: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, initialValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }; <U>(callbackfn: (previousValue: U, currentValue: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, currentIndex: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, obj: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, obj: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; } | undefined; }`

- `findIndex`: `(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, obj: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, start?: number, end?: number) => [{ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }]`

- `copyWithin`: `(target: number, start: number, end?: number) => [{ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }]`

- `entries`: `() => ArrayIterator<[number, { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<{ parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }>`

- `includes`: `(searchElement: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }`

- `findLast`: `{ <S>(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; } | undefined; }`

- `findLastIndex`: `(predicate: (value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, index: number, array: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]`

- `toSorted`: `(compareFn?: ((a: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }, b: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }) => number) | undefined) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; (start: number, deleteCount?: number): { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]; }`

- `with`: `(index: number, value: { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }) => { parts: readonly ChainOfThoughtPart[]; getMessagePart: (selector: { index: number; }) => PartMethods; }[]`

### createMessageQueue

```
const createMessageQueue: (driver: MessageQueueDriver) => MessageQueueController;
```

### DevToolsHooks

- `static subscribe?`: `(listener: () => void) => Unsubscribe`
- `static clearEventLogs?`: `(apiId: number) => void`
- `static getApis?`: `() => Map<number, DevToolsApiEntry>`

### InMemoryThreadList

- `0`: `InMemoryThreadListProps`

  - `thread`: `(threadId: string) => ResourceElement<ClientOutput<"thread">>`
  - `onSwitchToThread?`: `(threadId: string) => void`
  - `onSwitchToNewThread?`: `() => void`

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => InMemoryThreadListProps`

- `push`: `(...items: InMemoryThreadListProps[]) => number`

- `concat`: `{ (...items: ConcatArray<InMemoryThreadListProps>[]): InMemoryThreadListProps[]; (...items: (InMemoryThreadListProps | ConcatArray<InMemoryThreadListProps>)[]): InMemoryThreadListProps[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => InMemoryThreadListProps[]`

- `shift`: `() => InMemoryThreadListProps`

- `slice`: `(start?: number, end?: number) => InMemoryThreadListProps[]`

- `sort`: `(compareFn?: ((a: InMemoryThreadListProps, b: InMemoryThreadListProps) => number) | undefined) => [props: InMemoryThreadListProps]`

- `splice`: `{ (start: number, deleteCount?: number): InMemoryThreadListProps[]; (start: number, deleteCount: number, ...items: InMemoryThreadListProps[]): InMemoryThreadListProps[]; }`

- `unshift`: `(...items: InMemoryThreadListProps[]) => number`

- `indexOf`: `(searchElement: InMemoryThreadListProps, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: InMemoryThreadListProps, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => value is S, thisArg?: any): S[]; (predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => unknown, thisArg?: any): InMemoryThreadListProps[]; }`

- `reduce`: `{ (callbackfn: (previousValue: InMemoryThreadListProps, currentValue: InMemoryThreadListProps, currentIndex: number, array: InMemoryThreadListProps[]) => InMemoryThreadListProps): InMemoryThreadListProps; (callbackfn: (previousValue: InMemoryThreadListProps, currentValue: InMemoryThreadListProps, currentIndex: number, array: InMemoryThreadListProps[]) => InMemoryThreadListProps, initialValue: InMemoryThreadListProps): InMemoryThreadListProps; <U>(callbackfn: (previousValue: U, currentValue: InMemoryThreadListProps, currentIndex: number, array: InMemoryThreadListProps[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: InMemoryThreadListProps, currentValue: InMemoryThreadListProps, currentIndex: number, array: InMemoryThreadListProps[]) => InMemoryThreadListProps): InMemoryThreadListProps; (callbackfn: (previousValue: InMemoryThreadListProps, currentValue: InMemoryThreadListProps, currentIndex: number, array: InMemoryThreadListProps[]) => InMemoryThreadListProps, initialValue: InMemoryThreadListProps): InMemoryThreadListProps; <U>(callbackfn: (previousValue: U, currentValue: InMemoryThreadListProps, currentIndex: number, array: InMemoryThreadListProps[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: InMemoryThreadListProps, index: number, obj: InMemoryThreadListProps[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: InMemoryThreadListProps, index: number, obj: InMemoryThreadListProps[]) => unknown, thisArg?: any): InMemoryThreadListProps | undefined; }`

- `findIndex`: `(predicate: (value: InMemoryThreadListProps, index: number, obj: InMemoryThreadListProps[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: InMemoryThreadListProps, start?: number, end?: number) => [props: InMemoryThreadListProps]`

- `copyWithin`: `(target: number, start: number, end?: number) => [props: InMemoryThreadListProps]`

- `entries`: `() => ArrayIterator<[number, InMemoryThreadListProps]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<InMemoryThreadListProps>`

- `includes`: `(searchElement: InMemoryThreadListProps, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => InMemoryThreadListProps`

- `findLast`: `{ <S>(predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => unknown, thisArg?: any): InMemoryThreadListProps | undefined; }`

- `findLastIndex`: `(predicate: (value: InMemoryThreadListProps, index: number, array: InMemoryThreadListProps[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => InMemoryThreadListProps[]`

- `toSorted`: `(compareFn?: ((a: InMemoryThreadListProps, b: InMemoryThreadListProps) => number) | undefined) => InMemoryThreadListProps[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: InMemoryThreadListProps[]): InMemoryThreadListProps[]; (start: number, deleteCount?: number): InMemoryThreadListProps[]; }`

- `with`: `(index: number, value: InMemoryThreadListProps) => InMemoryThreadListProps[]`

### SingleThreadList

- `0`: `SingleThreadListProps`

  - `thread`: `ClientElement<"thread">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => { thread: ClientElement<"thread">; }`

- `push`: `(...items: { thread: ClientElement<"thread">; }[]) => number`

- `concat`: `{ (...items: ConcatArray<{ thread: ClientElement<"thread">; }>[]): { thread: ClientElement<"thread">; }[]; (...items: ({ thread: ClientElement<"thread">; } | ConcatArray<{ thread: ClientElement<"thread">; }>)[]): { thread: ClientElement<"thread">; }[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => { thread: ClientElement<"thread">; }[]`

- `shift`: `() => { thread: ClientElement<"thread">; }`

- `slice`: `(start?: number, end?: number) => { thread: ClientElement<"thread">; }[]`

- `sort`: `(compareFn?: ((a: { thread: ClientElement<"thread">; }, b: { thread: ClientElement<"thread">; }) => number) | undefined) => [{ thread: ClientElement<"thread">; }]`

- `splice`: `{ (start: number, deleteCount?: number): { thread: ClientElement<"thread">; }[]; (start: number, deleteCount: number, ...items: { thread: ClientElement<"thread">; }[]): { thread: ClientElement<"thread">; }[]; }`

- `unshift`: `(...items: { thread: ClientElement<"thread">; }[]) => number`

- `indexOf`: `(searchElement: { thread: ClientElement<"thread">; }, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: { thread: ClientElement<"thread">; }, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => value is S, thisArg?: any): S[]; (predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any): { thread: ClientElement<"thread">; }[]; }`

- `reduce`: `{ (callbackfn: (previousValue: { thread: ClientElement<"thread">; }, currentValue: { thread: ClientElement<"thread">; }, currentIndex: number, array: { thread: ClientElement<"thread">; }[]) => { thread: ClientElement<"thread">; }): { thread: ClientElement<"thread">; }; (callbackfn: (previousValue: { thread: ClientElement<"thread">; }, currentValue: { thread: ClientElement<"thread">; }, currentIndex: number, array: { thread: ClientElement<"thread">; }[]) => { thread: ClientElement<"thread">; }, initialValue: { thread: ClientElement<"thread">; }): { thread: ClientElement<"thread">; }; <U>(callbackfn: (previousValue: U, currentValue: { thread: ClientElement<"thread">; }, currentIndex: number, array: { thread: ClientElement<"thread">; }[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: { thread: ClientElement<"thread">; }, currentValue: { thread: ClientElement<"thread">; }, currentIndex: number, array: { thread: ClientElement<"thread">; }[]) => { thread: ClientElement<"thread">; }): { thread: ClientElement<"thread">; }; (callbackfn: (previousValue: { thread: ClientElement<"thread">; }, currentValue: { thread: ClientElement<"thread">; }, currentIndex: number, array: { thread: ClientElement<"thread">; }[]) => { thread: ClientElement<"thread">; }, initialValue: { thread: ClientElement<"thread">; }): { thread: ClientElement<"thread">; }; <U>(callbackfn: (previousValue: U, currentValue: { thread: ClientElement<"thread">; }, currentIndex: number, array: { thread: ClientElement<"thread">; }[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: { thread: ClientElement<"thread">; }, index: number, obj: { thread: ClientElement<"thread">; }[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: { thread: ClientElement<"thread">; }, index: number, obj: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any): { thread: ClientElement<"thread">; } | undefined; }`

- `findIndex`: `(predicate: (value: { thread: ClientElement<"thread">; }, index: number, obj: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: { thread: ClientElement<"thread">; }, start?: number, end?: number) => [{ thread: ClientElement<"thread">; }]`

- `copyWithin`: `(target: number, start: number, end?: number) => [{ thread: ClientElement<"thread">; }]`

- `entries`: `() => ArrayIterator<[number, { thread: ClientElement<"thread">; }]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<{ thread: ClientElement<"thread">; }>`

- `includes`: `(searchElement: { thread: ClientElement<"thread">; }, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => { thread: ClientElement<"thread">; }`

- `findLast`: `{ <S>(predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any): { thread: ClientElement<"thread">; } | undefined; }`

- `findLastIndex`: `(predicate: (value: { thread: ClientElement<"thread">; }, index: number, array: { thread: ClientElement<"thread">; }[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => { thread: ClientElement<"thread">; }[]`

- `toSorted`: `(compareFn?: ((a: { thread: ClientElement<"thread">; }, b: { thread: ClientElement<"thread">; }) => number) | undefined) => { thread: ClientElement<"thread">; }[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: { thread: ClientElement<"thread">; }[]): { thread: ClientElement<"thread">; }[]; (start: number, deleteCount?: number): { thread: ClientElement<"thread">; }[]; }`

- `with`: `(index: number, value: { thread: ClientElement<"thread">; }) => { thread: ClientElement<"thread">; }[]`

### Suggestions

- `0?`: `SuggestionConfig[]`
- `length`: `0 | 1`
- `toString`: `() => string`
- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`
- `pop`: `() => SuggestionConfig[]`
- `push`: `(...items: (SuggestionConfig[] | undefined)[]) => number`
- `concat`: `{ (...items: ConcatArray<SuggestionConfig[] | undefined>[]): (SuggestionConfig[] | undefined)[]; (...items: (SuggestionConfig[] | ConcatArray<SuggestionConfig[] | undefined> | undefined)[]): (SuggestionConfig[] | undefined)[]; }`
- `join`: `(separator?: string) => string`
- `reverse`: `() => (SuggestionConfig[] | undefined)[]`
- `shift`: `() => SuggestionConfig[]`
- `slice`: `(start?: number, end?: number) => (SuggestionConfig[] | undefined)[]`
- `sort`: `(compareFn?: ((a: SuggestionConfig[] | undefined, b: SuggestionConfig[] | undefined) => number) | undefined) => [suggestions?: SuggestionConfig[] | undefined]`
- `splice`: `{ (start: number, deleteCount?: number): (SuggestionConfig[] | undefined)[]; (start: number, deleteCount: number, ...items: (SuggestionConfig[] | undefined)[]): (SuggestionConfig[] | undefined)[]; }`
- `unshift`: `(...items: (SuggestionConfig[] | undefined)[]) => number`
- `indexOf`: `(searchElement: SuggestionConfig[] | undefined, fromIndex?: number) => number`
- `lastIndexOf`: `(searchElement: SuggestionConfig[] | undefined, fromIndex?: number) => number`
- `every`: `{ <S>(predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any): boolean; }`
- `some`: `(predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any) => boolean`
- `forEach`: `(callbackfn: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => void, thisArg?: any) => void`
- `map`: `<U>(callbackfn: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => U, thisArg?: any) => U[]`
- `filter`: `{ <S>(predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => value is S, thisArg?: any): S[]; (predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any): (SuggestionConfig[] | undefined)[]; }`
- `reduce`: `{ (callbackfn: (previousValue: SuggestionConfig[] | undefined, currentValue: SuggestionConfig[] | undefined, currentIndex: number, array: (SuggestionConfig[] | undefined)[]) => SuggestionConfig[] | undefined): SuggestionConfig[] | undefined; (callbackfn: (previousValue: SuggestionConfig[] | undefined, currentValue: SuggestionConfig[] | undefined, currentIndex: number, array: (SuggestionConfig[] | undefined)[]) => SuggestionConfig[] | undefined, initialValue: SuggestionConfig[] | undefined): SuggestionConfig[] | undefined; <U>(callbackfn: (previousValue: U, currentValue: SuggestionConfig[] | undefined, currentIndex: number, array: (SuggestionConfig[] | undefined)[]) => U, initialValue: U): U; }`
- `reduceRight`: `{ (callbackfn: (previousValue: SuggestionConfig[] | undefined, currentValue: SuggestionConfig[] | undefined, currentIndex: number, array: (SuggestionConfig[] | undefined)[]) => SuggestionConfig[] | undefined): SuggestionConfig[] | undefined; (callbackfn: (previousValue: SuggestionConfig[] | undefined, currentValue: SuggestionConfig[] | undefined, currentIndex: number, array: (SuggestionConfig[] | undefined)[]) => SuggestionConfig[] | undefined, initialValue: SuggestionConfig[] | undefined): SuggestionConfig[] | undefined; <U>(callbackfn: (previousValue: U, currentValue: SuggestionConfig[] | undefined, currentIndex: number, array: (SuggestionConfig[] | undefined)[]) => U, initialValue: U): U; }`
- `find`: `{ <S>(predicate: (value: SuggestionConfig[] | undefined, index: number, obj: (SuggestionConfig[] | undefined)[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: SuggestionConfig[] | undefined, index: number, obj: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any): SuggestionConfig[] | undefined; }`
- `findIndex`: `(predicate: (value: SuggestionConfig[] | undefined, index: number, obj: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any) => number`
- `fill`: `(value: SuggestionConfig[] | undefined, start?: number, end?: number) => [suggestions?: SuggestionConfig[] | undefined]`
- `copyWithin`: `(target: number, start: number, end?: number) => [suggestions?: SuggestionConfig[] | undefined]`
- `entries`: `() => ArrayIterator<[number, SuggestionConfig[] | undefined]>`
- `keys`: `() => ArrayIterator<number>`
- `values`: `() => ArrayIterator<SuggestionConfig[] | undefined>`
- `includes`: `(searchElement: SuggestionConfig[] | undefined, fromIndex?: number) => boolean`
- `flatMap`: `<U, This>(callback: (this: This, value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => U | readonly U[], thisArg?: This | undefined) => U[]`
- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`
- `at`: `(index: number) => SuggestionConfig[]`
- `findLast`: `{ <S>(predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any): SuggestionConfig[] | undefined; }`
- `findLastIndex`: `(predicate: (value: SuggestionConfig[] | undefined, index: number, array: (SuggestionConfig[] | undefined)[]) => unknown, thisArg?: any) => number`
- `toReversed`: `() => (SuggestionConfig[] | undefined)[]`
- `toSorted`: `(compareFn?: ((a: SuggestionConfig[] | undefined, b: SuggestionConfig[] | undefined) => number) | undefined) => (SuggestionConfig[] | undefined)[]`
- `toSpliced`: `{ (start: number, deleteCount: number, ...items: (SuggestionConfig[] | undefined)[]): (SuggestionConfig[] | undefined)[]; (start: number, deleteCount?: number): (SuggestionConfig[] | undefined)[]; }`
- `with`: `(index: number, value: SuggestionConfig[] | undefined) => (SuggestionConfig[] | undefined)[]`

### unstable\_defaultDirectiveFormatter

Default directive formatter using the `:type[label]{name=id}` syntax.

When `id` equals `label`, the `{name=…}` attribute is omitted for brevity.

```
const unstable_defaultDirectiveFormatter: Unstable_DirectiveFormatter;
```

### useSmooth

Animates streamed message part text with a typewriter-style reveal.

Takes the current part state and a `smooth` argument: `false` disables, `true` uses the default rate, and a SmoothOptions object tunes the reveal. Returns the part state with `text` replaced by the revealed prefix and `status` reporting `running` until the reveal catches up.

The reveal auto-disables under `prefers-reduced-motion: reduce`, committing the full text immediately; this takes precedence over an explicit `smooth={true}`.

```
const { text, status } = useSmooth(useMessagePartText(), {
  drainMs: 500,
  maxCharsPerFrame: 30,
});
```

- `state`: `(TextMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; }) | (ReasoningMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; })`

  - `type`: `"text"`
  - `text`: `string`
  - `status`: `MessagePartStreamStatus`
    - `type`: `"running"`
  - `providerMetadata?`: `PartProviderMetadata`
  - `parentId?`: `string`

- `smooth?`: `boolean | SmoothOptions`

### fromThreadMessageLike

> [!tip]
>
> **Experimental.** This API is experimental and may change without notice.

```
const fromThreadMessageLike: (like: ThreadMessageLike, fallbackId: string, fallbackStatus: MessageStatus) => ThreadMessage;
```

### generateId

> [!tip]
>
> **Experimental.** This API is experimental and may change without notice.

```
const generateId: (size?: number) => string;
```