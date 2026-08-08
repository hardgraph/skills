# ThreadListItemPrimitive
URL: /docs/api-reference/primitives/thread-list-item

Thread list item primitives for rendering selectable conversation rows with titles, archive controls, delete actions, and menus.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Anatomy

```
import { ThreadListItemPrimitive } from "@assistant-ui/react";

const ThreadListItem = () => (
  <ThreadListItemPrimitive.Root>
    <ThreadListItemPrimitive.Trigger>
      <ThreadListItemPrimitive.Title />
    </ThreadListItemPrimitive.Trigger>
    <ThreadListItemPrimitive.Archive />
    <ThreadListItemPrimitive.Unarchive />
    <ThreadListItemPrimitive.Delete />
  </ThreadListItemPrimitive.Root>
);
```

## API Reference

### Root

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

| Data attribute  | Values                                   |
| --------------- | ---------------------------------------- |
| `[data-active]` | Present when this is the current thread. |

### Archive

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Unarchive

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Delete

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Trigger

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Title

- `fallback?`: `ReactNode`