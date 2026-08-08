# Model Context Hooks
URL: /docs/api-reference/hooks/model-context

React hooks for registering assistant-ui tools, data renderers, instructions, and model context providers.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### useAuiToolOverrides

> [!tip]
>
> **Experimental.** Experimental, API may change.

Overrides toolkit entries for the current assistant scope.

This is intended for dynamic local-state tools whose model-facing contract is declared in a `"use generative"` toolkit file with `execute: stubTool()`, but whose actual executor must close over React state in the mounted component. Keep the override keys stable after mount; dynamic key addition/removal is not currently observed. Overrides are registered at priority 1000, above toolkit defaults. Only one mounted override provider may define a given tool name at a time.

- `overrides`: `AuiToolOverrides`