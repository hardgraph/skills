# ThreadListItemMorePrimitive
URL: /docs/api-reference/primitives/thread-list-item-more

Overflow menu primitives for secondary thread list item actions in custom assistant-ui sidebars.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Anatomy

```
import {
  ThreadListItemPrimitive,
  ThreadListItemMorePrimitive
} from "@assistant-ui/react";

const ThreadListItemMore = () => (
  <ThreadListItemMorePrimitive.Root>
    <ThreadListItemMorePrimitive.Trigger>
      More options
    </ThreadListItemMorePrimitive.Trigger>
    <ThreadListItemMorePrimitive.Content>
      <ThreadListItemPrimitive.Archive asChild>
        <ThreadListItemMorePrimitive.Item>
          Archive
        </ThreadListItemMorePrimitive.Item>
      </ThreadListItemPrimitive.Archive>
      <ThreadListItemMorePrimitive.Separator />
      <ThreadListItemPrimitive.Delete asChild>
        <ThreadListItemMorePrimitive.Item>
          Delete
        </ThreadListItemMorePrimitive.Item>
      </ThreadListItemPrimitive.Delete>
    </ThreadListItemMorePrimitive.Content>
  </ThreadListItemMorePrimitive.Root>
);
```

## API Reference

### Root

Root container for the overflow menu, built on Radix DropdownMenu. Defaults to a standard, self-contained modal dropdown. Pass ThreadListItemMorePrimitiveRoot.Props.sharedFocusGroup to fold it into the thread list's keyboard navigation instead.

- `dir?`: `Direction`
- `open?`: `boolean`
- `defaultOpen?`: `boolean`
- `onOpenChange?`: `(open: boolean) => void`
- `modal?`: `boolean`
- `sharedFocusGroup?`: `boolean` — Join the menu to the thread list's focus group: Right opens it, Left/Escape close it and return focus to the trigger (mirrored in RTL). Forces the menu non-modal, since a focus trap can't let focus cross the trigger/menu boundary. Defaults to a standalone modal dropdown.

### Trigger

This primitive renders a `<button>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

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

### Item

This primitive renders a `<div>` element unless `asChild` is set.

- `disabled?`: `boolean`
- `onSelect?`: `(event: Event) => void`
- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `textValue?`: `string`
- `render?`: `ReactElement`

### Separator

This primitive renders a `<div>` element unless `asChild` is set.

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`