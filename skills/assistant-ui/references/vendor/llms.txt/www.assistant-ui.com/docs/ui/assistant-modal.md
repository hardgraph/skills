# AssistantModal
URL: /docs/ui/assistant-modal

Floating chat bubble for support widgets and help desks.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A floating chat modal built on the popover primitive of your configured style (Radix UI Popover on Radix styles, Base UI Popover on `base-*` styles). Ideal for support widgets, help desks, and embedded assistants.

\[interactive preview omitted]

## Getting Started

1. ### Add `assistant-modal`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/assistant-modal
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/assistant-modal.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-markdown class-variance-authority remark-gfm tw-shimmer zustand
   ```

   ```bash
   npx shadcn@latest add button dialog tooltip avatar collapsible popover
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/assistant-modal.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/assistant-modal.tsx)
   - [components/assistant-ui/thread.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/thread.tsx)
   - [components/assistant-ui/attachment.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/attachment.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)
   - [components/assistant-ui/follow-up-suggestions.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx)
   - [components/assistant-ui/markdown-text.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/markdown-text.tsx)
   - [components/assistant-ui/reasoning.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/reasoning.tsx)
   - [components/assistant-ui/tool-fallback.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx)
   - [components/assistant-ui/tool-group.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-group.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/assistant-modal.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/assistant-modal.tsx \
     -o components/assistant-ui/thread.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread.tsx \
     -o components/assistant-ui/attachment.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/attachment.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx \
     -o components/assistant-ui/follow-up-suggestions.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx \
     -o components/assistant-ui/markdown-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/markdown-text.tsx \
     -o components/assistant-ui/reasoning.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/reasoning.tsx \
     -o components/assistant-ui/tool-fallback.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx \
     -o components/assistant-ui/tool-group.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-group.tsx
   ```

   This adds `/components/assistant-ui/assistant-modal.tsx` to your project, which you can adjust as needed.

2. ### Use in your application

   ```
   import { AssistantModal } from "@/components/assistant-ui/assistant-modal";

   export default function Home() {
     return (
       <div className="h-full">
         <AssistantModal />
       </div>
     );
   }
   ```

## Anatomy

The Radix flavor of `AssistantModal` is built with the following primitives. The Base UI flavor composes your project's Base UI Popover instead.

```
import { AssistantModalPrimitive } from "@assistant-ui/react";

<AssistantModalPrimitive.Root>
  <AssistantModalPrimitive.Anchor />
  <AssistantModalPrimitive.Trigger />
  <AssistantModalPrimitive.Content>
    {/* Thread component goes here */}
  </AssistantModalPrimitive.Content>
</AssistantModalPrimitive.Root>
```

## API Reference

### Root

Contains all parts of the modal when using `AssistantModalPrimitive` (the Radix flavor). The Base UI flavor uses your project's Base UI Popover instead.

- `defaultOpen?`: `boolean` — The initial open state when uncontrolled.
- `open?`: `boolean` — The controlled open state.
- `onOpenChange?`: `(open: boolean) => void` — Callback when the open state changes.
- `unstable_openOnRunStart?`: `boolean` — Automatically open the modal when the assistant starts running.

### Trigger

A button that toggles the modal open/closed state.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

This primitive renders a `<button>` element unless `asChild` is set.

### Content

The popover content container that holds the chat interface.

- `side`: `'top' | 'right' | 'bottom' | 'left'` (default `'top'`) — The preferred side of the anchor to render against.
- `align`: `'start' | 'center' | 'end'` (default `'end'`) — The preferred alignment against the anchor.
- `dissmissOnInteractOutside?`: `boolean` — Whether to close the modal when clicking outside.
- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

### Anchor

An optional anchor element to position the modal relative to.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

## Related Components

- [Thread](/docs/ui/thread) - The main chat interface used inside the modal
- [AssistantSidebar](/docs/ui/assistant-sidebar) - Alternative layout for side panel chat