# Model Context Registry
URL: /docs/api-reference/model-context/registry

Register and manage assistant-ui model context providers that contribute instructions and app state.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ModelContextRegistry

- `getModelContext?`: `() => ModelContext`
- `subscribe?`: `(callback: () => void) => Unsubscribe`
- `addTool?`: `(tool: AssistantToolProps<TArgs, TResult>) => ModelContextRegistryToolHandle<TArgs, TResult>`
- `addInstruction?`: `(config: string | AssistantInstructionsConfig) => ModelContextRegistryInstructionHandle`
- `addProvider?`: `(provider: ModelContextProvider) => ModelContextRegistryProviderHandle`