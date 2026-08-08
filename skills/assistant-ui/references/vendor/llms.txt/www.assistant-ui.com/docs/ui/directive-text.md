# Directive Text
URL: /docs/ui/directive-text

Render mention directives as inline chips in user messages.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

`DirectiveText` parses the directive syntax written by [`ComposerTriggerPopover`](/docs/ui/composer-trigger-popover) (default: `:type[label]{name=id}`) and renders each segment as an inline chip. Use it as the `Text` component in user messages so the raw directive syntax never shows up.

## Getting Started

1. ### Add `directive-text`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/directive-text
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/directive-text.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @base-ui/react class-variance-authority
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/directive-text.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/directive-text.tsx)
   - [components/assistant-ui/badge.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/badge.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/directive-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/directive-text.tsx \
     -o components/assistant-ui/badge.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/badge.tsx
   ```

   This adds `/components/assistant-ui/directive-text.tsx` with `DirectiveText` and the `createDirectiveText(formatter, options?)` factory.

2. ### Use in user messages

   Pass `DirectiveText` as the `Text` component in `MessagePrimitive.Parts`:

   ```
   import { DirectiveText } from "@/components/assistant-ui/directive-text";

   const UserMessage = () => (
     <MessagePrimitive.Root>
       <MessagePrimitive.Parts components={{ Text: DirectiveText }} />
     </MessagePrimitive.Root>
   );
   ```

   Keep your markdown renderer (e.g. `MarkdownText`) for assistant messages — assistants rarely emit directive syntax.

## Icons per Directive Type

`DirectiveText` ships icon-free by default — every parsed segment renders as a plain label chip regardless of its `type`. To add icons, use the `createDirectiveText` factory and pass an `iconMap` that routes each `type` string to an icon component, plus an optional `fallbackIcon` for unmapped types:

```
import { createDirectiveText } from "@/components/assistant-ui/directive-text";
import { unstable_defaultDirectiveFormatter } from "@assistant-ui/react";
import { FileTextIcon, SparklesIcon, UserIcon, WrenchIcon } from "lucide-react";

const DirectiveTextWithIcons = createDirectiveText(
  unstable_defaultDirectiveFormatter,
  {
    iconMap: {
      tool: WrenchIcon,
      user: UserIcon,
      file: FileTextIcon,
    },
    fallbackIcon: SparklesIcon,
  },
);

<MessagePrimitive.Parts components={{ Text: DirectiveTextWithIcons }} />;
```

A matching `iconMap` / `fallbackIcon` option is accepted by [`ComposerTriggerPopover`](/docs/ui/composer-trigger-popover), where it routes `item.metadata.icon` strings instead of `type` — keeping icon configuration consistent across the composer and the rendered message.

## Custom Formatter

The default format is `:type[label]{name=id}`. For a different format, build a custom `Unstable_DirectiveFormatter` and wrap it with `createDirectiveText`:

```
import type { Unstable_DirectiveFormatter } from "@assistant-ui/react";
import { createDirectiveText } from "@/components/assistant-ui/directive-text";

const slashFormatter: Unstable_DirectiveFormatter = {
  serialize: (item) => `/${item.id}`,
  parse: (text) => {
    /* return alternating text / mention segments */
  },
};

const SlashDirectiveText = createDirectiveText(slashFormatter);
```

Pass the **same formatter** to the composer trigger's `directive={{ formatter }}` (or `action={{ formatter }}`) so insertion and rendering stay consistent.

## Customizing the Chip

Because `directive-text.tsx` is copied into your project, you can also edit it directly — swap the chip styling, read `data-directive-id` to link to a detail page, or replace the chip wrapper entirely.

## API Reference

### `DirectiveText`

A `TextMessagePartComponent` that parses `:type[label]{name=id}` directives and renders them as inline chips. Uses the default formatter from `@assistant-ui/react` and renders chips without icons. For per-type icons, build a component with `createDirectiveText(formatter, { iconMap })`.

### `createDirectiveText(formatter, options?)`

Factory that returns a `TextMessagePartComponent` bound to a custom `Unstable_DirectiveFormatter`.

| Option         | Type                                                    | Description                                                                                                |
| -------------- | ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `iconMap`      | `Record<string, ComponentType<{ className?: string }>>` | Maps a directive segment's `type` to an icon rendered inside the chip                                      |
| `fallbackIcon` | `ComponentType<{ className?: string }>`                 | Icon used when no `iconMap` entry matches. When neither option resolves, the chip renders without an icon. |

## Related

- [Composer Trigger Popover](/docs/ui/composer-trigger-popover) — the picker that inserts directives
- [Mentions guide](/docs/guides/mentions) — directive format, backend parsing