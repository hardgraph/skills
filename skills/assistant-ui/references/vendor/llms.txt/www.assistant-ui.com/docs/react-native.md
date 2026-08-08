# React Native AI Chat
URL: /docs/react-native

Build AI chat for iOS and Android with @assistant-ui/react-native — streaming, tools, attachments, and platform-native components from the same primitives as the web SDK.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## Quick Start

The fastest way to get started with assistant-ui for React Native.

1. ### Create a new project

   ```
   npx assistant-ui@latest create --example with-expo my-chat-app
   cd my-chat-app
   ```

2. ### Configure API endpoint

   Create a `.env` file pointing to your chat API:

   ```
   EXPO_PUBLIC_CHAT_ENDPOINT_URL="http://localhost:3000/api/chat"
   ```

3. ### Start the app

   ```
   npx expo start
   ```

## Manual Setup

If you prefer to add assistant-ui to an existing Expo project, follow these steps.

1. ### Install dependencies

   ```bash
   npx expo install @assistant-ui/react-native @assistant-ui/react-ai-sdk
   ```

2. ### Setup Backend Endpoint

   Create a backend API route using the Vercel AI SDK. This is the same endpoint you'd use with `@assistant-ui/react` on the web:

   Choose one:

   **OpenAI**

   ```
   import { openai } from "@ai-sdk/openai";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages } = await req.json();
     const result = streamText({
       model: openai("gpt-5.6-luna"),
       messages: await convertToModelMessages(messages),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Anthropic**

   ```
   import { anthropic } from "@ai-sdk/anthropic";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages } = await req.json();
     const result = streamText({
       model: anthropic("claude-sonnet-4-6"),
       messages: await convertToModelMessages(messages),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   **Google**

   ```
   import { google } from "@ai-sdk/google";
   import { convertToModelMessages, streamText } from "ai";

   export const maxDuration = 30;

   export async function POST(req: Request) {
     const { messages } = await req.json();
     const result = streamText({
       model: google("gemini-2.0-flash"),
       messages: await convertToModelMessages(messages),
     });
     return result.toUIMessageStreamResponse();
   }
   ```

   > [!info]
   >
   > This is the same backend you'd use with `@assistant-ui/react` on the web. If you already have an API route, you can reuse it as-is.

3. ### Set up the runtime

   ```
   import { useChatRuntime, AssistantChatTransport } from "@assistant-ui/react-ai-sdk";

   const API_URL = process.env.EXPO_PUBLIC_API_URL ?? "http://localhost:3000";

   export function useAppRuntime() {
     return useChatRuntime({
       transport: new AssistantChatTransport({
         api: `${API_URL}/api/chat`,
       }),
     });
   }
   ```

4. ### Use it in your app

   Wrap your app with `AssistantRuntimeProvider` and build your chat UI:

   ```
   import {
     AssistantRuntimeProvider,
     useAuiState,
     useAui,
   } from "@assistant-ui/react-native";
   import type { ThreadMessage } from "@assistant-ui/react-native";
   import {
     View,
     Text,
     TextInput,
     FlatList,
     Pressable,
     KeyboardAvoidingView,
     Platform,
   } from "react-native";
   import { useAppRuntime } from "@/hooks/use-app-runtime";

   function MessageBubble({ message }: { message: ThreadMessage }) {
     const isUser = message.role === "user";
     const text = message.content
       .filter((p) => p.type === "text")
       .map((p) => ("text" in p ? p.text : ""))
       .join("\n");

     return (
       <View
         style={{
           alignSelf: isUser ? "flex-end" : "flex-start",
           backgroundColor: isUser ? "#007aff" : "#f0f0f0",
           borderRadius: 16,
           padding: 12,
           marginVertical: 4,
           marginHorizontal: 16,
           maxWidth: "80%",
         }}
       >
         <Text style={{ color: isUser ? "#fff" : "#000" }}>{text}</Text>
       </View>
     );
   }

   function Composer() {
     const aui = useAui();
     const text = useAuiState((s) => s.composer.text);
     const isEmpty = useAuiState((s) => s.composer.isEmpty);

     return (
       <View
         style={{
           flexDirection: "row",
           padding: 12,
           alignItems: "flex-end",
         }}
       >
         <TextInput
           value={text}
           onChangeText={(t) => aui.composer.setText(t)}
           placeholder="Message..."
           multiline
           style={{
             flex: 1,
             borderWidth: 1,
             borderColor: "#ddd",
             borderRadius: 20,
             paddingHorizontal: 16,
             paddingVertical: 10,
             maxHeight: 120,
           }}
         />
         <Pressable
           onPress={() => aui.composer.send()}
           disabled={isEmpty}
           style={{
             marginLeft: 8,
             backgroundColor: !isEmpty ? "#007aff" : "#ccc",
             borderRadius: 20,
             width: 36,
             height: 36,
             justifyContent: "center",
             alignItems: "center",
           }}
         >
           <Text style={{ color: "#fff", fontWeight: "bold" }}>↑</Text>
         </Pressable>
       </View>
     );
   }

   function ChatScreen() {
     const messages = useAuiState(
       (s) => s.thread.messages,
     ) as ThreadMessage[];

     return (
       <KeyboardAvoidingView
         style={{ flex: 1 }}
         behavior={Platform.OS === "ios" ? "padding" : "height"}
       >
         <FlatList
           data={messages}
           keyExtractor={(m) => m.id}
           renderItem={({ item }) => <MessageBubble message={item} />}
         />
         <Composer />
       </KeyboardAvoidingView>
     );
   }

   export default function App() {
     const runtime = useAppRuntime();

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <ChatScreen />
       </AssistantRuntimeProvider>
     );
   }
   ```

## What's Next?

- [Migration from Web](/docs/react-native/migration) — Already using assistant-ui? Migrate your web app to React Native.
- [Custom Backend](/docs/react-native/custom-backend) — Connect to your own backend API or manage threads server-side.
- [Primitives](/docs/react-native/primitives) — Composable native UI components for building chat interfaces.
- [Example App](https://github.com/assistant-ui/assistant-ui/tree/main/examples/with-expo) — Full Expo example with drawer navigation, thread list, and styled UI.