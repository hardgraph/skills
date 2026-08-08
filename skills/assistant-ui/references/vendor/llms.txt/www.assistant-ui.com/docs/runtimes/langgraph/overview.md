# LangGraph UI Runtime
URL: /docs/runtimes/langgraph/overview

Build a chat UI for LangGraph agents in React with assistant-ui — streaming, subgraph events, UI messages, interrupts, and end-to-end cancellation supported.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`@assistant-ui/react-langgraph` integrates with [`@langchain/langgraph-sdk`](https://docs.langchain.com/oss/javascript/langgraph-sdk) directly, exposing the full LangGraph Cloud feature set in assistant-ui: streaming, subgraph events, UI messages, message metadata, interrupts, and end-to-end cancellation.

> [!info]
>
> **Migrating from LangServe?** LangChain has deprecated `RemoteRunnable` upstream and consolidated the chain and agent execution stories under LangGraph. Rebuild your chain as a LangGraph graph (LangChain's [LangServe → LangGraph migration guide](https://github.com/langchain-ai/langserve/blob/main/MIGRATION.md) covers the shape change), deploy via LangGraph Cloud or self-host LangGraph Studio, and use this runtime on the frontend. The runtime ships streaming, interrupts, generative UI, message metadata, and end-to-end cancellation out of the box.

## When to use it

Pick the LangGraph runtime when:

- You have (or want) a LangGraph Cloud server, locally via [LangGraph Studio](https://docs.langchain.com/langsmith/quick-start-studio) or hosted via [LangSmith](https://www.langchain.com/langsmith).
- Your graph state has a `messages` key with LangChain-alike messages.
- You want generative UI (`ui_message`), per-message metadata, subgraph events, or checkpoint-based message editing.

> [!info]
>
> If you are already using `@langchain/react`'s `useStream` hook, the alternative [`@assistant-ui/react-langchain`](/docs/runtimes/langchain) adapter may fit better. It delegates the stream plumbing to the upstream hook; the feature surface differs, see [LangChain useStream](/docs/runtimes/langchain) for the comparison table.

## Architecture

`@assistant-ui/react-langgraph` is layered on `ExternalStoreRuntime` (see [architecture](/docs/runtimes/concepts/architecture)). Graph state is the source of truth; the runtime renders messages from `state.values.messages` and submits user input back to the graph.

Shared adapters (attachments, speech, feedback, history) work the same way described in [adapters](/docs/runtimes/concepts/adapters).

## Requirements

- A LangGraph Cloud API server.
- React 18 or 19.
- The graph state must include a `messages` key with LangChain-alike messages.

## Install

**React**

```bash
npm install @assistant-ui/react @assistant-ui/react-langgraph @langchain/langgraph-sdk
```

## Next

- [Quickstart](/docs/runtimes/langgraph/quickstart) — From-template and manual setup paths to a working LangGraph chat.
- [Streaming](/docs/runtimes/langgraph/streaming) — Event handlers, message metadata, generative UI.
- [Interrupts](/docs/runtimes/langgraph/interrupts) — Interrupt persistence and checkpoint-based message editing.
- [Threads](/docs/runtimes/langgraph/threads) — Basic thread support, AssistantCloud, custom thread list adapter.
- [Tutorial: Stockbroker](/docs/runtimes/langgraph/tutorial/introduction) — End-to-end stockbroker assistant with generative UI and approval flows.