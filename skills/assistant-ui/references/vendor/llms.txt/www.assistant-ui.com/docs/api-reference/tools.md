# Tools API Reference
URL: /docs/api-reference/tools

Tool definitions, React renderers, status helpers, and toolkits for exposing callable app capabilities to assistant-ui chat models.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Use these APIs when your assistant needs to call application code, render a custom tool result, or expose tool state inside a React UI.

Toolkits are the current registration API. Tool **rendering** and **status** are independent of the tool's execution location and apply to frontend, backend, human, and MCP tools.

|                  | `Tools({ toolkit })`                                                                   |
| ---------------- | -------------------------------------------------------------------------------------- |
| Scope            | Entire assistant subtree below the mount point                                         |
| Lifecycle        | Tied to the runtime / provider tree                                                    |
| Typical use      | App-wide tools, dynamic scoped tools, MCP merging, tools defined alongside the runtime |
| Stable reference | Pass a stable `toolkit` (module scope or `useMemo`)                                    |

`useAssistantTool`, `makeAssistantTool`, `useAssistantToolUI`, and `makeAssistantToolUI` are deprecated compatibility APIs. See [Migrating Tools to Toolkits](/docs/migrations/toolkit-tools).

## Pages

- [Toolkits](/docs/api-reference/tools/toolkits) — Define model-facing tools and compose them into named toolkits registered with an assistant-ui runtime scope.
- [Component Tools](/docs/api-reference/tools/component-tools) — Register assistant tools from mounted React components, scoped to the lifetime of part of the UI tree.
- [Tool Rendering](/docs/api-reference/tools/rendering) — Register React renderers for assistant-ui tool calls, tool results, and model data parts.
- [Tool Status](/docs/api-reference/tools/status) — Read tool arguments, execution status, and result state inside assistant-ui tool UI components.
- [Interactables](/docs/api-reference/tools/interactables) — Unstable interactables APIs for model-editable app and message state, including hooks, resources, toolkit helpers, and snapshot utilities.
- [Interactables (legacy)](/docs/api-reference/tools/interactables-legacy) — Deprecated legacy interactables APIs for registering model-editable components with per-instance update tools.