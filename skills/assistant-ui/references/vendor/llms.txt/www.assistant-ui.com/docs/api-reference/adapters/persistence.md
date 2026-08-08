# Persistence Adapters
URL: /docs/api-reference/adapters/persistence

Persistence adapters for saving assistant-ui message history, remote thread lists, and long-running chat sessions across browser reloads.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ExportedMessageRepository

- `headId?`: `string | null`
- `messages`: `Array<{ message: ThreadMessage; parentId: string | null; runConfig?: RunConfig; }>`

### GenericThreadHistoryAdapter

- `load`: `() => Promise<MessageFormatRepository<TMessage>>`
- `append`: `(item: MessageFormatItem<TMessage>) => Promise<void>`
- `update?`: `(item: MessageFormatItem<TMessage>, localMessageId: string) => Promise<void>`
- `delete?`: `(items: MessageFormatItem<TMessage>[]) => Promise<void>`
- `reportTelemetry?`: `(items: MessageFormatItem<TMessage>[], options?: { durationMs?: number; stepTimestamps?: { start_ms: number; end_ms: number; }[]; }) => void`

### InMemoryThreadListAdapter

- `list?`: `() => Promise<RemoteThreadListResponse>`
- `rename?`: `() => Promise<void>`
- `updateCustom?`: `() => Promise<void>`
- `archive?`: `() => Promise<void>`
- `unarchive?`: `() => Promise<void>`
- `delete?`: `() => Promise<void>`
- `initialize?`: `(threadId: string) => Promise<RemoteThreadInitializeResponse>`
- `generateTitle?`: `() => Promise<AssistantStream>`
- `fetch?`: `(threadId: string) => Promise<RemoteThreadMetadata>`

### MessageFormatAdapter

- `format`: `string`
- `encode`: `(item: MessageFormatItem<TMessage>) => TStorageFormat`
- `decode`: `(stored: MessageStorageEntry<TStorageFormat>) => MessageFormatItem<TMessage>`
- `getId`: `(message: TMessage) => string`

### RemoteThreadListAdapter

```
const runtime = useRemoteThreadListRuntime({
  adapter: myRemoteThreadListAdapter,
  runtimeHook: () => useLocalRuntime(chatModelAdapter),
});
```

- `list`: `(params?: RemoteThreadListPageOptions) => Promise<RemoteThreadListResponse>`
- `rename`: `(remoteId: string, newTitle: string) => Promise<void>`
- `updateCustom?`: `(remoteId: string, custom: Record<string, unknown> | undefined) => Promise<void>`
- `archive`: `(remoteId: string) => Promise<void>`
- `unarchive`: `(remoteId: string) => Promise<void>`
- `delete`: `(remoteId: string) => Promise<void>`
- `initialize`: `(threadId: string) => Promise<RemoteThreadInitializeResponse>`
- `generateTitle`: `(remoteId: string, unstable_messages: readonly ThreadMessage[]) => Promise<AssistantStream>`
- `fetch`: `(threadId: string) => Promise<RemoteThreadMetadata>`
- `unstable_Provider?`: `RemoteThreadListProviderComponent` — Optional React component wrapped around each active thread. Use it to inject per-thread context such as a history or attachments adapter (see \`useCloudThreadListAdapter\` for the canonical shape). The Provider must render \`children\` on its first commit; deferring them behind a loading state, a Suspense boundary, or a \`useEffect\`-gated render is unsupported and leaves thread context unavailable to downstream consumers. Load data inside an always-mounted child instead.

### ThreadHistoryAdapter

```
const runtime = useLocalRuntime(chatModelAdapter, {
  adapters: {
    history: myHistoryAdapter,
  },
});
```

- `load`: `() => Promise<ExportedMessageRepository & { state?: ReadonlyJSONValue; unstable_resume?: boolean; }>`
- `resume?`: `(options: ChatModelRunOptions) => AsyncGenerator<ChatModelRunResult, void, unknown>`
- `append`: `(item: ExportedMessageRepositoryItem) => Promise<void>`
- `update?`: `(item: ExportedMessageRepositoryItem) => Promise<void>` — Rewrites a previously appended message in place, keyed by its message id. Adapters that implement this let a runtime persist a run paused for tool approval and finalize the same message once the run resumes. An update may arrive for an id whose earlier write failed; treat it as an upsert keyed on the message id rather than assuming the entry exists.
- `delete?`: `(items: ExportedMessageRepositoryItem[]) => Promise<void>`
- `withFormat?`: `<TMessage, TStorageFormat extends Record<string, unknown>>(formatAdapter: MessageFormatAdapter<TMessage, TStorageFormat>) => GenericThreadHistoryAdapter<TMessage>` — Required when used with \`useAISDKRuntime\` / \`useChatRuntime\`.