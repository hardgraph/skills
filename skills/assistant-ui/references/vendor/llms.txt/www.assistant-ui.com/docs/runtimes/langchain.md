# LangChain React Runtime
URL: /docs/runtimes/langchain

Use LangChain's useStream hook with a React chat UI through assistant-ui — a lighter LangGraph adapter that delegates streaming to @langchain/react.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`@assistant-ui/react-langchain` wraps [`useStream`](https://reference.langchain.com/javascript/langchain-react/use-stream) from `@langchain/react` and exposes it as an assistant-ui runtime. It targets the same backend as [`@assistant-ui/react-langgraph`](/docs/runtimes/langgraph/overview) (LangGraph Cloud) but at a higher level, delegating stream plumbing to the upstream hook.

## When to use it

Pick `react-langchain` over `react-langgraph` when:

- You are scaffolding via `npx create-assistant-ui -t langchain` (the official template uses it).
- Your app already depends on `@langchain/react` and uses `useStream` elsewhere.
- You want to read custom state keys (`todos`, `files`, plans) reactively with `useLangChainState<T>(key)`.
- You prefer a thin wrapper that stays pinned to upstream behavior.

Pick `react-langgraph` instead when:

- You need a fully custom/bespoke backend stream (your own async generator) rather than `useStream`'s transport, or you're maintaining an existing `react-langgraph` app.

Both adapters are first-class and now at feature parity (the [comparison](#comparison-with-react-langgraph) below has the full table); `react-langchain` is the newer, thinner wrapper pinned to upstream `useStream`.

## Architecture

`@assistant-ui/react-langchain` is layered on `ExternalStoreRuntime` (see [architecture](/docs/runtimes/concepts/architecture)). Graph state is the source of truth; the runtime renders messages from `state.values.messages` and submits user input back to the graph.

Shared adapters (attachments, speech, feedback) work the same way described in [adapters](/docs/runtimes/concepts/adapters). Cloud thread persistence is built in.

`thread.isLoading` reflects `useStream`'s `isThreadLoading` (initial history hydration), separate from `isRunning` (a run being in-flight).

## Requirements

- A LangGraph Cloud API server (locally via [LangGraph Studio](https://docs.langchain.com/langsmith/quick-start-studio) or hosted via [LangSmith](https://www.langchain.com/langsmith)).
- The graph state must include a `messages` key with LangChain-alike messages, or pass a custom `messagesKey`.

## Quickstart

1. ### Install dependencies

   **React**

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-langchain @langchain/react @langchain/langgraph-sdk
   ```

2. ### Define the assistant component

   **React**

   ```
   "use client";

   import { Thread } from "@/components/assistant-ui/thread";
   import { AssistantRuntimeProvider } from "@assistant-ui/react";
   import { useStreamRuntime } from "@assistant-ui/react-langchain";

   export function MyAssistant() {
     const runtime = useStreamRuntime({
       assistantId: process.env["NEXT_PUBLIC_LANGGRAPH_ASSISTANT_ID"]!,
       apiUrl: process.env["NEXT_PUBLIC_LANGGRAPH_API_URL"],
     });

     return (
       <AssistantRuntimeProvider runtime={runtime}>
         <Thread />
       </AssistantRuntimeProvider>
     );
   }
   ```

3. ### Mount the component

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

4. ### Set environment variables

   **React**

   ```
   NEXT_PUBLIC_LANGGRAPH_API_URL=http://localhost:2024
   NEXT_PUBLIC_LANGGRAPH_ASSISTANT_ID=your_graph_id
   ```

5. ### Set up UI components

   **React**

   Follow the [UI Components guide](/docs/ui/thread).

## `useStreamRuntime` options

`useStreamRuntime` accepts every option upstream `useStream` does, plus these assistant-ui-specific fields:

| Option        | Type                                   | Description                                                           |
| ------------- | -------------------------------------- | --------------------------------------------------------------------- |
| `cloud`       | `AssistantCloud`                       | Optional. Persists threads via assistant-cloud.                       |
| `adapters`    | `{ attachments?, speech?, feedback? }` | Optional. Attachment, speech, and feedback adapters.                  |
| `messagesKey` | `string`                               | The state key that holds messages. Defaults to `"messages"`.          |
| `uiStateKey`  | `string`                               | The state key that holds generative `UIMessage`s. Defaults to `"ui"`. |

### Forwarding per-run config

When a message is sent, its `runConfig.custom` (for example a selected mode or model) is forwarded on the underlying `useStream().submit` call as `config.configurable`. Read it in the graph from `config["configurable"]`; on LangGraph v1 the same values are also reachable through the Runtime `context`, which `config.configurable` is aliased to. This lets per-run app config reach the graph without extra wiring.

## Reading custom state keys

LangGraph agents often expose structured state beyond messages (plans, todos, scratch files, generative-UI artifacts). Read them directly with `useLangChainState`. It mirrors `useStream().values[key]` upstream and updates when the stream emits new state.

**React**

```
import { useLangChainState } from "@assistant-ui/react-langchain";

type Todo = { id: string; title: string; done: boolean };

function TodoList() {
  const todos = useLangChainState<Todo[]>("todos", []);
  return (
    <ul>
      {todos.map((t) => (
        <li key={t.id}>
          {t.done ? "✓" : "○"} {t.title}
        </li>
      ))}
    </ul>
  );
}
```

Signatures:

```
useLangChainState<T>(key: string): T | undefined;
useLangChainState<T>(key: string, defaultValue: T): T;
```

Useful with the [`deepagents`](https://docs.langchain.com/oss/python/deepagents) middleware, whose `write_todos` step updates `state.todos` alongside the tool-call stream. Reading the state key directly avoids reconstructing the list from partial tool-call args.

> [!info]
>
> Added in v0.0.2 — see issue [#3862](https://github.com/assistant-ui/assistant-ui/issues/3862) for motivation.

## Interrupts

LangGraph interrupts pause the graph and wait for client input. `useLangChainInterruptState` exposes the current interrupt; `useLangChainRespond` resumes it with a response payload, while `useLangChainSubmit` resumes the graph with a raw state update.

`useLangChainRespond` is the cleaner resume path; it carries the response payload via `useStream().respond` and handles interrupt namespaces, so you don't have to construct a `Command`.

**React**

```
import {
  useLangChainInterruptState,
  useLangChainRespond,
} from "@assistant-ui/react-langchain";

function InterruptPrompt() {
  const interrupt = useLangChainInterruptState();
  const respond = useLangChainRespond();
  if (!interrupt) return null;
  return (
    <div>
      <pre>{JSON.stringify(interrupt.value, null, 2)}</pre>
      <button onClick={() => respond({ approved: true })}>Approve</button>
    </div>
  );
}
```

When several interrupts are pending at once (e.g. parallel approvals), use `useLangChainRespondAll()` to resume them all in one run. Sequential `useLangChainRespond` calls can't, since the first resume starts a run and strands the rest.

```
import {
  useLangChainInterrupts,
  useLangChainRespondAll,
} from "@assistant-ui/react-langchain";

const interrupts = useLangChainInterrupts();
const respondAll = useLangChainRespondAll();
// resume every pending interrupt with the same payload:
await respondAll(
  Object.fromEntries(
    interrupts.flatMap((i) => (i.id ? [[i.id, { approved: true }]] : [])),
  ),
);
```

You can also resume with a raw `Command` via `useLangChainSubmit`:

**React**

```
import {
  useLangChainInterruptState,
  useLangChainSubmit,
} from "@assistant-ui/react-langchain";
import { Command } from "@langchain/langgraph-sdk";

function InterruptPrompt() {
  const interrupt = useLangChainInterruptState();
  const submit = useLangChainSubmit();
  if (!interrupt) return null;
  return (
    <div>
      <pre>{JSON.stringify(interrupt.value, null, 2)}</pre>
      <button
        onClick={() =>
          submit(null, { command: new Command({ resume: "approved" }) })
        }
      >
        Approve
      </button>
    </div>
  );
}
```

## Tool calls

`useLangChainToolCalls` exposes the root tool calls `useStream` assembles from the `tools` channel — each entry carries a `name`, `args`, and `id`. Use it to render pending or streamed tool calls and approval UIs. It defaults to an empty array, so you can `.map` without a guard.

**React**

```
import { useLangChainToolCalls } from "@assistant-ui/react-langchain";

function PendingToolCalls() {
  const toolCalls = useLangChainToolCalls();
  return (
    <ul>
      {toolCalls.map((tc) => (
        <li key={tc.id}>
          {tc.name}: <code>{JSON.stringify(tc.args)}</code>
        </li>
      ))}
    </ul>
  );
}
```

## Subagent and subgraph discovery

For multi-agent graphs, `useStream` tracks which subagents and subgraphs it has seen on the current run as cheap discovery maps. `useLangChainSubagents` and `useLangChainSubgraphs` surface those maps — keyed by namespace, each value a `SubagentDiscoverySnapshot` / `SubgraphDiscoverySnapshot`. Both default to a stable empty map, so you can iterate without a guard.

These replace `react-langgraph`'s subgraph `eventHandlers`: v1 surfaces discovery as maps you read reactively rather than per-event callbacks.

```
import {
  useLangChainSubagents,
  useLangChainSubgraphs,
} from "@assistant-ui/react-langchain";

function Discovered() {
  const subagents = useLangChainSubagents();
  const subgraphs = useLangChainSubgraphs();
  return (
    <ul>
      {[...subagents.keys()].map((ns) => (
        <li key={ns}>subagent: {ns}</li>
      ))}
      {[...subgraphs.keys()].map((ns) => (
        <li key={ns}>subgraph: {ns}</li>
      ))}
    </ul>
  );
}
```

## Subagent and subgraph views

The discovery maps above pair with v1's scoped selector hooks to render a subagent's or subgraph's own messages and tool calls. `useLangChainStream` exposes the underlying `useStream` handle; feed it to `useMessages(stream, target)` / `useToolCalls(stream, target)` from `@langchain/react`, with a `SubagentDiscoverySnapshot` / `SubgraphDiscoverySnapshot` from `useLangChainSubagents` / `useLangChainSubgraphs` as the target. `useLangChainStream` returns `undefined` outside the runtime provider.

```
import {
  useLangChainStream,
  useLangChainSubagents,
} from "@assistant-ui/react-langchain";
import { useMessages, useToolCalls } from "@langchain/react";

function SubagentCard({ stream, subagent }) {
  const messages = useMessages(stream, subagent);
  const toolCalls = useToolCalls(stream, subagent);
  return (
    <div>
      <strong>{subagent.namespace.join("/")}</strong>
      <pre>{messages.map((m) => m.content).join("\n")}</pre>
      <ul>
        {toolCalls.map((tc) => (
          <li key={tc.id}>{tc.name}</li>
        ))}
      </ul>
    </div>
  );
}

function Subagents() {
  const stream = useLangChainStream();
  const subagents = useLangChainSubagents();
  if (!stream) return null;
  return [...subagents.values()].map((s) => (
    <SubagentCard key={s.namespace.join("/")} stream={stream} subagent={s} />
  ));
}
```

## Per-message metadata

`useMessageMetadata` (from `@langchain/react`) reads per-message metadata (e.g. `parentCheckpointId`) for a given message id. Pair it with `useLangChainStream`, which returns `undefined` outside the runtime provider; guard the stream before calling the selector, since `useMessageMetadata` throws on an undefined stream.

```
import { useLangChainStream } from "@assistant-ui/react-langchain";
import { useMessageMetadata } from "@langchain/react";

function MessageInfoView({
  stream,
  messageId,
}: {
  stream: NonNullable<ReturnType<typeof useLangChainStream>>;
  messageId: string;
}) {
  const { parentCheckpointId } = useMessageMetadata(stream, messageId) ?? {};
  // …render per-message metadata…
}

function MessageInfo({ messageId }: { messageId: string }) {
  const stream = useLangChainStream();
  if (!stream) return null;
  return <MessageInfoView stream={stream} messageId={messageId} />;
}
```

## Media

The v1 media hooks surface multimodal media streamed by the agent. Feed the `useLangChainStream` handle to them — the same pattern as the views and metadata sections above.

```
import { useLangChainStream } from "@assistant-ui/react-langchain";
import { useImages, useAudio } from "@langchain/react";

function MediaView({
  stream,
}: {
  stream: NonNullable<ReturnType<typeof useLangChainStream>>;
}) {
  const images = useImages(stream);
  // useAudio / useVideo / useFiles likewise
  return <>{/* …render media… */}</>;
}

function Media() {
  const stream = useLangChainStream();
  if (!stream) return null;
  return <MediaView stream={stream} />;
}
```

## Generative UI

Graphs can accumulate UI components in their state and attach each one to the assistant message that produced it. The runtime reads these from `stream.values[uiStateKey]` (default `"ui"`) and, for every `UIMessage` whose parent id matches an assistant message, emits a `data` part on that message. The parent id is read from `metadata.message_id` (Python SDK) or `metadata.id` (JS SDK). Pass `uiStateKey` if your graph stores them elsewhere.

Register the components with `makeAssistantDataUI`, keyed by the `UIMessage`'s `name`, and mount the returned element once inside the runtime provider:

```
import {
  AssistantRuntimeProvider,
  makeAssistantDataUI,
  Thread,
} from "@assistant-ui/react";
import { useStreamRuntime } from "@assistant-ui/react-langchain";

const ChartUI = makeAssistantDataUI({
  name: "chart",
  render: ({ data }) => <Chart points={data.points} />,
});

function App() {
  const runtime = useStreamRuntime({
    assistantId: "agent",
    apiUrl: "http://localhost:2024",
  });

  return (
    <AssistantRuntimeProvider runtime={runtime}>
      <ChartUI />
      <Thread />
    </AssistantRuntimeProvider>
  );
}
```

This covers two paths transparently. The **state-snapshot** path renders whatever UI the graph has committed to its state. The **live** path renders UI streamed over the `custom` channel by `push_ui_message` / `remove_ui_message` before (or instead of) it lands in state, so components appear as the graph emits them. The runtime accumulates the live events and merges them with the state snapshot, with the snapshot authoritative once a UI lands in state. Both feed the same `makeAssistantDataUI` renderers.

The live path requires the graph's `streamMode` to include `"custom"` so the UI events reach the client.

## Errors

`useLangChainError` exposes the last run or hydration error from upstream `useStream().error`. It is untyped (`unknown`), so narrow it at the use site before rendering.

```
import { useLangChainError } from "@assistant-ui/react-langchain";

function ErrorBanner() {
  const error = useLangChainError();
  if (!error) return null;
  return <div role="alert">{error instanceof Error ? error.message : String(error)}</div>;
}
```

## Message conversion

`convertLangChainBaseMessage` transforms a LangChain `BaseMessage` into an assistant-ui message. Use it when building a custom `ExternalStoreAdapter` that consumes LangChain messages outside `useStreamRuntime`.

```
import { convertLangChainBaseMessage } from "@assistant-ui/react-langchain";
```

## Cloud persistence

Pass an `AssistantCloud` instance to persist threads across sessions. The runtime automatically wires thread list management and resumes state from the cloud.

```
// see "AssistantCloud" in /docs/runtimes/concepts/threads for cloud setup
const runtime = useStreamRuntime({
  cloud,
  assistantId: "agent",
  apiUrl: "http://localhost:2024",
});
```

## Custom `messagesKey`

If your graph stores messages under a non-default key, pass `messagesKey` so the runtime submits tool results and human turns to the correct state slot:

```
const runtime = useStreamRuntime({
  assistantId: "agent",
  apiUrl: "http://localhost:2024",
  messagesKey: "chat_messages",
});
```

## Comparison with `react-langgraph`

Both packages connect assistant-ui to LangGraph backends. They are independent adapters for different upstream libraries; one is not a successor to the other. The official `create-assistant-ui` template (`-t langchain`) ships `react-langchain`. The two are now at feature parity; `react-langgraph` remains the choice for a fully custom backend stream or an existing `react-langgraph` app.

| Aspect                         | `react-langgraph`                    | `react-langchain`                     |
| ------------------------------ | ------------------------------------ | ------------------------------------- |
| Wraps                          | `@langchain/langgraph-sdk` (raw SDK) | `@langchain/react` (`useStream` hook) |
| Age                            | Sept 2024 onward                     | April 2026 onward                     |
| Version                        | `0.13.x`                             | `0.0.x`                               |
| Lines of source                | \~7,500                              | \~600                                 |
| Built on                       | `useExternalStoreRuntime`            | `useExternalStoreRuntime`             |
| `create-assistant-ui` template | No template                          | `-t langchain`                        |

### Feature coverage

| Feature                                           | `react-langgraph`                      | `react-langchain`                                                         |
| ------------------------------------------------- | -------------------------------------- | ------------------------------------------------------------------------- |
| Stream messages                                   | Yes (`useLangGraphRuntime`)            | Yes (`useStreamRuntime`)                                                  |
| Interrupt state                                   | Yes                                    | Yes                                                                       |
| Thread history loading state (`thread.isLoading`) | Yes                                    | Yes                                                                       |
| Send raw state update / resume command            | Yes                                    | Yes (`useLangChainSubmit`; `useLangChainRespond` for payload resume)      |
| Forward per-run `runConfig`                       | Yes (whole object → `config`)          | Yes (`custom` → `config.configurable`)                                    |
| Read arbitrary custom state key                   | No                                     | Yes (`useLangChainState<T>(key)`)                                         |
| Root tool-calls projection                        | Via message parts                      | Yes (`useLangChainToolCalls`)                                             |
| File content parts in messages                    | Yes                                    | Yes                                                                       |
| Regenerate (checkpoint fork)                      | Yes (`getCheckpointId`)                | Yes (auto-resolved)                                                       |
| Edit message (checkpoint fork)                    | Yes                                    | Yes (auto-resolved)                                                       |
| Per-message metadata (`messages-tuple`)           | Yes                                    | Yes (`useMessageMetadata` via `useLangChainStream`)                       |
| Multimodal media (audio/image/video/file)         | Via parts                              | Yes (v1 media hooks via `useLangChainStream`)                             |
| Generative UI (state snapshot)                    | Yes                                    | Yes (`uiStateKey`)                                                        |
| Generative UI (live `push_ui_message`)            | Yes                                    | Yes (`streamMode: ["custom"]`)                                            |
| Subgraph / namespaced stream events               | Yes (via `eventHandlers`)              | Replaced by `useLangChainSubagents` / `useLangChainSubgraphs` + views     |
| Subagent discovery                                | Via `eventHandlers`                    | Yes (`useLangChainSubagents`)                                             |
| Subgraph discovery                                | Via `eventHandlers`                    | Yes (`useLangChainSubgraphs`)                                             |
| Subagent/subgraph message views                   | Via `eventHandlers`                    | Yes (via `useLangChainStream` + v1 selector hooks)                        |
| End-to-end cancellation primitive                 | Yes (`unstable_createLangGraphStream`) | n/a — cancellation via `useStream().stop()` (Cancel button on by default) |
| Message accumulator utility                       | Yes (`LangGraphMessageAccumulator`)    | n/a — `useStream` owns accumulation                                       |
| Streaming timing (per-message)                    | Yes                                    | Yes                                                                       |
| Cloud thread persistence                          | Yes                                    | Yes                                                                       |

Per-message streaming timing is attached automatically to `message.metadata.timing` (no setup), matching `react-langgraph`. It captures time-to-first-token, total stream time, chunk count, and tokens/sec, computed from message growth while the run is in flight.

`react-langchain` is the newer, thinner wrapper. It delegates to upstream `useStream` rather than re-implementing the stream plumbing, which is why its footprint is smaller.

Regenerate works without configuration. Clicking regenerate resolves the server checkpoint to fork from (the checkpoint each message records as it streams in, falling back to a `stream.client.threads.getHistory(threadId)` id match for older turns), then re-runs with `submit(null, { forkFrom })`. It works on checkpoint-enabled backends (for example LangGraph Cloud) and degrades gracefully; when no checkpoint resolves the regenerate is a no-op. `react-langgraph` instead requires a user-supplied `getCheckpointId` callback. Editing a message forks from the same checkpoint and submits the edited human content instead of re-running the prior turn; editing the first message forks from the thread's initial checkpoint to restart it.

### Hook name mapping

| `react-langgraph`             | `react-langchain`                                                 | Notes                                                                                                                       |
| ----------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `useLangGraphRuntime`         | `useStreamRuntime`                                                | Options extend upstream `UseStreamOptions`; no `stream` / `create` / `load` to write.                                       |
| `useLangGraphInterruptState`  | `useLangChainInterruptState`                                      | Same return shape.                                                                                                          |
| `useLangGraphSendCommand`     | `useLangChainRespond`                                             | Preferred resume; carries a payload via `stream.respond`. For a raw `Command`, use `useLangChainSubmit(null, { command })`. |
| `useLangGraphSend`            | *(use `runtime.thread.append`)*                                   | No direct equivalent; send turns through the runtime.                                                                       |
| `useLangGraphMessageMetadata` | *(use `useMessageMetadata(useLangChainStream(), id)`)*            | Per-message metadata via the upstream selector hook.                                                                        |
| `useLangGraphUIMessages`      | *(UI renders as data parts; register with `makeAssistantDataUI`)* | See [Generative UI](#generative-ui).                                                                                        |
| *(none)*                      | `useLangChainState<T>(key)`                                       | New — reads any custom state key reactively.                                                                                |
| *(none)*                      | `useLangChainToolCalls`                                           | New — root tool calls assembled by `useStream`.                                                                             |
| *(none)*                      | `useLangChainError()`                                             | New — reads the last run/hydration error reactively.                                                                        |
| *(none)*                      | `useLangChainRespondAll`                                          | Resume multiple interrupts at one checkpoint (`stream.respondAll`).                                                         |
| *(none)*                      | `useLangChainInterrupts`                                          | New — every interrupt pending at the checkpoint, each with `id` and `value`.                                                |
| *(via `eventHandlers`)*       | `useLangChainSubagents`                                           | New — subagents discovered on the current run; replaces subgraph event handlers.                                            |
| *(via `eventHandlers`)*       | `useLangChainSubgraphs`                                           | New — subgraphs discovered on the current run; replaces subgraph event handlers.                                            |
| *(none)*                      | `useLangChainStream`                                              | New — the raw `useStream` handle for v1's scoped `useMessages` / `useToolCalls` selector hooks.                             |

## Related

- [LangGraph](/docs/runtimes/langgraph/overview) — The original LangGraph adapter, built on the raw SDK.
- [ExternalStoreRuntime](/docs/runtimes/custom/external-store) — The core runtime react-langchain is built on.