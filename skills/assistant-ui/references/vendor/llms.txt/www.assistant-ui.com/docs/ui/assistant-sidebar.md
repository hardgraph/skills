# AssistantSidebar
URL: /docs/ui/assistant-sidebar

Side panel chat for co-pilot experiences and inline assistance.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A resizable side panel layout with your main content on the left and a Thread chat interface on the right. Ideal for co-pilot experiences and inline assistance.

\[interactive preview omitted]

## Getting Started

1. ### Add `assistant-sidebar`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/assistant-sidebar
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/assistant-sidebar.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-markdown class-variance-authority remark-gfm tw-shimmer zustand
   ```

   ```bash
   npx shadcn@latest add resizable button dialog tooltip avatar collapsible
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/assistant-sidebar.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/assistant-sidebar.tsx)
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
     -o components/assistant-ui/assistant-sidebar.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/assistant-sidebar.tsx \
     -o components/assistant-ui/thread.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread.tsx \
     -o components/assistant-ui/attachment.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/attachment.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx \
     -o components/assistant-ui/follow-up-suggestions.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx \
     -o components/assistant-ui/markdown-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/markdown-text.tsx \
     -o components/assistant-ui/reasoning.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/reasoning.tsx \
     -o components/assistant-ui/tool-fallback.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx \
     -o components/assistant-ui/tool-group.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-group.tsx
   ```

   This adds `/components/assistant-ui/assistant-sidebar.tsx` to your project, which you can adjust as needed.

2. ### Use in your application

   ```
   import { AssistantSidebar } from "@/components/assistant-ui/assistant-sidebar";

   export default function Home() {
     return (
       <div className="h-full">
         <AssistantSidebar>{/* your app */}</AssistantSidebar>
       </div>
     );
   }
   ```

## API Reference

### AssistantSidebar

A layout component that creates a resizable two-panel interface.

- `children?`: `ReactNode` — Content to display in the left panel (your main application).

The component uses `ResizablePanelGroup` from shadcn/ui internally, creating:

- **Left panel**: Your application content (passed as `children`)
- **Right panel**: The Thread chat interface (rendered automatically)
- **Resize handle**: Draggable divider between panels

## Customization

Since this component is copied to your project at `/components/assistant-ui/assistant-sidebar.tsx`, you can customize:

- Panel default sizes and min/max constraints
- Resize handle styling
- Thread component configuration

```
<ResizablePanelGroup direction="horizontal">
  <ResizablePanel defaultSize={60} minSize={30}>
    {children}
  </ResizablePanel>
  <ResizableHandle withHandle />
  <ResizablePanel defaultSize={40} minSize={20}>
    <Thread />
  </ResizablePanel>
</ResizablePanelGroup>
```

## Related Components

- [Thread](/docs/ui/thread) - The chat interface displayed in the sidebar
- [AssistantModal](/docs/ui/assistant-modal) - Alternative floating modal layout