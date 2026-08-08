# A2A Agent Runtime
URL: /docs/runtimes/a2a/overview

Connect any A2A v1.0 protocol-compliant agent server to a React chat UI with assistant-ui — full streaming, tool calls, and message metadata supported.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

`@assistant-ui/react-a2a` is a runtime adapter for the [A2A (Agent-to-Agent) v1.0 protocol](https://github.com/a2aproject/A2A). Use it when your backend speaks A2A; the adapter handles the wire protocol so you can focus on the UI.

## When to use it

Pick the A2A runtime when:

- You have an A2A v1.0 compatible agent server, or you are building one and want a reference client.
- You want streaming task state, artifacts, and the protocol's full state machine surfaced in the UI.
- You need multi-tenant or extension-aware A2A clients.

If your backend is not A2A-compliant, see [picking a runtime](/docs/runtimes/pick-a-runtime) for alternatives.

## Architecture

`@assistant-ui/react-a2a` is layered on `ExternalStoreRuntime` (see [architecture](/docs/runtimes/concepts/architecture)). Features that ship as runtime adapters (attachments, speech, feedback, history, threads) work the same way they do everywhere else; see [adapters](/docs/runtimes/concepts/adapters).

## Requirements

- An A2A v1.0 compatible agent server.
- React 18 or 19.

## Install

**React**

```bash
npm install @assistant-ui/react @assistant-ui/react-a2a
```

## Next

- [Quickstart](/docs/runtimes/a2a/quickstart) — Minimal runtime setup with a Thread component.
- [Client and hooks](/docs/runtimes/a2a/client-and-hooks) — A2AClient, useA2ARuntime options, hooks reference, task states, artifacts.