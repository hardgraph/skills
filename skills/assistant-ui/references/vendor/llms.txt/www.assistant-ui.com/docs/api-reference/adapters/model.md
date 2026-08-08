# Model Adapters
URL: /docs/api-reference/adapters/model

Adapter interfaces for connecting chat models, streaming responses, and model execution to assistant-ui runtimes.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ChatModelAdapter

- `run`: `(options: ChatModelRunOptions) => Promise<ChatModelRunResult> | AsyncGenerator<ChatModelRunResult, void>`

### ChatModelRunOptions

- `messages`: `readonly ThreadMessage[]`

- `runConfig`: `RunConfig`
  - `custom?`: `Record<string, unknown>`

- `abortSignal`: `AbortSignal`

  - `aborted`: `boolean`
  - `onabort`: `((this:AbortSignal,ev:Event)=>any)|null`
  - `reason`: `any`
  - `throwIfAborted`: `() => void`
  - `addEventListener`: `{ <K>(type: K, listener: (this: AbortSignal, ev: AbortSignalEventMap[K]) => any, options?: boolean | AddEventListenerOptions): void; (type: string, listener: EventListenerOrEventListenerObject, options?: boolean | AddEventListenerOptions): void; }`
  - `removeEventListener`: `{ <K>(type: K, listener: (this: AbortSignal, ev: AbortSignalEventMap[K]) => any, options?: boolean | EventListenerOptions): void; (type: string, listener: EventListenerOrEventListenerObject, options?: boolean | EventListenerOptions): void; }`
  - `dispatchEvent`: `(event: Event) => boolean` — The \*\*\`dispatchEvent()\`\*\* method of the EventTarget sends an Event to the object, (synchronously) invoking the affected event listeners in the appropriate order. The normal event processing rules (including the capturing and optional bubbling phase) also apply to events dispatched manually with dispatchEvent(). [MDN Reference](https://developer.mozilla.org/docs/Web/API/EventTarget/dispatchEvent)

- `context`: `ModelContext`

  - `priority?`: `number`

  - `system?`: `string`

  - `tools?`: `Record<string, Tool<any, any>>`

  - `callSettings?`: `LanguageModelV1CallSettings`

    - `maxTokens?`: `number`
    - `temperature?`: `number`
    - `topP?`: `number`
    - `presencePenalty?`: `number`
    - `frequencyPenalty?`: `number`
    - `seed?`: `number`
    - `headers?`: `Record<string, string | undefined>`

  - `config?`: `LanguageModelConfig`

    - `apiKey?`: `string`
    - `baseUrl?`: `string`
    - `modelName?`: `string`
    - `reasoningEffort?`: `string`

  - `unstable_composerMetadata?`: `Record<string, unknown>` — Persisted message metadata pulled at send time and merged into the outgoing user message's \`metadata.custom\` (not forwarded to the model directly). Ignored by the transport, which only reads system/tools/callSettings/config.

- `unstable_assistantMessageId?`: `string`

- `unstable_threadId?`: `string`

- `unstable_parentId?`: `string | null`

- `unstable_getMessage`: `() => ThreadMessage`

### ChatModelRunResult

- `content?`: `readonly ThreadAssistantMessagePart[]`

- `status?`: `MessageStatus`

- `metadata?`: `ChatModelRunResult["metadata"]`

  - `unstable_state?`: `ReadonlyJSONValue`

  - `unstable_annotations?`: `readonly ReadonlyJSONValue[]`

  - `unstable_data?`: `readonly ReadonlyJSONValue[]`

  - `steps?`: `readonly ThreadStep[]`

  - `timing?`: `MessageTiming`

    - `streamStartTime`: `number`
    - `firstTokenTime?`: `number`
    - `totalStreamTime?`: `number`
    - `tokenCount?`: `number`
    - `tokensPerSecond?`: `number`
    - `totalChunks`: `number`
    - `toolCallCount`: `number`

  - `custom?`: `Record<string, unknown>`

### ChatModelRunUpdate

- `content`: `readonly ThreadAssistantMessagePart[]`
- `metadata?`: `Record<string, unknown>`

### CreateResumeRunConfig

- `parentId`: `string | null`
- `sourceId?`: `string | null`
- `runConfig?`: `RunConfig`
  - `custom?`: `Record<string, unknown>`
- `stream?`: `( options: ChatModelRunOptions, ) => AsyncGenerator<ChatModelRunResult, void, unknown>`

### CreateStartRunConfig

- `parentId`: `string | null`
- `sourceId?`: `string | null`
- `runConfig?`: `RunConfig`
  - `custom?`: `Record<string, unknown>`

### LanguageModelConfig

- `apiKey?`: `string`
- `baseUrl?`: `string`
- `modelName?`: `string`
- `reasoningEffort?`: `string`