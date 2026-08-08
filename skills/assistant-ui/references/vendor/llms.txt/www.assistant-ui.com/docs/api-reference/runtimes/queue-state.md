# QueueItemState
URL: /docs/api-reference/runtimes/queue-state

State shape for queued assistant-ui thread operations and pending runtime work.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### QueueItemState

- `id`: `string`
- `prompt`: `string` (deprecated: Derive from the text parts of \`parts\` instead. Removal after 2026-11-05.)
- `parts`: `readonly (FileMessagePart | TextMessagePart)[]`