# Google ADK Runtime
URL: /docs/runtimes/google-adk/overview

Connect Google's Agent Development Kit (ADK) to a React chat UI with assistant-ui — streaming, tool calls, and multi-agent orchestration supported.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`@assistant-ui/react-google-adk` integrates with [Google ADK JS](https://github.com/google/adk-js), Google's official agent framework for TypeScript, and works with ADK Python backends as well. It supports streaming text, tool calls, multi-agent orchestration, code execution, session state, tool confirmations, auth flows, and input requests.

## When to use it

Pick the Google ADK runtime when:

- You are building agents with ADK JS (`LlmAgent`, sequential / parallel / loop agents) or ADK Python.
- You want first-class support for ADK's HITL (human-in-the-loop) primitives: tool confirmations, auth credential flow, workflow input requests.
- You need multi-agent author and branch tracking, agent transfer events, and grounding / citation metadata.

## Architecture

`@assistant-ui/react-google-adk` is layered on `ExternalStoreRuntime` (see [architecture](/docs/runtimes/concepts/architecture)). Features that ship as runtime adapters work the same way they do everywhere else; see [adapters](/docs/runtimes/concepts/adapters).

The package automatically normalizes snake\_case event fields from ADK Python backends to camelCase. No configuration is needed; connect to either ADK JS or ADK Python servers. All nested fields (`function_call` → `functionCall`, `requested_tool_confirmations` → `requestedToolConfirmations`) are handled.

## Requirements

- A Google ADK agent running on a server (ADK JS or ADK Python).
- React 18 or 19.

## Install

**React**

```bash
npm install @assistant-ui/react @assistant-ui/react-google-adk @google/adk
```

> [!info]
>
> `@google/adk` is only needed on the server side. The client-side runtime has no dependency on it.

## Next

- [Quickstart](/docs/runtimes/google-adk/quickstart) — Minimal API route + client setup with createAdkApiRoute.
- [API reference](/docs/runtimes/google-adk/api) — createAdkStream, server helpers, session adapter, threads, message editing.
- [Hooks](/docs/runtimes/google-adk/hooks) — Tool confirmations, auth, input requests, artifacts, escalation, metadata.