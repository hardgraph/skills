# Mermaid Diagrams
URL: /docs/ui/mermaid

Render Mermaid diagrams in chat messages with streaming support.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Getting Started

1. ### Add `mermaid-diagram` component

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/mermaid-diagram
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/mermaid-diagram.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-markdown beautiful-mermaid
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/mermaid-diagram.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/mermaid-diagram.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/mermaid-diagram.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/mermaid-diagram.tsx
   ```

   This will install the required dependencies and add the component to your project.

2. ### Add it to `componentsByLanguage` in `markdown-text.tsx`

   ```
   import { MermaidDiagram } from "@/components/assistant-ui/mermaid-diagram";

   const MarkdownTextImpl = () => {
     return (
       <MarkdownTextPrimitive
         remarkPlugins={[remarkGfm]}
         className="aui-md"
         components={defaultComponents}
         componentsByLanguage={{
           mermaid: {
             SyntaxHighlighter: MermaidDiagram 
           },
         }}
       />
     );
   };

   export const MarkdownText = memo(MarkdownTextImpl);
   ```

## Configuration

Configure rendering options in `mermaid-diagram.tsx`:

```
renderMermaidSVG(code, {
  bg: "var(--background)",
  fg: "var(--foreground)",
  muted: "var(--muted-foreground)",
  border: "var(--border)",
  accent: "var(--foreground)",
  transparent: true,
});
```

The palette follows your theme's background, foreground, and shadcn color tokens, so diagrams match light and dark mode automatically.

## Streaming Performance

The `MermaidDiagram` component is optimized for streaming scenarios:

- **Skeleton while streaming**: Shows a placeholder skeleton until the response finishes streaming, then renders the diagram synchronously
- **Raw source fallback**: Invalid or unsupported diagrams fall back to displaying the raw source

## Supported Diagram Types

The component renders these diagram types:

- Flowcharts and decision trees
- Sequence diagrams
- Class diagrams
- State diagrams
- Entity relationship diagrams
- XY charts (bar, line, combined)

Other mermaid diagram types fall back to displaying the raw source. See the [Mermaid documentation](https://mermaid.js.org/) for syntax reference.

## Related Components

- [Markdown](/docs/ui/markdown) - Rich text rendering where mermaid is integrated
- [Syntax Highlighting](/docs/ui/syntax-highlighting) - Code highlighting for other languages