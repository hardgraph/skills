# MessagePartPrimitive
URL: /docs/api-reference/primitives/message-part

Message part primitives for rendering text, tool calls, data parts, reasoning, source content, and custom assistant output.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Anatomy

```
import { MessagePartPrimitive } from "@assistant-ui/react";

const TextMessagePart = () => {
  return <MessagePartPrimitive.Text />;
};
```

## API Reference

### Text

Renders the text content of a message part with optional smooth streaming. This component displays text content from the current message part context, with support for smooth streaming animation that shows text appearing character by character as it's generated.

This primitive renders a `<span>` element unless `asChild` is set.

```
<MessagePartPrimitive.Text
  smooth={true}
  component="p"
  className="message-text"
/>
```

- `render?`: `ReactElement`
- `smooth`: `boolean | SmoothOptions` (default `true`) — Whether to enable smooth text streaming animation. When enabled, text appears with a typing effect as it streams in. Pass a \`SmoothOptions\` object to tune the reveal rate. Auto-disables under \`prefers-reduced-motion: reduce\`.
- `component`: `ElementType` (default `"span"`) — The HTML element or React component to render as.

| Data attribute  | Values                                       |
| --------------- | -------------------------------------------- |
| `[data-status]` | `"running"`, `"complete"`, or `"incomplete"` |

### Image

Renders an image from the current message part context. This component displays image content from the current message part, automatically setting the src attribute from the message part's image data.

```
<MessagePartPrimitive.Image
  alt="Generated image"
  className="message-image"
  style={{ maxWidth: '100%' }}
/>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### InProgress



### Messages

- `components?`: `NonNullable<ThreadPrimitiveMessages.Props["components"]>`

  - `Message?`: `ComponentType` — Component used to render all message types
  - `EditComposer?`: `ComponentType` — Component used when editing any message type
  - `UserEditComposer?`: `ComponentType` — Component used when editing user messages specifically
  - `AssistantEditComposer?`: `ComponentType` — Component used when editing assistant messages specifically
  - `SystemEditComposer?`: `ComponentType` — Component used when editing system messages specifically
  - `UserMessage?`: `ComponentType` — Component used to render user messages specifically
  - `AssistantMessage?`: `ComponentType` — Component used to render assistant messages specifically
  - `SystemMessage?`: `ComponentType` — Component used to render system messages specifically

- `children?`: `(value: { message: ThreadMessage }) => ReactNode` — Render function called for each message. Receives the message.