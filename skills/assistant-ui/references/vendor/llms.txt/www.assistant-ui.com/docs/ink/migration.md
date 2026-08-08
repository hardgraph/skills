# Migration from Web
URL: /docs/ink/migration

Migrate an existing @assistant-ui/react app to the terminal with React Ink.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

If you already have an assistant-ui web app, most of your code transfers directly. The runtime core is shared between `@assistant-ui/react` and `@assistant-ui/react-ink` via `@assistant-ui/core` — only the UI layer changes.

## What stays the same

- **Runtime setup** — `useLocalRuntime`, `ChatModelAdapter`, and all runtime options work identically.
- **AI SDK integration** — `@assistant-ui/react-ai-sdk` works with React Ink. Your runtime setup transfers directly.
- **Tool definitions** — `Tools({ toolkit })` and toolkit renderers use the same API.
- **State hooks** — `useAuiState`, `useAui`, and selector patterns are the same.
- **Backend code** — Your API routes, streaming endpoints, and server-side logic need zero changes.

## What changes

| Web (`@assistant-ui/react`)               | Terminal (`@assistant-ui/react-ink`)                                   |
| ----------------------------------------- | ---------------------------------------------------------------------- |
| `AssistantRuntimeProvider`                | `AssistantRuntimeProvider` (same name, from `@assistant-ui/react-ink`) |
| DOM primitives (`div`, `button`, `input`) | Ink primitives (`Box`, `Text`, `TextInput`)                            |
| CSS / Tailwind styling                    | Ink's flexbox + ANSI colors                                            |
| `@assistant-ui/ui` (shadcn components)    | Build your own terminal UI with primitives                             |
| `@assistant-ui/react-markdown`            | `@assistant-ui/react-ink-markdown` (ANSI-rendered markdown)            |

## Step-by-step

1. ### Install the React Ink package

   ```bash
   npm install @assistant-ui/react-ink ink react
   ```

2. ### Keep your runtime hook

   Your `useLocalRuntime` setup works as-is — no changes needed:

   ```
   // This file is identical to your web version
   import { useLocalRuntime, type ChatModelAdapter } from "@assistant-ui/react-ink";

   export function useAppRuntime(adapter: ChatModelAdapter) {
     return useLocalRuntime(adapter);
   }
   ```

   > [!tip]
   >
   > If you use a monorepo, you can share the adapter between web and terminal projects directly.

3. ### Replace the provider

   ```
   // Before (web)
   // import { AssistantRuntimeProvider } from "@assistant-ui/react";

   // After (Terminal) — same name, different package
   import { AssistantRuntimeProvider } from "@assistant-ui/react-ink";

   export function App() {
     const runtime = useAppRuntime(adapter);
     return (
       <AssistantRuntimeProvider runtime={runtime}>
         {/* your terminal UI */}
       </AssistantRuntimeProvider>
     );
   }
   ```

4. ### Rebuild the UI layer

   This is the main migration effort. Web components don't render in the terminal. Replace them with Ink primitives:

   ```
   // Web — DOM-based
   // import { Thread } from "@/components/assistant-ui/thread";

   // Terminal — use Ink primitives
   import {
     ThreadPrimitive,
     ComposerPrimitive,
     MessagePrimitive,
   } from "@assistant-ui/react-ink";
   import { Box } from "ink";

   function MyMessage() {
     return (
       <Box marginBottom={1}>
         <MessagePrimitive.Parts />
       </Box>
     );
   }

   function ChatScreen() {
     return (
       <ThreadPrimitive.Root>
         <ThreadPrimitive.Messages>
           {() => <MyMessage />}
         </ThreadPrimitive.Messages>
         <Box borderStyle="round" borderColor="gray" paddingX={1}>
           <ComposerPrimitive.Input submitOnEnter placeholder="Message..." autoFocus />
         </Box>
       </ThreadPrimitive.Root>
     );
   }
   ```

   See the [Primitives reference](/docs/ink/primitives) for the full list of available components.

## Monorepo code sharing

In a monorepo, you can share everything except the UI layer:

```
packages/
  shared/
    hooks/          ← useAppRuntime with ChatModelAdapter (shared)
    tools/          ← tool definitions (shared)
  web/
    components/     ← DOM-based UI
  terminal/
    components/     ← Ink-based terminal UI
```

The runtime hook, tool definitions, and backend code are platform-agnostic. Only the UI components need separate implementations for web and terminal.