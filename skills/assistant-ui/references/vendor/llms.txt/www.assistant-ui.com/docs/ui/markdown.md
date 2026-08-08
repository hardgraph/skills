# Markdown
URL: /docs/ui/markdown

Display rich text with headings, lists, links, and code blocks.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

> [!info]
>
> Markdown support is already included by default in the `Thread` component.

## Enabling markdown support

1. ### Add `markdown-text`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/markdown-text
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/markdown-text.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react-markdown remark-gfm
   ```

   ```bash
   npx shadcn@latest add tooltip button
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/markdown-text.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/markdown-text.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/markdown-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/markdown-text.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx
   ```

   This adds a `/components/assistant-ui/markdown-text.tsx` file to your project, which you can adjust as needed.

2. ### Use it in your application

   Pass the `MarkdownText` component to the `MessagePrimitive.Parts` component

   ```
   import { MarkdownText } from "@/components/assistant-ui/markdown-text";

   const AssistantMessage: FC = () => {
     return (
       <MessagePrimitive.Root className="...">
         <div className="...">
           <MessagePrimitive.Parts>
             {({ part }) => part.type === "text" ? <MarkdownText /> : null}
           </MessagePrimitive.Parts>
         </div>
         <AssistantActionBar />

         <BranchPicker className="..." />
       </MessagePrimitive.Root>
     );
   };
   ```

## Syntax highlighting

Syntax Highlighting is not included by default, see [Syntax Highlighting](/docs/ui/syntax-highlighting) to learn how to add it.

## Related Components

- [Syntax Highlighting](/docs/ui/syntax-highlighting) - Add code highlighting to markdown
- [Mermaid](/docs/ui/mermaid) - Render diagrams in markdown code blocks