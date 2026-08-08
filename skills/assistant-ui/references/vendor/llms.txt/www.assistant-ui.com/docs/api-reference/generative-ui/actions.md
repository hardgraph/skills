# Generative UI Actions
URL: /docs/api-reference/generative-ui/actions

Register handlers for model-emitted $action payloads and dispatch them from interactive generative UI components.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ActionDispatchContext

Context handed to an [ActionHandler](/docs/api-reference/generative-ui/actions#actionhandler) when an `$action` fires.

- `payload`: `Action` — The action payload. For fire-and-forget actions this is the \`$action\` as the model emitted it. For an interactive component the user's runtime input is merged in under the reserved \`$input\` key: a single value for a standalone control's dispatch, or an object keyed by each field's \`name\` for a \`Form\` or a \`Card\` with \`asForm\` set. A model-supplied \`value\` field is never clobbered.
  - `type`: `string`

### ActionHandler

Resolves a single `$action.type`. Fire-and-forget actions return `void` or a promise that resolves to it; human-in-the-loop actions return a value the runtime uses to resume the run (see IR doc Open question #2 — the resume value is left as `unknown` and documented per-action). The return value is handed back to `dispatch`'s caller as `unknown`.

```
type ActionHandler = (
  ctx: ActionDispatchContext,
) => unknown | Promise<unknown>;
```

### ActionRegistry

The host-provided map from `$action.type` to [ActionHandler](/docs/api-reference/generative-ui/actions#actionhandler). This is the single dispatch target for rendered generative UI: every interactive component fires its `$action` here through the injected `$dispatch` (a submit Button defers to its ancestor form's dispatch instead). The payload's `$input` takes one of two shapes: a single value for a standalone control's dispatch (`Select`, `Input`, `DatePicker`, `Checkbox`, `RadioGroup`), or an object keyed by field `name` for a `Form` or a `Card` with `asForm` set. Construct it with [createActionRegistry](/docs/api-reference/generative-ui/actions#createactionregistry) and pass it to `JSONGenerativeUI`.

- `dispatch`: `(action: Action) => unknown`
- `has`: `(type: string) => boolean`

### createActionRegistry

- `handlers`: `Readonly<Record<string, ActionHandler>>`

### emptyActionRegistry

A no-op registry used when no handlers are provided. Dispatch resolves to `undefined` and logs a warning in dev for unknown types, so a model-emitted action with no handler degrades to "does nothing" rather than throwing.

```
const emptyActionRegistry: ActionRegistry;
```