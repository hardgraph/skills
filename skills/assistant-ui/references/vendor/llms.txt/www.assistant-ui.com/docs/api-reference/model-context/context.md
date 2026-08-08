# Model Context
URL: /docs/api-reference/model-context/context

Provide model instructions, contextual state, and inline renderers to assistant-ui runtimes.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### makeAssistantVisible

```
const makeAssistantVisible: <T extends ComponentType<any>>(Component: T, config?: { clickable?: boolean | undefined; editable?: boolean | undefined; }) => T;
```

### mergeModelContexts

```
const mergeModelContexts: (configSet: Set<ModelContextProvider>) => ModelContext;
```

### ModelContextClient

```
const ModelContextClient: Resource<ClientOutput<"modelContext">, []>;
```

### ModelContextProvider

- `getModelContext`: `() => ModelContext`
- `subscribe?`: `(callback: () => void) => Unsubscribe`

### useAssistantContext

- `config`: `AssistantContextConfig`

  - `getContext`: `() => string`
  - `disabled?`: `boolean`

### useAssistantInstructions

- `config`: `string | AssistantInstructionsConfig`

### useInlineRender

- `toolUI`: `FC<ToolCallMessagePart<ReadonlyJSONObject, unknown> & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; } & ToolCallMessagePart<TArgs, TResult> & { addResult: (result: TResult | ToolResponse<TResult>) => void; resume: (payload: unknown) => void; respondToApproval: (response: ToolApprovalResponse) => void; }>`