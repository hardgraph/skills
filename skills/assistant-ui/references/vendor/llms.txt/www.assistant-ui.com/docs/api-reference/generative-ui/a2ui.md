# A2UI
URL: /docs/api-reference/generative-ui/a2ui

Convert A2UI surface operations into generative UI specs.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### A2uiOperation

- `version`: `"v0.9"`

### A2uiOperationResult

- `state`: `A2uiState`

  - `forEach`: `(callbackfn: (value: A2uiSurfaceState, key: string, map: ReadonlyMap<string, A2uiSurfaceState>) => void, thisArg?: any) => void`
  - `get`: `(key: string) => A2uiSurfaceState`
  - `has`: `(key: string) => boolean`
  - `size`: `number`
  - `entries`: `() => MapIterator<[string, A2uiSurfaceState]>`
  - `keys`: `() => MapIterator<string>`
  - `values`: `() => MapIterator<A2uiSurfaceState>`

- `warnings`: `string[]`

### A2uiState

- `forEach`: `(callbackfn: (value: A2uiSurfaceState, key: string, map: ReadonlyMap<string, A2uiSurfaceState>) => void, thisArg?: any) => void`
- `get`: `(key: string) => A2uiSurfaceState`
- `has`: `(key: string) => boolean`
- `size`: `number`
- `entries`: `() => MapIterator<[string, A2uiSurfaceState]>`
- `keys`: `() => MapIterator<string>`
- `values`: `() => MapIterator<A2uiSurfaceState>`

### A2uiSurfaceState

- `components`: `Map<string, Record<string, unknown>>`

  - `clear`: `() => void`
  - `delete`: `(key: string) => boolean`
  - `forEach`: `(callbackfn: (value: Record<string, unknown>, key: string, map: Map<string, Record<string, unknown>>) => void, thisArg?: any) => void`
  - `get`: `(key: string) => Record<string, unknown>`
  - `has`: `(key: string) => boolean`
  - `set`: `(key: string, value: Record<string, unknown>) => Map<string, Record<string, unknown>>`
  - `size`: `number`
  - `entries`: `() => MapIterator<[string, Record<string, unknown>]>`
  - `keys`: `() => MapIterator<string>`
  - `values`: `() => MapIterator<Record<string, unknown>>`

- `dataModel`: `unknown`

### applyA2uiOperations

- `state`: `A2uiState`

  - `forEach`: `(callbackfn: (value: A2uiSurfaceState, key: string, map: ReadonlyMap<string, A2uiSurfaceState>) => void, thisArg?: any) => void`
  - `get`: `(key: string) => A2uiSurfaceState`
  - `has`: `(key: string) => boolean`
  - `size`: `number`
  - `entries`: `() => MapIterator<[string, A2uiSurfaceState]>`
  - `keys`: `() => MapIterator<string>`
  - `values`: `() => MapIterator<A2uiSurfaceState>`

- `operations`: `unknown`

### convertSurfaceToUISpec

- `surface`: `A2uiSurfaceState`

  - `components`: `Map<string, Record<string, unknown>>`

    - `clear`: `() => void`
    - `delete`: `(key: string) => boolean`
    - `forEach`: `(callbackfn: (value: Record<string, unknown>, key: string, map: Map<string, Record<string, unknown>>) => void, thisArg?: any) => void`
    - `get`: `(key: string) => Record<string, unknown>`
    - `has`: `(key: string) => boolean`
    - `set`: `(key: string, value: Record<string, unknown>) => Map<string, Record<string, unknown>>`
    - `size`: `number`
    - `entries`: `() => MapIterator<[string, Record<string, unknown>]>`
    - `keys`: `() => MapIterator<string>`
    - `values`: `() => MapIterator<Record<string, unknown>>`

  - `dataModel`: `unknown`