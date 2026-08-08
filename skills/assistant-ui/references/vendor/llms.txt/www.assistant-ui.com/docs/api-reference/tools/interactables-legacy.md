# Interactables (legacy)
URL: /docs/api-reference/tools/interactables-legacy

Deprecated legacy interactables APIs for registering model-editable components with per-instance update tools.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### Interactables

> [!warn]
>
> **Deprecated.** Since 2026-06-14 — migrate to the Unstable / Experimental API. Scheduled for removal on/after 2026-09-14. See [Interactables migration guide](https://www.assistant-ui.com/docs/tools/interactables#migrating-from-the-previous-api).

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

### useAssistantInteractable

> [!warn]
>
> **Deprecated.** Since 2026-06-14 — migrate to the Unstable / Experimental API. Scheduled for removal on/after 2026-09-14. See [Interactables migration guide](https://www.assistant-ui.com/docs/tools/interactables#migrating-from-the-previous-api).

Registers an interactable with the AI assistant.

This hook handles registration only. To read and write the interactable's state, use [useInteractableState](/docs/api-reference/tools/interactables-legacy#useinteractablestate) with the returned id.

- `name`: `string`

- `config`: `AssistantInteractableProps`

  - `description`: `string`
  - `stateSchema`: `InteractableStateSchema`
  - `initialState`: `unknown`
  - `id?`: `string`
  - `selected?`: `boolean`

### useInteractableState

> [!warn]
>
> **Deprecated.** Since 2026-06-14 — migrate to the Unstable / Experimental API. Scheduled for removal on/after 2026-09-14. See [Interactables migration guide](https://www.assistant-ui.com/docs/tools/interactables#migrating-from-the-previous-api).

Reads and writes the state of a registered interactable.

Pair with [useAssistantInteractable](/docs/api-reference/tools/interactables-legacy#useassistantinteractable) which handles registration.

- `id`: `string`
- `fallback`: `TState`