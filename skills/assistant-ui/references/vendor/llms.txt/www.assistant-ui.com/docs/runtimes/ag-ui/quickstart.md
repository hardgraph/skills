# Quickstart
URL: /docs/runtimes/ag-ui/quickstart

Minimal HttpAgent + useAgUiRuntime setup against an AG-UI server.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Three steps to a working chat against an AG-UI agent. Assumes you have already installed the package and have an AG-UI server reachable; if not, start at [overview](/docs/runtimes/ag-ui/overview).

1. ### Wire up the runtime provider

   Create an `HttpAgent` pointing at your AG-UI endpoint and pass it to `useAgUiRuntime`:

   **React**

   ```
   "use client";

   import { useMemo } from "react";
   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useAgUiRuntime } from "@assistant-ui/react-ag-ui";
   import { HttpAgent } from "@ag-ui/client";

   export function MyRuntimeProvider({
     children,
   }: {
     children: React.ReactNode;
   }) {
     const agent = useMemo(
       () =>
         new HttpAgent({
           url: "http://localhost:8000/agent",
         }),
       [],
     );

     const runtime = useAgUiRuntime({ agent });

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

Once your AG-UI server is reachable, the runtime parses incoming events (`TEXT_MESSAGE_*`, `TOOL_CALL_*`, `STATE_SNAPSHOT`, etc.) into assistant-ui messages.

## Showing thinking and reasoning

`showThinking` (default `true`) controls whether `THINKING_*` and `REASONING_*` events render as visible reasoning in the UI:

```
const runtime = useAgUiRuntime({
  agent,
  showThinking: false, // hide thinking blocks
});
```

## Adding adapters

Attachments, speech, dictation, feedback, history, and a custom thread list are supported via the standard adapter slots. See [adapters](/docs/runtimes/concepts/adapters):

```
const runtime = useAgUiRuntime({
  agent,
  adapters: { attachments, history, speech, feedback },
});
```

For multi-thread, pass `adapters.threadList` (experimental); see [runtime options](/docs/runtimes/ag-ui/runtime-options#thread-list-experimental).

## Next

- [Runtime options](/docs/runtimes/ag-ui/runtime-options) — useAgUiRuntime options, adapters, events, thread list.
- [Pick a runtime](/docs/runtimes/pick-a-runtime) — Compare AG-UI to other runtime options.