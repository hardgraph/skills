# Thread List Component
URL: /docs/ui/thread-list

Sidebar or dropdown component for switching between AI chat conversations. Persistent thread state, search, and active selection — built for assistant-ui apps.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

> [!info]
>
> This demo uses `ThreadListSidebar`, which includes `thread-list` as a dependency and provides a complete sidebar layout. For custom implementations, you can use `thread-list` directly.

## Getting Started

1. ### Add the component

   Use `threadlist-sidebar` for a complete sidebar layout or `thread-list` for custom layouts.

   #### ThreadListSidebar

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/threadlist-sidebar
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/threadlist-sidebar.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react
   ```

   ```bash
   npx shadcn@latest add sidebar button input skeleton tooltip
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/threadlist-sidebar.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/threadlist-sidebar.tsx)
   - [components/icons/github.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/icons/github.tsx)
   - [components/assistant-ui/thread-list.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/thread-list.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/threadlist-sidebar.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/threadlist-sidebar.tsx \
     -o components/icons/github.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/icons/github.tsx \
     -o components/assistant-ui/thread-list.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread-list.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx
   ```

   #### ThreadList

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/thread-list
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/thread-list.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react
   ```

   ```bash
   npx shadcn@latest add button input skeleton tooltip
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/thread-list.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/thread-list.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/thread-list.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread-list.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx
   ```

2. ### Use in your application

   Choose one:

   **With Sidebar**

   ```
   import { Thread } from "@/components/assistant-ui/thread";
   import { ThreadListSidebar } from "@/components/assistant-ui/threadlist-sidebar";
   import {
     SidebarProvider,
     SidebarInset,
     SidebarTrigger
   } from "@/components/ui/sidebar";

   export default function Assistant() {
     return (
       <SidebarProvider>
         <div className="flex h-dvh w-full">
           <ThreadListSidebar />
           <SidebarInset>
             {/* Add sidebar trigger, location can be customized */}
             <SidebarTrigger className="absolute top-4 left-4" />
             <Thread />
           </SidebarInset>
         </div>
       </SidebarProvider>
     );
   }
   ```

   **Without Sidebar**

   ```
   import { Thread } from "@/components/assistant-ui/thread";
   import { ThreadList } from "@/components/assistant-ui/thread-list";

   export default function Assistant() {
     return (
       <div className="grid h-full grid-cols-[200px_1fr]">
         <ThreadList />
         <Thread />
       </div>
     );
   }
   ```

## Anatomy

The `ThreadList` component is built with the following primitives:

```
import { ThreadListPrimitive, ThreadListItemPrimitive } from "@assistant-ui/react";

<ThreadListPrimitive.Root>
  <ThreadListPrimitive.New />
  <ThreadListPrimitive.Items>
    {() => (
      <ThreadListItemPrimitive.Root>
        <ThreadListItemPrimitive.Trigger>
          <ThreadListItemPrimitive.Title />
        </ThreadListItemPrimitive.Trigger>
        <ThreadListItemPrimitive.Archive />
        <ThreadListItemPrimitive.Delete />
      </ThreadListItemPrimitive.Root>
    )}
  </ThreadListPrimitive.Items>
</ThreadListPrimitive.Root>
```

## API Reference

### ThreadListPrimitive.Root

Container for the thread list.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

### ThreadListPrimitive.Items

Renders all threads in the list.

- `archived?`: `boolean` — When true, renders archived threads instead of active threads.
- `components`: `object` — Component configuration.
  - `ThreadListItem`: `ComponentType` — Component to render for each thread item.

### ThreadListPrimitive.New

A button to create a new thread.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### ThreadListPrimitive.LoadMore

A button that appends the next page of threads. See the [LoadMore primitive reference](/docs/primitives/thread-list#loadmore) for usage and [Threads concepts](/docs/runtimes/concepts/threads#paginating-the-thread-list) for the adapter contract.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### ThreadListItemPrimitive.Root

Container for a single thread item. Automatically sets `data-active` and `aria-current` when this is the current thread.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

### ThreadListItemPrimitive.Trigger

A button that switches to this thread when clicked.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### ThreadListItemPrimitive.Title

Renders the thread's title.

- `fallback?`: `ReactNode` — Content to display when the thread has no title.

### ThreadListItemPrimitive.Archive

A button to archive the thread.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### ThreadListItemPrimitive.Unarchive

A button to restore an archived thread.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### ThreadListItemPrimitive.Delete

A button to permanently delete the thread.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### ThreadListItemMorePrimitive

A dropdown menu for additional thread actions, built on Radix UI DropdownMenu.

#### ThreadListItemMorePrimitive.Root

Menu container that manages dropdown state.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

#### ThreadListItemMorePrimitive.Trigger

Button to open the menu.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

#### ThreadListItemMorePrimitive.Content

Menu content container.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

#### ThreadListItemMorePrimitive.Item

Individual menu item.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

#### ThreadListItemMorePrimitive.Separator

Visual separator between items.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

## Related Components

- [Thread](/docs/ui/thread) - The main chat interface displayed alongside the list