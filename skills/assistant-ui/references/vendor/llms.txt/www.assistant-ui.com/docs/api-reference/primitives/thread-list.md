# ThreadListPrimitive
URL: /docs/api-reference/primitives/thread-list

Thread list primitives for rendering conversation navigation, new thread actions, and custom assistant sidebars.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [ThreadList](/docs/primitives/thread-list).

## Anatomy

```
import { ThreadListPrimitive } from "@assistant-ui/react";

const ThreadList = () => (
  <ThreadListPrimitive.Root>
    <ThreadListPrimitive.New />
    <ThreadListPrimitive.Items>
      {() => <MyThreadListItem />}
    </ThreadListPrimitive.Items>
  </ThreadListPrimitive.Root>
);
```

## API Reference

### Root

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### New

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

| Data attribute  | Values                                                                  |
| --------------- | ----------------------------------------------------------------------- |
| `[data-active]` | Present when the new-thread button represents the current fresh thread. |

### Items

- `archived?`: `boolean`
- `components?`: `ThreadListItemsComponentConfig` (deprecated: Use the children render function instead.)
  - `ThreadListItem`: `ComponentType`
- `children?`: `(value: { threadListItem: ThreadListItemState }) => ReactNode` — Render function called for each thread list item. Receives the item.

### ItemByIndex

Renders a single thread list item at the specified index.

- `index`: `number`
- `archived?`: `boolean`
- `components`: `ThreadListItemsComponentConfig`
  - `ThreadListItem`: `ComponentType`

### LoadMore

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`