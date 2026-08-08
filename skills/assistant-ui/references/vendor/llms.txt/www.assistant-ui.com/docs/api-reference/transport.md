# Transport API Reference
URL: /docs/api-reference/transport

Transport commands, frame messages, and protocol types for synchronizing assistant-ui runtimes across process or iframe boundaries.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Transport APIs are for runtimes that cross a network, worker, iframe, or other execution boundary. They document the command and frame contracts that keep assistant-ui state synchronized with an external process.

## Pages

- [Assistant Transport](/docs/api-reference/transport/assistant-transport) — Command, protocol, and transport types for connecting assistant-ui runtimes across execution boundaries.
- [Assistant Frame](/docs/api-reference/transport/frame) — Frame bridge APIs and serialized message types for embedding assistant-ui runtimes in external contexts.