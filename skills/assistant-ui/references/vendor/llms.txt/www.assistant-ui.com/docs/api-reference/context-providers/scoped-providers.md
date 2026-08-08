# Scoped Providers
URL: /docs/api-reference/context-providers/scoped-providers

Lower-level assistant-ui providers for custom renderers, scoped message parts, attachments, and advanced composition.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ChainOfThoughtByIndicesProvider

- `startIndex`: `number`
- `endIndex`: `number`

### ComposerAttachmentByIndexProvider

- `index`: `number`

### MessageAttachmentByIndexProvider

- `index`: `number`

### MessageByIndexProvider

- `index`: `number`

### MessageProvider

- `message`: `ThreadMessage`

  - `status?`: `ThreadAssistantMessage["status"]`
    - `type`: `"running"`

  - `metadata`: `ThreadMessage["metadata"]`

    - `unstable_state?`: `ReadonlyJSONValue`

    - `unstable_annotations?`: `readonly ReadonlyJSONValue[]`

    - `unstable_data?`: `readonly ReadonlyJSONValue[]`

    - `steps?`: `readonly ThreadStep[]`

    - `submittedFeedback?`: `ThreadMessage["metadata"]["submittedFeedback"]`
      - `type`: `"positive" | "negative"`

    - `timing?`: `MessageTiming`

      - `streamStartTime`: `number`
      - `firstTokenTime?`: `number`
      - `totalStreamTime?`: `number`
      - `tokenCount?`: `number`
      - `tokensPerSecond?`: `number`
      - `totalChunks`: `number`
      - `toolCallCount`: `number`

    - `isOptimistic?`: `boolean` — Marks a client-side optimistic placeholder. Such messages are evicted once off the head branch and are never persisted.

    - `custom`: `Record<string, unknown>`

  - `attachments?`: `ThreadUserMessage["attachments"]`

  - `id`: `string`

  - `createdAt`: `Date`

  - `role`: `"system"`

  - `content`: `readonly [TextMessagePart]`

- `index`: `number`

- `isLast?`: `boolean`

- `branchNumber?`: `number`

- `branchCount?`: `number`

### PartByIndexProvider

- `index`: `number`

### ReadonlyThreadProvider

- `messages`: `readonly ThreadMessage[]`

### RuntimeAdapterProvider

- `adapters`: `RuntimeAdapters`

  - `modelContext?`: `ModelContextProvider`

    - `getModelContext`: `() => ModelContext`
    - `subscribe?`: `(callback: () => void) => Unsubscribe`

  - `history?`: `ThreadHistoryAdapter`

    - `load`: `() => Promise<ExportedMessageRepository & { state?: ReadonlyJSONValue; unstable_resume?: boolean; }>`
    - `resume?`: `(options: ChatModelRunOptions) => AsyncGenerator<ChatModelRunResult, void, unknown>`
    - `append`: `(item: ExportedMessageRepositoryItem) => Promise<void>`
    - `update?`: `(item: ExportedMessageRepositoryItem) => Promise<void>` — Rewrites a previously appended message in place, keyed by its message id. Adapters that implement this let a runtime persist a run paused for tool approval and finalize the same message once the run resumes. An update may arrive for an id whose earlier write failed; treat it as an upsert keyed on the message id rather than assuming the entry exists.
    - `delete?`: `(items: ExportedMessageRepositoryItem[]) => Promise<void>`
    - `withFormat?`: `<TMessage, TStorageFormat extends Record<string, unknown>>(formatAdapter: MessageFormatAdapter<TMessage, TStorageFormat>) => GenericThreadHistoryAdapter<TMessage>` — Required when used with \`useAISDKRuntime\` / \`useChatRuntime\`.

  - `attachments?`: `AttachmentAdapter`

    - `accept`: `string`
    - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
    - `remove`: `(attachment: Attachment) => Promise<void>`
    - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

- `children?`: `ReactNode`

### SuggestionByIndexProvider

- `index`: `number`

### TextMessagePartProvider

- `text`: `string`
- `isRunning?`: `boolean`

### ThreadListItemByIndexProvider

- `index`: `number`
- `archived`: `boolean`

### ThreadListItemRuntimeProvider

- `runtime`: `ThreadListItemRuntime`

  - `path`: `ThreadListItemRuntimePath`

    - `ref`: `string`
    - `threadSelector`: `ThreadListItemRuntimePath["threadSelector"]`
      - `type`: `"main"`

  - `getState`: `() => ThreadListItemState`

  - `initialize`: `() => Promise<{ remoteId: string; externalId: string | undefined; }>`

  - `generateTitle`: `() => Promise<void>`

  - `switchTo`: `(options?: { unarchive?: boolean; }) => Promise<void>`

  - `rename`: `(newTitle: string) => Promise<void>`

  - `updateCustom`: `(custom: Record<string, unknown> | undefined) => Promise<void>`

  - `archive`: `() => Promise<void>`

  - `unarchive`: `() => Promise<void>`

  - `delete`: `() => Promise<void>`

  - `detach`: `() => void`

  - `subscribe`: `(callback: () => void) => Unsubscribe`

  - `unstable_on`: `<E extends ThreadListItemEventType>(event: E, callback: ThreadListItemEventCallback<E>) => Unsubscribe`