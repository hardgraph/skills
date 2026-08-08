# ActionBarMorePrimitive
URL: /docs/api-reference/primitives/action-bar-more

Overflow menu primitives for grouping secondary assistant message actions in a custom React UI.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Anatomy

```
import { ActionBarPrimitive, ActionBarMorePrimitive } from "@assistant-ui/react";

const MessageActions = () => (
  <ActionBarPrimitive.Root>
    <ActionBarPrimitive.Copy />
    <ActionBarPrimitive.Reload />

    <ActionBarMorePrimitive.Root>
      <ActionBarMorePrimitive.Trigger>
        <MoreHorizontalIcon />
      </ActionBarMorePrimitive.Trigger>
      <ActionBarMorePrimitive.Content>
        <ActionBarMorePrimitive.Item onSelect={() => console.log("Edit")}>
          Edit
        </ActionBarMorePrimitive.Item>
        <ActionBarMorePrimitive.Item onSelect={() => console.log("Speak")}>
          Read aloud
        </ActionBarMorePrimitive.Item>
        <ActionBarMorePrimitive.Separator />
        <ActionBarMorePrimitive.Item onSelect={() => console.log("Feedback")}>
          Submit feedback
        </ActionBarMorePrimitive.Item>
      </ActionBarMorePrimitive.Content>
    </ActionBarMorePrimitive.Root>
  </ActionBarPrimitive.Root>
);
```

## API Reference

### Root

- `dir?`: `Direction`
- `open?`: `boolean`
- `defaultOpen?`: `boolean`
- `onOpenChange?`: `(open: boolean) => void`
- `modal?`: `boolean`

### Trigger

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

| Data attribute    | Values                |
| ----------------- | --------------------- |
| `[data-state]`    | `"open" \| "closed"`  |
| `[data-disabled]` | Present when disabled |

### Content

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.

- `side?`: `Side`

- `sideOffset?`: `number`

- `align?`: `Align`

- `alignOffset?`: `number`

- `arrowPadding?`: `number`

- `avoidCollisions?`: `boolean`

- `collisionBoundary?`: `Boundary | Boundary[]`

- `collisionPadding?`: `number | Partial<Record<Side, number>>`

- `sticky?`: `'partial' | 'always'`

- `hideWhenDetached?`: `boolean`

- `updatePositionStrategy?`: `'optimized' | 'always'`

- `onCloseAutoFocus?`: `FocusScopeProps['onUnmountAutoFocus']` — Event handler called when auto-focusing on close. Can be prevented.

- `loop?`: `RovingFocusGroupProps['loop']` — Whether keyboard navigation should loop around

- `onEscapeKeyDown?`: `DismissableLayerProps['onEscapeKeyDown']`

- `onPointerDownOutside?`: `DismissableLayerProps['onPointerDownOutside']`

- `onFocusOutside?`: `DismissableLayerProps['onFocusOutside']`

- `onInteractOutside?`: `DismissableLayerProps['onInteractOutside']`

- `forceMount?`: `true` — Used to force mounting when more control is needed. Useful when controlling animation with React animation libraries.

- `render?`: `ReactElement`

- `portalProps?`: `ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Portal>`

  - `container?`: `PortalProps['container']` — Specify a container element to portal the content into.
  - `forceMount?`: `true` — Used to force mounting when more control is needed. Useful when controlling animation with React animation libraries.

| Data attribute | Values                                   |
| -------------- | ---------------------------------------- |
| `[data-state]` | `"open" \| "closed"`                     |
| `[data-side]`  | `"top" \| "right" \| "bottom" \| "left"` |
| `[data-align]` | `"start" \| "center" \| "end"`           |

### Item

This primitive renders a `<div>` element unless `asChild` is set.

- `disabled?`: `boolean`
- `onSelect?`: `(event: Event) => void`
- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `textValue?`: `string`
- `render?`: `ReactElement`

| Data attribute       | Values                   |
| -------------------- | ------------------------ |
| `[data-disabled]`    | Present when disabled    |
| `[data-highlighted]` | Present when highlighted |

### Separator

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`