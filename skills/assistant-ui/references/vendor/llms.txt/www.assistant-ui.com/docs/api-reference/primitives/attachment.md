# AttachmentPrimitive
URL: /docs/api-reference/primitives/attachment

Attachment primitives for rendering file previews, names, thumbnails, and remove controls in assistant-ui messages and composers.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [Attachment](/docs/primitives/attachment).

## Anatomy

```
import { AttachmentPrimitive } from "@assistant-ui/react";

const MyMessageAttachment = () => (
  <AttachmentPrimitive.Root>
    <AttachmentPrimitive.unstable_Thumb />
    <AttachmentPrimitive.Name />
  </AttachmentPrimitive.Root>
);

const MyComposerAttachment = () => (
  <AttachmentPrimitive.Root>
    <AttachmentPrimitive.unstable_Thumb />
    <AttachmentPrimitive.Name />
    <AttachmentPrimitive.Remove />
  </AttachmentPrimitive.Root>
);
```

## API Reference

### Root

The root container component for an attachment. This component provides the foundational wrapper for attachment-related components and content. It serves as the context provider for attachment state and actions.

This primitive renders a `<div>` element unless `asChild` is set.

```
<AttachmentPrimitive.Root>
  <AttachmentPrimitive.Name />
  <AttachmentPrimitive.Remove />
</AttachmentPrimitive.Root>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Name



### Remove

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`