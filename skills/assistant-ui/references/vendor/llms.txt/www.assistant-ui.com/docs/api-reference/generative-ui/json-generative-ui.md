# JSONGenerativeUI
URL: /docs/api-reference/generative-ui/json-generative-ui

Create present and prompt_user tools from a generative UI component library, with options for display mode and action dispatch.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### JSONGenerativeUI

Client build of [JSONGenerativeUI](/docs/api-reference/generative-ui/json-generative-ui#jsongenerativeui), resolved through the package's `default` export condition (browser and SSR).

`present` is a frontend tool that renders the model's `{ $type,...props }` tree against the library and resolves immediately. `prompt_user` is a human-in-the-loop tool: the model pauses and the rendered UI supplies the result. Both draw the tree the same way, so they share one `render`. The `actions` registry (if provided) is threaded into the render context so interactive components can fire `$action` through `$dispatch`.

- `constructor?`: `(options: JSONGenerativeUIOptions) => JSONGenerativeUI`
- `present?`: `(options?: PresentToolOptions) => PresentTool`
- `promptUser?`: `() => PromptUserTool`

### JSONGenerativeUIOptions

Options for [JSONGenerativeUI](/docs/api-reference/generative-ui/json-generative-ui#jsongenerativeui).

- `library`: `GenerativeUILibrary` — The components the model is allowed to render, keyed by the \`$type\` it selects them with. Author it with \`defineGenerativeComponents({... })\` so a \`"use generative"\` build can split each \`render\` from its \`properties\`.

- `actions?`: `ActionRegistry` — Host-provided map from \`$action.type\` to its handler. Interactive components (\`Button\`/\`Select\`/\`Input\`/\`DatePicker\`) call \`$dispatch($action)\` on their event, which resolves through this registry. Omit it for a read-only render where model-emitted actions degrade to a no-op.

  - `dispatch`: `(action: Action) => unknown`
  - `has`: `(type: string) => boolean`

### PresentToolOptions

Options for [JSONGenerativeUI.present](/docs/api-reference/generative-ui/json-generative-ui#jsongenerativeui).

- `display?`: `"standalone"` — Set \`"standalone"\` to render the component on its own surface (outside the chain-of-thought trace), e.g. a full-bleed artifact like a card. Omit it for the default inline rendering — there is no \`"inline"\` value because that is already the default.