# Quickstart
URL: /docs/runtimes/google-adk/quickstart

Minimal API route and client setup with createAdkApiRoute.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Four steps to a working ADK chat. Assumes you have already installed the package and have ADK ready on the server side; if not, start at [overview](/docs/runtimes/google-adk/overview).

1. ### Create a backend API endpoint

   Use `createAdkApiRoute` to create an API route in one line: For React Native and Ink, host this route in a separate backend project and point the client runtime at its absolute URL.

   ```
   import { createAdkApiRoute } from "@assistant-ui/react-google-adk/server";
   import { InMemoryRunner, LlmAgent } from "@google/adk";

   const agent = new LlmAgent({
     name: "my_agent",
     model: "gemini-2.5-flash",
     instruction: "You are a helpful assistant.",
   });

   const runner = new InMemoryRunner({ agent, appName: "my-app" });

   export const POST = createAdkApiRoute({
     runner,
     userId: "user_1",
     sessionId: (req) =>
       new URL(req.url).searchParams.get("sessionId") ?? "default",
   });
   ```

   Both `userId` and `sessionId` accept either a static string or `(req: Request) => string` for dynamic resolution from cookies, headers, or query params.

2. ### Set up the client runtime

   Use `createAdkStream` to connect to your API route. No manual SSE parsing needed:

   **React**

   ```
   "use client";

   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import {
     useAdkRuntime,
     createAdkStream,
   } from "@assistant-ui/react-google-adk";
   import { Thread } from "@/components/assistant-ui/thread";

   export function MyAssistant() {
     const runtime = useAdkRuntime({
       stream: createAdkStream({ api: "/api/chat" }),
     });
     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <Thread />
       </AssistantRuntimeProvider>
     );
   }
   ```

3. ### Use the component

   **React**

   ```
   import { MyAssistant } from "@/components/MyAssistant";

   export default function Home() {
     return (
       <main className="h-dvh">
         <MyAssistant />
       </main>
     );
   }
   ```

4. ### Set up UI components

   **React**

   Follow the [UI Components guide](/docs/ui/thread) to wire up the Thread, composer, and supporting primitives.

## Adding adapters

Attachments, speech, feedback, history, and a custom thread list are supported via the standard adapter slots. See [adapters](/docs/runtimes/concepts/adapters):

```
const runtime = useAdkRuntime({
  stream: createAdkStream({ api: "/api/chat" }),
  adapters: { attachments, history, speech, feedback },
});
```

For multi-thread, see [threads](/docs/runtimes/concepts/threads).

## Next

- [API reference](/docs/runtimes/google-adk/api) — createAdkStream, server helpers, session adapter, threads, message editing.
- [Hooks](/docs/runtimes/google-adk/hooks) — Tool confirmations, auth, input requests, artifacts, escalation, metadata.