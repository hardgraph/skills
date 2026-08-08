# Runtime Adapter Context
URL: /docs/api-reference/adapters/runtime

Provide assistant-ui runtime adapters through React context for model, attachment, speech, and feedback behavior.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### RuntimeAdapters

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