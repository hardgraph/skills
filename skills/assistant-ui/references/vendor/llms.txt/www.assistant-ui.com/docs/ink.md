# Terminal AI Chat with Ink
URL: /docs/ink

Build AI chat interfaces for the terminal in TypeScript with @assistant-ui/react-ink — streaming, tool calls, and keyboard navigation in CLI apps.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Quick Start

The fastest way to get started with assistant-ui for the terminal.

1. ### Create a new project

   ```
   npx assistant-ui@latest create --ink my-chat-app
   cd my-chat-app
   ```

2. ### Install dependencies

   ```
   pnpm install
   ```

3. ### Start the app

   ```
   pnpm dev
   ```

## Manual Setup

If you prefer to add assistant-ui to an existing Node.js project, follow these steps.

1. ### Install dependencies

   ```bash
   npm install @assistant-ui/react-ink @assistant-ui/react-ink-markdown ink react
   ```

2. ### Create a chat model adapter

   Define how your app communicates with your AI backend. This example uses a simple streaming adapter:

   ```
   import type { ChatModelAdapter } from "@assistant-ui/react-ink";

   export const myChatAdapter: ChatModelAdapter = {
     async *run({ messages, abortSignal }) {
       const response = await fetch("http://localhost:3000/api/chat", {
         method: "POST",
         headers: { "Content-Type": "application/json" },
         body: JSON.stringify({ messages }),
         signal: abortSignal,
       });

       const reader = response.body?.getReader();
       if (!reader) throw new Error("No response body");

       const decoder = new TextDecoder();
       let fullText = "";

       while (true) {
         const { done, value } = await reader.read();
         if (done) break;

         const chunk = decoder.decode(value, { stream: true });
         fullText += chunk;
         yield { content: [{ type: "text", text: fullText }] };
       }
     },
   };
   ```

   > [!info]
   >
   > This is the same `ChatModelAdapter` interface used in `@assistant-ui/react` and `@assistant-ui/react-native`. If you already have an adapter, you can reuse it as-is.

3. ### Set up the runtime

   ```
   import { Box, Text } from "ink";
   import {
     AssistantRuntimeProvider,
     useLocalRuntime,
   } from "@assistant-ui/react-ink";
   import { myChatAdapter } from "./adapters/my-chat-adapter.js";

   export function App() {
     const runtime = useLocalRuntime(myChatAdapter);

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <Box flexDirection="column" padding={1}>
           <Text bold color="cyan">My Terminal Chat</Text>
           {/* your chat UI */}
         </Box>
       </AssistantRuntimeProvider>
     );
   }
   ```

4. ### Build your chat UI

   Use primitives to compose your terminal chat interface:

   ```
   import { Box, Text } from "ink";
   import {
     ThreadPrimitive,
     ComposerPrimitive,
     LoadingPrimitive,
     useAuiState,
   } from "@assistant-ui/react-ink";
   import { MarkdownText } from "@assistant-ui/react-ink-markdown";

   const Message = () => {
     const message = useAuiState((s) => s.message);
     const isUser = message.role === "user";
     const text = message.content
       .filter((p) => p.type === "text")
       .map((p) => ("text" in p ? p.text : ""))
       .join("");

     if (isUser) {
       return (
         <Box marginBottom={1}>
           <Text bold color="green">You: </Text>
           <Text wrap="wrap">{text}</Text>
         </Box>
       );
     }

     return (
       <Box flexDirection="column" marginBottom={1}>
         <Text bold color="blue">AI:</Text>
         <MarkdownText text={text} />
       </Box>
     );
   };

   const StatusIndicator = () => (
     <LoadingPrimitive.Root marginBottom={1} gap={1}>
       <LoadingPrimitive.Spinner variant="bar" />
       <LoadingPrimitive.Text />
       <LoadingPrimitive.ElapsedTime />
     </LoadingPrimitive.Root>
   );

   export const Thread = () => {
     return (
       <ThreadPrimitive.Root>
         <ThreadPrimitive.Empty>
           <Box marginBottom={1}>
             <Text dimColor>No messages yet. Start typing below!</Text>
           </Box>
         </ThreadPrimitive.Empty>

         <ThreadPrimitive.Messages>
           {() => <Message />}
         </ThreadPrimitive.Messages>

         <StatusIndicator />

         <Box borderStyle="round" borderColor="gray" paddingX={1}>
           <Text color="gray">{"> "}</Text>
           <ComposerPrimitive.Input
             submitOnEnter
             placeholder="Type a message..."
             autoFocus
           />
         </Box>
       </ThreadPrimitive.Root>
     );
   };
   ```

5. ### Render with Ink

   ```
   import { render } from "ink";
   import { App } from "./app.js";

   render(<App />);
   ```

## What's Next?

- [Migration from Web](/docs/ink/migration) — Already using assistant-ui? Migrate your web app to the terminal.
- [Custom Backend](/docs/ink/custom-backend) — Connect to your own backend API or manage threads server-side.
- [Primitives](/docs/ink/primitives) — Composable terminal UI components for building chat interfaces.
- [Example App](https://github.com/assistant-ui/assistant-ui/tree/main/examples/with-react-ink) — Full terminal chat example with markdown rendering and streaming.