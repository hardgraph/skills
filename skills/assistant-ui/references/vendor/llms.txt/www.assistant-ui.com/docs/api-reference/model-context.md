# Model Context API Reference
URL: /docs/api-reference/model-context

Model instructions, contextual state, provider registries, and renderers for giving assistant-ui runtimes app-aware context.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Model context is the layer that tells the assistant what the surrounding app knows. Use it to contribute instructions, register contextual providers, and keep model-facing state close to the React components that own it.

## Pages

- [Model Context](/docs/api-reference/model-context/context) — Provide model instructions, contextual state, and inline renderers to assistant-ui runtimes.
- [Model Context Registry](/docs/api-reference/model-context/registry) — Register and manage assistant-ui model context providers that contribute instructions and app state.