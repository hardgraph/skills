# Attachment
URL: /docs/ui/attachment

UI components for attaching and viewing files in messages.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

> [!info]
>
> **Note:** These components provide the UI for attachments, but you also need to configure attachment adapters in your runtime to handle file uploads and processing. See the [Attachments Guide](/docs/guides/attachments) for complete setup instructions.

## Getting Started

1. ### Add `attachment`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/attachment
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/attachment.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react zustand
   ```

   ```bash
   npx shadcn@latest add dialog tooltip avatar button
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/attachment.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/attachment.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/attachment.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/attachment.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx
   ```

   This adds a `/components/assistant-ui/attachment.tsx` file to your project, which you can adjust as needed.

2. ### Use in your application

   ```
   import {
     ComposerAttachments,
     ComposerAddAttachment,
   } from "@/components/assistant-ui/attachment";

   const Composer: FC = () => {
     return (
       <ComposerPrimitive.Root className="...">
         <ComposerAttachments />
         <ComposerAddAttachment />

         <ComposerPrimitive.Input
           autoFocus
           placeholder="Write a message..."
           rows={1}
           className="..."
         />
         <ComposerAction />
       </ComposerPrimitive.Root>
     );
   };
   ```

   ```
   import { UserMessageAttachments } from "@/components/assistant-ui/attachment";

   const UserMessage: FC = () => {
     return (
       <MessagePrimitive.Root className="...">
         <UserActionBar />

         <UserMessageAttachments />

         <div className="...">
           <MessagePrimitive.Parts />
         </div>

         <BranchPicker className="..." />
       </MessagePrimitive.Root>
     );
   };
   ```

## API Reference

### Composer Attachments

#### ComposerPrimitive.Attachments

Renders all attachments in the composer.

- `components?`: `AttachmentComponents` — Components to render for different attachment types.

  - `Image?`: `ComponentType` — Component for image attachments.
  - `Document?`: `ComponentType` — Component for document attachments (PDF, etc.).
  - `File?`: `ComponentType` — Component for generic file attachments.
  - `Attachment?`: `ComponentType` — Fallback component for all attachment types.

#### ComposerPrimitive.AddAttachment

A button that opens the file picker to add attachments.

- `multiple`: `boolean` (default `true`) — Allow selecting multiple files at once.
- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

This primitive renders a `<button>` element unless `asChild` is set.

### Message Attachments

#### MessagePrimitive.Attachments

Renders all attachments in a user message.

- `components?`: `AttachmentComponents` — Components to render for different attachment types (same as ComposerPrimitive.Attachments).

### Attachment Primitives

#### AttachmentPrimitive.Root

Container for a single attachment.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

#### AttachmentPrimitive.Name

Renders the attachment's file name.

#### AttachmentPrimitive.Remove

A button to remove the attachment from the composer.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.

### Attachment Types

Attachments have the following structure:

```
type Attachment = {
  id: string;
  type: "image" | "document" | "file" | (string & {});
  name: string;
  contentType?: string;
  file?: File;
  status:
    | { type: "running" | "requires-action" | "incomplete"; progress?: number }
    | { type: "complete" };
};
```

The `type` field accepts custom strings (e.g. `"data-workflow"`) beyond the built-in types. When an unknown type is encountered, the generic `Attachment` component is used as a fallback. The `contentType` field is optional — it can be omitted for non-file attachments where a MIME type is not meaningful.

## Related Components

- [Thread](/docs/ui/thread) - Main chat interface that displays attachments
- [Attachments Guide](/docs/guides/attachments) - Complete setup instructions for attachment adapters