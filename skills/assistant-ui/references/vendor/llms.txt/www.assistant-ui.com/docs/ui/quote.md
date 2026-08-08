# Quote
URL: /docs/ui/quote

Let users select and quote text from messages with a floating toolbar, composer preview, and inline quote display.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

## Getting Started

1. ### Add `quote`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/quote
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/quote.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/quote.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/quote.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/quote.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/quote.tsx
   ```

   This adds a `/components/assistant-ui/quote.tsx` file with three components: `QuoteBlock`, `SelectionToolbar`, and `ComposerQuotePreview`.

2. ### Display Quotes in User Messages

   Add `MessagePrimitive.Quote` above `MessagePrimitive.Parts` in your user message:

   ```
   import { QuoteBlock } from "@/components/assistant-ui/quote";

   const UserMessage = () => {
     return (
       <MessagePrimitive.Root>
         <MessagePrimitive.Quote>
           {(quote) => <QuoteBlock {...quote} />}
         </MessagePrimitive.Quote>
         <MessagePrimitive.Parts />
       </MessagePrimitive.Root>
     );
   };
   ```

3. ### Add the Floating Selection Toolbar

   Render `SelectionToolbar` inside `ThreadPrimitive.Root`. It portals to the document body and appears near the user's text selection.

   ```
   import { SelectionToolbar } from "@/components/assistant-ui/quote";

   const Thread = () => {
     return (
       <ThreadPrimitive.Root>
         <ThreadPrimitive.Viewport>
           <ThreadPrimitive.Messages>
             {({ message }) => { ... }}
           </ThreadPrimitive.Messages>
           ...
         </ThreadPrimitive.Viewport>

         <SelectionToolbar />
       </ThreadPrimitive.Root>
     );
   };
   ```

4. ### Show Quote Preview in Composer

   Add `ComposerQuotePreview` inside your composer. It only renders when a quote is set.

   ```
   import { ComposerQuotePreview } from "@/components/assistant-ui/quote";

   const Composer = () => {
     return (
       <ComposerPrimitive.Root>
         <ComposerQuotePreview />
         <ComposerPrimitive.Input placeholder="Send a message..." />
         <ComposerPrimitive.Send />
       </ComposerPrimitive.Root>
     );
   };
   ```

5. ### Forward Quote Context to the LLM

   Quote data is stored in message metadata, **not** in message content. Use `injectQuoteContext` in your route handler so the LLM sees the quoted text:

   ```
   import { convertToModelMessages, streamText } from "ai";
   import { injectQuoteContext } from "@assistant-ui/react-ai-sdk";

   export async function POST(req: Request) {
     const { messages } = await req.json();

     const result = streamText({
       model: myModel,
       messages: await convertToModelMessages(injectQuoteContext(messages)),
     });

     return result.toUIMessageStreamResponse();
   }
   ```

   `injectQuoteContext` prepends the quoted text as a markdown `>` blockquote to the message parts so the LLM receives the context the user is referring to.

   > [!info]
   >
   > For alternative backend approaches (Claude SDK citation source, OpenAI message context), see the [Quoting guide](/docs/guides/quoting#backend-handling).

## Customization

All three components expose sub-components for full control over styling:

```
// Custom QuoteBlock
<QuoteBlock.Root className="my-custom-class">
  <QuoteBlock.Icon />
  <QuoteBlock.Text>{text}</QuoteBlock.Text>
</QuoteBlock.Root>

// Custom SelectionToolbar
<SelectionToolbar.Root>
  <SelectionToolbar.Quote>Reply with quote</SelectionToolbar.Quote>
</SelectionToolbar.Root>

// Custom ComposerQuotePreview
<ComposerQuotePreview.Root>
  <ComposerQuotePreview.Icon />
  <ComposerQuotePreview.Text />
  <ComposerQuotePreview.Dismiss />
</ComposerQuotePreview.Root>
```

## API Reference

### `QuoteBlock`

Renders quoted text in user messages. Pass to `MessagePrimitive.Parts` as `components.Quote`.

| Prop        | Type     | Description              |
| ----------- | -------- | ------------------------ |
| `text`      | `string` | The quoted text          |
| `messageId` | `string` | ID of the source message |

**Sub-components:** `QuoteBlock.Root`, `QuoteBlock.Icon`, `QuoteBlock.Text`

### `SelectionToolbar`

Floating toolbar that appears when text is selected within a message. Renders as a portal positioned above the selection.

| Prop        | Type     | Description            |
| ----------- | -------- | ---------------------- |
| `className` | `string` | Additional class names |

**Sub-components:** `SelectionToolbar.Root`, `SelectionToolbar.Quote`

### `ComposerQuotePreview`

Quote preview inside the composer. Only renders when a quote is set.

| Prop        | Type     | Description            |
| ----------- | -------- | ---------------------- |
| `className` | `string` | Additional class names |

**Sub-components:** `ComposerQuotePreview.Root`, `ComposerQuotePreview.Icon`, `ComposerQuotePreview.Text`, `ComposerQuotePreview.Dismiss`

### `injectQuoteContext`

```
import { injectQuoteContext } from "@assistant-ui/react-ai-sdk";

injectQuoteContext(messages: UIMessage[]): UIMessage[]
```

Extracts `metadata.custom.quote` from each message and prepends the quoted text as a `> blockquote` text part. Use before `convertToModelMessages` in your route handler. For alternative backend approaches, see the [Quoting guide](/docs/guides/quoting#backend-handling).

### `useMessageQuote`

```
import { useMessageQuote } from "@assistant-ui/react";

const quote: QuoteInfo | undefined = useMessageQuote();
```

Returns the quote attached to the current message, or `undefined`. Useful for building custom quote displays without `QuoteBlock`. For a usage example, see the [Quoting guide](/docs/guides/quoting#reading-quote-data).

### `ComposerRuntime.setQuote`

```
setQuote(quote: QuoteInfo | undefined): void
```

Set or clear the quote on the composer programmatically. The quote is automatically cleared when the message is sent. For a usage example, see the [Quoting guide](/docs/guides/quoting#programmatic-api).

## Related

- [Quoting guide](/docs/guides/quoting): Backend handling, programmatic API, data shape, and design notes
- [Thread](/docs/ui/thread): Main chat container