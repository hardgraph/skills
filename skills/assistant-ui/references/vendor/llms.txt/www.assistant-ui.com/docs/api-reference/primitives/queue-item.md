# QueueItemPrimitive
URL: /docs/api-reference/primitives/queue-item

Queue item primitives for rendering pending assistant-ui thread operations, optimistic work, and runtime queue state.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### Text

Renders the prompt text of a queue item.

This primitive renders a `<span>` element unless `asChild` is set.

```
<QueueItemPrimitive.Text />
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Steer

A button that steers the current run to process this queue item immediately.

This primitive renders a `<button>` element unless `asChild` is set.

```
<QueueItemPrimitive.Steer>Run Now</QueueItemPrimitive.Steer>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Remove

A button that removes this item from the queue.

This primitive renders a `<button>` element unless `asChild` is set.

```
<QueueItemPrimitive.Remove>×</QueueItemPrimitive.Remove>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`