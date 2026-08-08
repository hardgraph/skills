# Quickstart
URL: /docs/runtimes/a2a/quickstart

Minimal runtime and Thread setup against an A2A server.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Three steps to a working chat against an A2A server. Assumes you have already installed the package and have an A2A v1.0 server reachable; if not, start at [overview](/docs/runtimes/a2a/overview).

1. ### Wire up the runtime provider

   **React**

   ```
   "use client";

   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useA2ARuntime } from "@assistant-ui/react-a2a";

   export function MyRuntimeProvider({
     children,
   }: {
     children: React.ReactNode;
   }) {
     const runtime = useA2ARuntime({
       baseUrl: "http://localhost:9999",
     });
     return (
       <AssistantRuntimeProvider runtime={runtime}>
         {children}
       </AssistantRuntimeProvider>
     );
   }
   ```

2. ### Render the Thread

   **React**

   ```
   import { Thread } from "@/components/assistant-ui/thread";
   import { MyRuntimeProvider } from "./MyRuntimeProvider";

   export default function Page() {
     return (
       <MyRuntimeProvider>
         <Thread />
       </MyRuntimeProvider>
     );
   }
   ```

3. ### Set up UI components

   **React**

   Follow the [UI Components guide](/docs/ui/thread) to wire up the Thread, composer, and supporting primitives.

Once your A2A server is reachable, the runtime negotiates streaming vs non-streaming based on the agent card's `capabilities.streaming` flag and starts forwarding messages.

## Auth and headers

Pass static or dynamic headers when your server expects auth:

```
const runtime = useA2ARuntime({
  baseUrl: "http://localhost:9999",
  headers: async () => ({
    Authorization: `Bearer ${await getAccessToken()}`,
  }),
});
```

## Adding adapters

Attachments, speech, feedback, history, and a custom thread list are all supported via the standard adapter slots. See [adapters](/docs/runtimes/concepts/adapters) for the contracts; pass them on `useA2ARuntime`:

```
const runtime = useA2ARuntime({
  baseUrl: "http://localhost:9999",
  adapters: { attachments, history, speech, feedback },
});
```

For multi-thread, see [threads](/docs/runtimes/concepts/threads) and pass `adapters.threadList`.

## Next

- [Client and hooks](/docs/runtimes/a2a/client-and-hooks) — A2AClient, useA2ARuntime options, hooks reference, task states, artifacts.
- [Pick a runtime](/docs/runtimes/pick-a-runtime) — Compare A2A to other runtime options.