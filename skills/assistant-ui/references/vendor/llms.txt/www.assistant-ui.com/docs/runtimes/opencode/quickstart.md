# Quickstart
URL: /docs/runtimes/opencode/quickstart

Minimal useOpenCodeRuntime setup against a local OpenCode server.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Three steps to a working chat against a local OpenCode server. Assumes you have OpenCode running; if not, follow [opencode.ai](https://opencode.ai/) to install and start the server first.

1. ### Wire up the runtime provider

   ```
   "use client";

   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useOpenCodeRuntime } from "@assistant-ui/react-opencode";

   export function MyRuntimeProvider({
     children,
   }: {
     children: React.ReactNode;
   }) {
     const runtime = useOpenCodeRuntime({
       baseUrl: "http://localhost:4096",
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         {children}
       </AssistantRuntimeProvider>
     );
   }
   ```

   `baseUrl` defaults to `http://localhost:4096` if omitted.

2. ### Render the Thread

   ```
   import { Thread } from "@assistant-ui/react";
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

   Follow the [UI Components guide](/docs/ui/thread) to wire up the Thread, composer, and supporting primitives.

The runtime opens an SSE event stream against the OpenCode server, listens for session and message events, and projects them into the thread. Each OpenCode session corresponds to one thread; the thread list reflects sessions on the server.

## Default model and agent

Pin a default model or agent when starting new sessions:

```
const runtime = useOpenCodeRuntime({
  baseUrl: "http://localhost:4096",
  defaultModel: { providerID: "anthropic", modelID: "claude-sonnet-4-6" },
  defaultAgent: "coder",
});
```

## Bring your own client

If you need a pre-configured `OpencodeClient` (e.g. for auth headers), pass it directly:

```
import { createOpencodeClient } from "@assistant-ui/react-opencode";

const client = createOpencodeClient({
  baseUrl: "http://localhost:4096",
  // additional client config...
});

const runtime = useOpenCodeRuntime({ client });
```

## Resuming a session

Pass `initialSessionId` to open the runtime against an existing session on first load:

```
const runtime = useOpenCodeRuntime({
  baseUrl: "http://localhost:4096",
  initialSessionId: "ses_abc123",
});
```

## Next

- [Hooks](/docs/runtimes/opencode/hooks) — Permissions, questions, session state, runtime extras.
- [Pick a runtime](/docs/runtimes/pick-a-runtime) — Compare OpenCode to other runtime options.