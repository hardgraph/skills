# Threads
URL: /docs/runtimes/langgraph/threads

Basic thread support, AssistantCloud, and custom thread list adapter.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`useLangGraphRuntime` supports the same three-path thread model documented in [threads](/docs/runtimes/concepts/threads), tailored for LangGraph thread ids. This page covers the LangGraph-specific wiring.

## Basic thread support

`useLangGraphRuntime` includes built-in thread management:

```
const runtime = useLangGraphRuntime({
  stream: async (messages, { initialize, ...config }) => {
    // initialize() creates or loads a thread and returns its IDs
    const { remoteId, externalId } = await initialize();
    // Use externalId (your backend's thread ID) for API calls
    return sendMessage({ threadId: externalId, messages, config });
  },
  create: async () => {
    // Called when creating a new thread
    const { thread_id } = await createThread();
    return { externalId: thread_id };
  },
  load: async (externalId) => {
    // Called when loading an existing thread
    const state = await getThreadState(externalId);
    return {
      messages: state.values.messages,
      interrupts: state.tasks[0]?.interrupts,
    };
  },
});
```

## Cloud persistence

For managed multi-thread support, persistence, sync, and titles, pass an `AssistantCloud` instance:

```
const runtime = useLangGraphRuntime({
  cloud, // see "AssistantCloud" in /docs/runtimes/concepts/threads
  // ... stream, create, load functions
});
```

See the [cloud persistence guide](/docs/cloud/langgraph) for setup details.

## Custom thread list

To surface pre-existing LangGraph `thread_id`s in the thread picker without running assistant-cloud, pass a `RemoteThreadListAdapter` via `unstable_threadListAdapter`. A common implementation backs `list()` with `client.threads.search()` and `initialize()` with `client.threads.create()`.

```
import type { RemoteThreadListAdapter } from "@assistant-ui/react";
import { Client } from "@langchain/langgraph-sdk";

const client = new Client({
  apiUrl: process.env.NEXT_PUBLIC_LANGGRAPH_API_URL,
});

const threadListAdapter: RemoteThreadListAdapter = {
  async list() {
    const threads = await client.threads.search({ limit: 50 });
    return {
      threads: threads.map((t) => ({
        status: "regular",
        remoteId: t.thread_id,
        externalId: t.thread_id,
        title: (t.metadata as { title?: string } | undefined)?.title,
      })),
    };
  },
  async initialize() {
    const t = await client.threads.create();
    return { remoteId: t.thread_id, externalId: t.thread_id };
  },
  async delete(remoteId) {
    await client.threads.delete(remoteId);
  },
  // rename, archive, unarchive, fetch, generateTitle — see the threads concept page
};

const runtime = useLangGraphRuntime({
  stream: async function* (messages, { initialize }) {
    /* ... */
  },
  load: async (externalId) => {
    /* ... */
  },
  unstable_threadListAdapter: threadListAdapter,
});
```

Setting `remoteId === externalId` keeps the ids assistant-ui stores aligned with the LangGraph thread ids your `load` and `stream` callbacks receive. See [threads](/docs/runtimes/concepts/threads) for the full adapter contract.

> [!info]
>
> When `unstable_threadListAdapter` is provided, the `cloud`, `create`, and `delete` options are ignored; the adapter owns the full thread-list lifecycle.

## Next

- [Streaming](/docs/runtimes/langgraph/streaming) — Event handlers, message metadata, generative UI.
- [Threads (concept)](/docs/runtimes/concepts/threads) — General multi-thread model across runtimes.