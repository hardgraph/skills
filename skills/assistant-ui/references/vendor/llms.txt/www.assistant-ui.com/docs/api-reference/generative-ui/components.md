# Generative UI Components
URL: /docs/api-reference/generative-ui/components

Define the component library JSONGenerativeUI can render, including schemas, render functions, and the default vocabulary.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### defaultGenerativeUILibrary

The closed generative-ui vocabulary the factory ships: a fixed set of intrinsic components the model may render, each a zod `properties` schema plus an unstyled structural `render` (semantic HTML with a `data-aui` attribute naming the component, plus `data-aui-<prop>` hooks, for the host to style). Users opt in by passing it to `new JSONGenerativeUI({ library: defaultGenerativeUILibrary })`, and override or extend entries with their own `defineGenerativeComponents`.

```
const defaultGenerativeUILibrary: GenerativeUILibrary;
```

### defineGenerativeComponents

Authoring helper for a `"use generative"` generative-UI library — the set of components the model may render. Each component colocates its `properties` schema (kept on every build, drives the tool parameters) with its `render` (kept only on the client). Pass the result to [JSONGenerativeUI](/docs/api-reference/generative-ui/json-generative-ui#jsongenerativeui):

```
"use generative";
const generative = new JSONGenerativeUI({
  library: defineGenerativeComponents({
    Card: {
      description: "A card.",
      properties: z.object({ title: z.string() }),
      render: (props) => <Card {...props} />,
    },
  }),
});
```

Unlike [defineToolkit](/docs/api-reference/tools/toolkits#definetoolkit), it has **no runtime implementation**. A `"use generative"` compiler unwraps the `defineGenerativeComponents(...)` call per build, dropping each `render` (and its client-only imports) from the server build. Reaching it at runtime means the module wasn't compiled (the directive is missing, or it was used outside a `"use generative"` file), so it throws rather than shipping client `render` code to the server.

- `_library`: `GenerativeUILibrary`

### GenerativeUIComponent

A component the model is allowed to render, with the schema for its props.

Components opt into prop streaming with `streamProperties`: they render as props arrive, so `render` sees `Partial<P>` while `$status` is `"streaming"` and the full `P` once it is `"done"`. By default a component opts out and is only rendered once its props are complete.

`render` is a function of `props`, not a React component. Direct hook use is fine (each node is mounted on its own fiber), but a given node must keep its `type` across renders for hook identity to be stable.

- `description`: `string` — Natural-language description shown to the model when selecting the component.
- `properties`: `ZodType<P>` — Schema for the props the model must provide. Drives the tool parameters.
- `streamProperties?`: `boolean` — Render from partially-streamed props. Widened to \`boolean | undefined\` so a non-literal value (e.g. a variable) still resolves to this branch cleanly rather than matching neither; the strict \`false | undefined\` branch below is the only one that promises complete props.
- `render`: `(props: StreamingRenderProps<P>) => ReactNode`

### GenerativeUILibrary

The consumer-provided allowlist of components the model is permitted to render. Keys are the `type` values referenced in the generative-ui tree (e.g. `"Card"`, `"Button"`); values describe each component.

This registry is the security boundary — any `type` not present is rejected.

```
type GenerativeUILibrary = Record<string, GenerativeUIComponent>;
```