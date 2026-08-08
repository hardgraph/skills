# BranchPickerPrimitive
URL: /docs/api-reference/primitives/branch-picker

Branch picker primitives for navigating regenerated assistant responses and alternate message paths inside a chat thread.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [BranchPicker](/docs/primitives/branch-picker).

## Anatomy

```
import { BranchPickerPrimitive } from "@assistant-ui/react";

const BranchPicker = () => (
  <BranchPickerPrimitive.Root>
    <BranchPickerPrimitive.Previous />
    <BranchPickerPrimitive.Number /> / <BranchPickerPrimitive.Count />
    <BranchPickerPrimitive.Next />
  </BranchPickerPrimitive.Root>
);
```

## API Reference

### Root

The root container for branch picker components. This component provides a container for branch navigation controls, with optional conditional rendering based on the number of available branches. It integrates with the message branching system to allow users to navigate between different response variations.

This primitive renders a `<div>` element unless `asChild` is set.

```
<BranchPickerPrimitive.Root hideWhenSingleBranch={true}>
  <BranchPickerPrimitive.Previous />
  <BranchPickerPrimitive.Count />
  <BranchPickerPrimitive.Next />
</BranchPickerPrimitive.Root>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `hideWhenSingleBranch`: `boolean` (default `false`) — Whether to hide the branch picker when there's only one branch available. When true, the component will only render when multiple branches exist.

### Next

A button component that navigates to the next branch in the message tree. This component automatically handles switching to the next available branch and is disabled when there are no more branches to navigate to.

This primitive renders a `<button>` element unless `asChild` is set.

```
<BranchPickerPrimitive.Next>
  Next →
</BranchPickerPrimitive.Next>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Previous

A button component that navigates to the previous branch in the message tree. This component automatically handles switching to the previous available branch and is disabled when there are no previous branches to navigate to.

This primitive renders a `<button>` element unless `asChild` is set.

```
<BranchPickerPrimitive.Previous>
  ← Previous
</BranchPickerPrimitive.Previous>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Count

A component that displays the total number of branches for the current message. This component renders the branch count as plain text, useful for showing users how many alternative responses are available.

```
<div>
  Branch <BranchPickerPrimitive.Count /> of {totalBranches}
</div>
```



### Number