# ErrorPrimitive
URL: /docs/api-reference/primitives/error

Error primitives for rendering assistant-ui runtime, thread, and message failures inside custom chat interfaces.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [Error](/docs/primitives/error).

## Anatomy

```
import { ErrorPrimitive } from "@assistant-ui/react";

const ErrorDisplay = () => (
  <ErrorPrimitive.Root>
    <ErrorPrimitive.Message />
  </ErrorPrimitive.Root>
);
```

## API Reference

### Root

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Message

This primitive renders a `<span>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`