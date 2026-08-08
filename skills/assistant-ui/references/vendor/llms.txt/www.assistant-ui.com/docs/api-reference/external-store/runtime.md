# External Store Runtime
URL: /docs/api-reference/external-store/runtime

Runtime components, options, and adapters for using assistant-ui with externally owned chat state.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ExternalStoreAdapter

- `isDisabled?`: `boolean` — Whether the entire thread is disabled. When \`true\`, the composer's input is also disabled (the user cannot type, attach files, or submit). For a narrower gate that keeps the input usable but blocks only sending, use \`isSendDisabled\`.

- `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` becomes a no-op and the thread composer's \`canSend\` is \`false\`. Use this to gate sending on external React state (e.g. while tool config is loading) without disabling the input itself the way \`isDisabled\` does. Edit composers (saving message edits) intentionally ignore this flag.

- `isRunning?`: `boolean` — Whether the thread is running. When provided, this value flows directly to \`thread.isRunning\`, letting the application keep the thread in a running state even after the last assistant message has completed (for example while non-message stream chunks like suggestions or metadata updates are still arriving). When omitted, \`thread.isRunning\` falls back to the last-message-status heuristic.

- `isLoading?`: `boolean`

- `messages?`: `readonly T[]`

- `messageRepository?`: `ExportedMessageRepository`

  - `headId?`: `string | null`
  - `messages`: `Array<{ message: ThreadMessage; parentId: string | null; runConfig?: RunConfig; }>`

- `suggestions?`: `readonly ThreadSuggestion[]`

- `state?`: `ReadonlyJSONValue`

- `extras?`: `unknown`

- `setMessages?`: `((messages: readonly T[]) => void)`

- `unstable_onBranchChange?`: `((event: ExternalStoreBranchChange) => void)` (deprecated: This API is still under active development and might change without notice.) — Fires when the user explicitly switches branches via the runtime's \`switchToBranch\` action (e.g. a BranchPicker click). It does not fire on adapter resync, \`append\`, edit/regenerate, content-only updates, or while the thread is running. Consecutive switches that resolve to the same canonical head are de-duped. \`headId\` is the canonical (persisted) head of the now-visible branch — optimistic/transient ids are never surfaced. \`visibleMessageIds\` lists the visible path in order. This complements \`setMessages\` rather than replacing it: switching still requires \`setMessages\`, and this callback does not on its own enable branch switching.

- `onImport?`: `((messages: readonly ThreadMessage[]) => void)`

- `onExportExternalState?`: `(() => any)`

- `onLoadExternalState?`: `((state: any) => void)`

- `onNew`: `(message: AppendMessage) => Promise<void>`

- `queue?`: `ExternalThreadQueueAdapter` — Opt in to message queuing. Typically produced by \`createMessageQueue\`.

  - `items`: `readonly QueueItemState[]`
  - `steerItems`: `readonly QueueItemState[]`
  - `enqueue`: `(message: AppendMessage) => void` — Send a message into the queue lane, processed in order.
  - `steer`: `(message: AppendMessage) => void` — Send a message into the steer lane, processed next.
  - `move`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane. An unanchored move into the steer lane mid-run interrupts — cancels the live run and dispatches the item — when the runtime supports cancellation; a move with \`insertAfter\`/\`insertBefore\` only places the item and never interrupts.
  - `edit`: `(queueItemId: string, message: AppendMessage) => void`
  - `remove`: `(queueItemId: string) => void`

- `onEdit?`: `((message: AppendMessage) => Promise<void>)`

- `onDelete?`: `((messageId: string) => Promise<void> | void)`

- `onReload?`: `((parentId: string | null, config: StartRunConfig) => Promise<void>)`

- `onResume?`: `((config: ResumeRunConfig) => Promise<void>)`

- `onCancel?`: `(() => Promise<void>)`

- `onRefetchThread?`: `(() => Promise<void>)` — Re-fetches the thread's state from the backing store, in place; a rejection reaches the \`threads.reloadMainThread()\` caller. Unrelated to \`onReload\`, which re-generates an assistant message. The caller does not stop a run in progress first, so an adapter that can stream owns whatever coordination one needs.

- `onAddToolResult?`: `((options: AddToolResultOptions) => Promise<void> | void)`

- `onResumeToolCall?`: `((options: { toolCallId: string; payload: unknown }) => void)`

- `onRespondToToolApproval?`: `((options: RespondToToolApprovalOptions) => Promise<void> | void)`

- `convertMessage?`: `ExternalStoreMessageConverter<T>`

- `adapters?`: `ExternalStoreAdapter["adapters"]`

  - `attachments?`: `AttachmentAdapter`

    - `accept`: `string`
    - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
    - `remove`: `(attachment: Attachment) => Promise<void>`
    - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

  - `speech?`: `SpeechSynthesisAdapter`
    - `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

  - `dictation?`: `DictationAdapter`

    - `listen`: `() => DictationAdapter.Session`
    - `disableInputDuringDictation?`: `boolean`

  - `voice?`: `RealtimeVoiceAdapter`
    - `connect`: `(options: { abortSignal?: AbortSignal; }) => RealtimeVoiceAdapter.Session`

  - `feedback?`: `FeedbackAdapter`
    - `submit`: `(feedback: FeedbackAdapterFeedback) => void`

  - `threadList?`: `ExternalStoreThreadListAdapter` (deprecated: This API is still under active development and might change without notice.)

    - `threadId?`: `string` (deprecated: This API is still under active development and might change without notice.)
    - `isLoading?`: `boolean`
    - `threads?`: `readonly ExternalStoreThreadData<"regular">[]`
    - `archivedThreads?`: `readonly ExternalStoreThreadData<"archived">[]`
    - `onSwitchToNewThread?`: `(() => Promise<void> | void)` (deprecated: This API is still under active development and might change without notice.)
    - `onSwitchToThread?`: `((threadId: string) => Promise<void> | void)` (deprecated: This API is still under active development and might change without notice.)
    - `onRename?`: `( threadId: string, newTitle: string, ) => (Promise<void> | void)`
    - `onUpdateCustom?`: `(( threadId: string, custom: Record<string, unknown> | undefined, ) => Promise<void> | void)`
    - `onArchive?`: `((threadId: string) => Promise<void> | void)`
    - `onUnarchive?`: `((threadId: string) => Promise<void> | void)`
    - `onDelete?`: `((threadId: string) => Promise<void> | void)`

- `unstable_capabilities?`: `ExternalStoreAdapter["unstable_capabilities"]`
  - `copy?`: `boolean`

- `unstable_enableToolInvocations?`: `boolean` — Opt in to the built-in client-side tool-invocations pipeline (\`streamCall\` / \`execute\` / tool-status tracking) for this thread. Defaults to \`false\` — the runtime does \*not\* drive client-side tool callbacks on its own. Set to \`true\` to have the runtime construct a \`ToolInvocationTracker\` and feed every snapshot through it, so tool callbacks fire automatically for tool-call parts in \`messages\`. Opt-in by default because most external-store runtimes either run tools entirely server-side, or already wire their own client-side dispatch path. Enabling the embedded tracker on top of an existing dispatch path would cause tool callbacks to run twice. When enabled, client-side tool results (from \`execute()\` returning, or from \`streamCall\` resolving) flow back through \`adapter.onAddToolResult\` like any other tool result, with \`modelContent\` populated when present.

- `setToolStatuses?`: `((statuses: Record<string, ToolExecutionStatus>) => void)` — Receives the current per-tool-call execution status map whenever it changes. Only invoked when \`unstable\_enableToolInvocations\` is \`true\` — the runtime maintains the map via the embedded tracker. Wire this into local React state and feed it into the converter's \`metadata.toolStatuses\` so the UI can render \`executing\` spinners and human-input prompts.

### ExternalThread

- `0`: `ExternalThreadProps`

  - `messages`: `readonly ExternalThreadMessage[]`

  - `isRunning?`: `boolean`

  - `isLoading?`: `boolean`

  - `state?`: `ReadonlyJSONValue`

  - `extras?`: `unknown`

  - `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` is a no-op and \`composer.canSend\` is \`false\`. Edit composers (saving message edits) intentionally ignore this flag.

  - `onNew?`: `(message: AppendMessage) => void` — Callback for new messages (non-queue runtimes).

  - `onEdit?`: `(message: AppendMessage) => void`

  - `onReload?`: `(parentId: string | null) => void`

  - `onStartRun?`: `() => void`

  - `onCancel?`: `() => void`

  - `onResume?`: `(() => void)`

  - `onAddToolResult?`: `((options: AddToolResultOptions) => void)`

  - `onResumeToolCall?`: `((options: ResumeToolCallOptions) => void)` — Callback for resuming a tool call that is waiting for human input.

  - `onLoadExternalState?`: `((state: unknown) => void)`

  - `attachmentAdapter?`: `AttachmentAdapter`

    - `accept`: `string`
    - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
    - `remove`: `(attachment: Attachment) => Promise<void>`
    - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

  - `feedbackAdapter?`: `FeedbackAdapter`
    - `submit`: `(feedback: FeedbackAdapterFeedback) => void`

  - `speechAdapter?`: `SpeechSynthesisAdapter`
    - `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

  - `queue?`: `ExternalThreadQueueAdapter` — Queue adapter for runtimes that support message queuing and steering.

    - `items`: `readonly QueueItemState[]`
    - `steerItems`: `readonly QueueItemState[]`
    - `enqueue`: `(message: AppendMessage) => void` — Send a message into the queue lane, processed in order.
    - `steer`: `(message: AppendMessage) => void` — Send a message into the steer lane, processed next.
    - `move`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane. An unanchored move into the steer lane mid-run interrupts — cancels the live run and dispatches the item — when the runtime supports cancellation; a move with \`insertAfter\`/\`insertBefore\` only places the item and never interrupts.
    - `edit`: `(queueItemId: string, message: AppendMessage) => void`
    - `remove`: `(queueItemId: string) => void`

  - `branches?`: `ExternalThreadBranchAdapter` — Branch adapter for runtimes that track sibling variants of messages.

    - `getBranches`: `(messageId: string) => readonly string[]` — Returns the sibling branch ids for a message in display order, including the message's own id. Return an empty array for messages without alternative branches.
    - `switchToBranch`: `(branchId: string) => void` — Makes the given branch the visible one. The runtime is expected to swap the \`messages\` array to the selected branch. May be invoked programmatically while a run is in progress; pending queue items are not cleared, and reconciling them with the new branch is the runtime's responsibility.

  - `onRespondToToolApproval?`: `(options: RespondToToolApprovalOptions) => void` — Callback for tool approval decisions. Absent: responding to an approval throws a capability error.

- `length`: `1`

- `toString`: `() => string`

- `toLocaleString`: `{ (): string; (locales: string | string[], options?: Intl.NumberFormatOptions & Intl.DateTimeFormatOptions): string; }`

- `pop`: `() => ExternalThreadProps`

- `push`: `(...items: ExternalThreadProps[]) => number`

- `concat`: `{ (...items: ConcatArray<ExternalThreadProps>[]): ExternalThreadProps[]; (...items: (ExternalThreadProps | ConcatArray<ExternalThreadProps>)[]): ExternalThreadProps[]; }`

- `join`: `(separator?: string) => string`

- `reverse`: `() => ExternalThreadProps[]`

- `shift`: `() => ExternalThreadProps`

- `slice`: `(start?: number, end?: number) => ExternalThreadProps[]`

- `sort`: `(compareFn?: ((a: ExternalThreadProps, b: ExternalThreadProps) => number) | undefined) => [ExternalThreadProps]`

- `splice`: `{ (start: number, deleteCount?: number): ExternalThreadProps[]; (start: number, deleteCount: number, ...items: ExternalThreadProps[]): ExternalThreadProps[]; }`

- `unshift`: `(...items: ExternalThreadProps[]) => number`

- `indexOf`: `(searchElement: ExternalThreadProps, fromIndex?: number) => number`

- `lastIndexOf`: `(searchElement: ExternalThreadProps, fromIndex?: number) => number`

- `every`: `{ <S>(predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => value is S, thisArg?: any): this is S[]; (predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => unknown, thisArg?: any): boolean; }`

- `some`: `(predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => unknown, thisArg?: any) => boolean`

- `forEach`: `(callbackfn: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => void, thisArg?: any) => void`

- `map`: `<U>(callbackfn: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => U, thisArg?: any) => U[]`

- `filter`: `{ <S>(predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => value is S, thisArg?: any): S[]; (predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => unknown, thisArg?: any): ExternalThreadProps[]; }`

- `reduce`: `{ (callbackfn: (previousValue: ExternalThreadProps, currentValue: ExternalThreadProps, currentIndex: number, array: ExternalThreadProps[]) => ExternalThreadProps): ExternalThreadProps; (callbackfn: (previousValue: ExternalThreadProps, currentValue: ExternalThreadProps, currentIndex: number, array: ExternalThreadProps[]) => ExternalThreadProps, initialValue: ExternalThreadProps): ExternalThreadProps; <U>(callbackfn: (previousValue: U, currentValue: ExternalThreadProps, currentIndex: number, array: ExternalThreadProps[]) => U, initialValue: U): U; }`

- `reduceRight`: `{ (callbackfn: (previousValue: ExternalThreadProps, currentValue: ExternalThreadProps, currentIndex: number, array: ExternalThreadProps[]) => ExternalThreadProps): ExternalThreadProps; (callbackfn: (previousValue: ExternalThreadProps, currentValue: ExternalThreadProps, currentIndex: number, array: ExternalThreadProps[]) => ExternalThreadProps, initialValue: ExternalThreadProps): ExternalThreadProps; <U>(callbackfn: (previousValue: U, currentValue: ExternalThreadProps, currentIndex: number, array: ExternalThreadProps[]) => U, initialValue: U): U; }`

- `find`: `{ <S>(predicate: (value: ExternalThreadProps, index: number, obj: ExternalThreadProps[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: ExternalThreadProps, index: number, obj: ExternalThreadProps[]) => unknown, thisArg?: any): ExternalThreadProps | undefined; }`

- `findIndex`: `(predicate: (value: ExternalThreadProps, index: number, obj: ExternalThreadProps[]) => unknown, thisArg?: any) => number`

- `fill`: `(value: ExternalThreadProps, start?: number, end?: number) => [ExternalThreadProps]`

- `copyWithin`: `(target: number, start: number, end?: number) => [ExternalThreadProps]`

- `entries`: `() => ArrayIterator<[number, ExternalThreadProps]>`

- `keys`: `() => ArrayIterator<number>`

- `values`: `() => ArrayIterator<ExternalThreadProps>`

- `includes`: `(searchElement: ExternalThreadProps, fromIndex?: number) => boolean`

- `flatMap`: `<U, This>(callback: (this: This, value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => U | readonly U[], thisArg?: This | undefined) => U[]`

- `flat`: `<A, D>(this: A, depth?: D | undefined) => FlatArray<A, D>[]`

- `at`: `(index: number) => ExternalThreadProps`

- `findLast`: `{ <S>(predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => value is S, thisArg?: any): S | undefined; (predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => unknown, thisArg?: any): ExternalThreadProps | undefined; }`

- `findLastIndex`: `(predicate: (value: ExternalThreadProps, index: number, array: ExternalThreadProps[]) => unknown, thisArg?: any) => number`

- `toReversed`: `() => ExternalThreadProps[]`

- `toSorted`: `(compareFn?: ((a: ExternalThreadProps, b: ExternalThreadProps) => number) | undefined) => ExternalThreadProps[]`

- `toSpliced`: `{ (start: number, deleteCount: number, ...items: ExternalThreadProps[]): ExternalThreadProps[]; (start: number, deleteCount?: number): ExternalThreadProps[]; }`

- `with`: `(index: number, value: ExternalThreadProps) => ExternalThreadProps[]`

### ExternalThreadProps

- `messages`: `readonly ExternalThreadMessage[]`

- `isRunning?`: `boolean`

- `isLoading?`: `boolean`

- `state?`: `ReadonlyJSONValue`

- `extras?`: `unknown`

- `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` is a no-op and \`composer.canSend\` is \`false\`. Edit composers (saving message edits) intentionally ignore this flag.

- `onNew?`: `(message: AppendMessage) => void` — Callback for new messages (non-queue runtimes).

- `onEdit?`: `(message: AppendMessage) => void`

- `onReload?`: `(parentId: string | null) => void`

- `onStartRun?`: `() => void`

- `onCancel?`: `() => void`

- `onResume?`: `(() => void)`

- `onAddToolResult?`: `((options: AddToolResultOptions) => void)`

- `onResumeToolCall?`: `((options: ResumeToolCallOptions) => void)` — Callback for resuming a tool call that is waiting for human input.

- `onLoadExternalState?`: `((state: unknown) => void)`

- `attachmentAdapter?`: `AttachmentAdapter`

  - `accept`: `string`
  - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
  - `remove`: `(attachment: Attachment) => Promise<void>`
  - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

- `feedbackAdapter?`: `FeedbackAdapter`
  - `submit`: `(feedback: FeedbackAdapterFeedback) => void`

- `speechAdapter?`: `SpeechSynthesisAdapter`
  - `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

- `queue?`: `ExternalThreadQueueAdapter` — Queue adapter for runtimes that support message queuing and steering.

  - `items`: `readonly QueueItemState[]`
  - `steerItems`: `readonly QueueItemState[]`
  - `enqueue`: `(message: AppendMessage) => void` — Send a message into the queue lane, processed in order.
  - `steer`: `(message: AppendMessage) => void` — Send a message into the steer lane, processed next.
  - `move`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane. An unanchored move into the steer lane mid-run interrupts — cancels the live run and dispatches the item — when the runtime supports cancellation; a move with \`insertAfter\`/\`insertBefore\` only places the item and never interrupts.
  - `edit`: `(queueItemId: string, message: AppendMessage) => void`
  - `remove`: `(queueItemId: string) => void`

- `branches?`: `ExternalThreadBranchAdapter` — Branch adapter for runtimes that track sibling variants of messages.

  - `getBranches`: `(messageId: string) => readonly string[]` — Returns the sibling branch ids for a message in display order, including the message's own id. Return an empty array for messages without alternative branches.
  - `switchToBranch`: `(branchId: string) => void` — Makes the given branch the visible one. The runtime is expected to swap the \`messages\` array to the selected branch. May be invoked programmatically while a run is in progress; pending queue items are not cleared, and reconciling them with the new branch is the runtime's responsibility.

- `onRespondToToolApproval?`: `(options: RespondToToolApprovalOptions) => void` — Callback for tool approval decisions. Absent: responding to an approval throws a capability error.

### ExternalThreadQueueAdapter

The queue surface a runtime exposes so the composer can stay usable during a run and render the pending messages.

- `items`: `readonly QueueItemState[]`
- `steerItems`: `readonly QueueItemState[]`
- `enqueue`: `(message: AppendMessage) => void` — Send a message into the queue lane, processed in order.
- `steer`: `(message: AppendMessage) => void` — Send a message into the steer lane, processed next.
- `move`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane. An unanchored move into the steer lane mid-run interrupts — cancels the live run and dispatches the item — when the runtime supports cancellation; a move with \`insertAfter\`/\`insertBefore\` only places the item and never interrupts.
- `edit`: `(queueItemId: string, message: AppendMessage) => void`
- `remove`: `(queueItemId: string) => void`

### pickExternalStoreSharedOptions

```
const pickExternalStoreSharedOptions: (options: ExternalStoreSharedOptions) => ExternalStoreSharedOptions;
```

### useExternalStoreRuntime

- `store`: `ExternalStoreAdapter<T>`

  - `isDisabled?`: `boolean` — Whether the entire thread is disabled. When \`true\`, the composer's input is also disabled (the user cannot type, attach files, or submit). For a narrower gate that keeps the input usable but blocks only sending, use \`isSendDisabled\`.

  - `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` becomes a no-op and the thread composer's \`canSend\` is \`false\`. Use this to gate sending on external React state (e.g. while tool config is loading) without disabling the input itself the way \`isDisabled\` does. Edit composers (saving message edits) intentionally ignore this flag.

  - `isRunning?`: `boolean` — Whether the thread is running. When provided, this value flows directly to \`thread.isRunning\`, letting the application keep the thread in a running state even after the last assistant message has completed (for example while non-message stream chunks like suggestions or metadata updates are still arriving). When omitted, \`thread.isRunning\` falls back to the last-message-status heuristic.

  - `isLoading?`: `boolean`

  - `messages?`: `readonly T[]`

  - `messageRepository?`: `ExportedMessageRepository`

    - `headId?`: `string | null`
    - `messages`: `Array<{ message: ThreadMessage; parentId: string | null; runConfig?: RunConfig; }>`

  - `suggestions?`: `readonly ThreadSuggestion[]`

  - `state?`: `ReadonlyJSONValue`

  - `extras?`: `unknown`

  - `setMessages?`: `((messages: readonly T[]) => void)`

  - `unstable_onBranchChange?`: `((event: ExternalStoreBranchChange) => void)` (deprecated: This API is still under active development and might change without notice.) — Fires when the user explicitly switches branches via the runtime's \`switchToBranch\` action (e.g. a BranchPicker click). It does not fire on adapter resync, \`append\`, edit/regenerate, content-only updates, or while the thread is running. Consecutive switches that resolve to the same canonical head are de-duped. \`headId\` is the canonical (persisted) head of the now-visible branch — optimistic/transient ids are never surfaced. \`visibleMessageIds\` lists the visible path in order. This complements \`setMessages\` rather than replacing it: switching still requires \`setMessages\`, and this callback does not on its own enable branch switching.

  - `onImport?`: `((messages: readonly ThreadMessage[]) => void)`

  - `onExportExternalState?`: `(() => any)`

  - `onLoadExternalState?`: `((state: any) => void)`

  - `onNew`: `(message: AppendMessage) => Promise<void>`

  - `queue?`: `ExternalThreadQueueAdapter` — Opt in to message queuing. Typically produced by \`createMessageQueue\`.

    - `items`: `readonly QueueItemState[]`
    - `steerItems`: `readonly QueueItemState[]`
    - `enqueue`: `(message: AppendMessage) => void` — Send a message into the queue lane, processed in order.
    - `steer`: `(message: AppendMessage) => void` — Send a message into the steer lane, processed next.
    - `move`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane. An unanchored move into the steer lane mid-run interrupts — cancels the live run and dispatches the item — when the runtime supports cancellation; a move with \`insertAfter\`/\`insertBefore\` only places the item and never interrupts.
    - `edit`: `(queueItemId: string, message: AppendMessage) => void`
    - `remove`: `(queueItemId: string) => void`

  - `onEdit?`: `((message: AppendMessage) => Promise<void>)`

  - `onDelete?`: `((messageId: string) => Promise<void> | void)`

  - `onReload?`: `((parentId: string | null, config: StartRunConfig) => Promise<void>)`

  - `onResume?`: `((config: ResumeRunConfig) => Promise<void>)`

  - `onCancel?`: `(() => Promise<void>)`

  - `onRefetchThread?`: `(() => Promise<void>)` — Re-fetches the thread's state from the backing store, in place; a rejection reaches the \`threads.reloadMainThread()\` caller. Unrelated to \`onReload\`, which re-generates an assistant message. The caller does not stop a run in progress first, so an adapter that can stream owns whatever coordination one needs.

  - `onAddToolResult?`: `((options: AddToolResultOptions) => Promise<void> | void)`

  - `onResumeToolCall?`: `((options: { toolCallId: string; payload: unknown }) => void)`

  - `onRespondToToolApproval?`: `((options: RespondToToolApprovalOptions) => Promise<void> | void)`

  - `convertMessage?`: `ExternalStoreMessageConverter<T>`

  - `adapters?`: `ExternalStoreAdapter["adapters"]`

    - `attachments?`: `AttachmentAdapter`

      - `accept`: `string`
      - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
      - `remove`: `(attachment: Attachment) => Promise<void>`
      - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

    - `speech?`: `SpeechSynthesisAdapter`
      - `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

    - `dictation?`: `DictationAdapter`

      - `listen`: `() => DictationAdapter.Session`
      - `disableInputDuringDictation?`: `boolean`

    - `voice?`: `RealtimeVoiceAdapter`
      - `connect`: `(options: { abortSignal?: AbortSignal; }) => RealtimeVoiceAdapter.Session`

    - `feedback?`: `FeedbackAdapter`
      - `submit`: `(feedback: FeedbackAdapterFeedback) => void`

    - `threadList?`: `ExternalStoreThreadListAdapter` (deprecated: This API is still under active development and might change without notice.)

      - `threadId?`: `string` (deprecated: This API is still under active development and might change without notice.)
      - `isLoading?`: `boolean`
      - `threads?`: `readonly ExternalStoreThreadData<"regular">[]`
      - `archivedThreads?`: `readonly ExternalStoreThreadData<"archived">[]`
      - `onSwitchToNewThread?`: `(() => Promise<void> | void)` (deprecated: This API is still under active development and might change without notice.)
      - `onSwitchToThread?`: `((threadId: string) => Promise<void> | void)` (deprecated: This API is still under active development and might change without notice.)
      - `onRename?`: `( threadId: string, newTitle: string, ) => (Promise<void> | void)`
      - `onUpdateCustom?`: `(( threadId: string, custom: Record<string, unknown> | undefined, ) => Promise<void> | void)`
      - `onArchive?`: `((threadId: string) => Promise<void> | void)`
      - `onUnarchive?`: `((threadId: string) => Promise<void> | void)`
      - `onDelete?`: `((threadId: string) => Promise<void> | void)`

  - `unstable_capabilities?`: `ExternalStoreAdapter["unstable_capabilities"]`
    - `copy?`: `boolean`

  - `unstable_enableToolInvocations?`: `boolean` — Opt in to the built-in client-side tool-invocations pipeline (\`streamCall\` / \`execute\` / tool-status tracking) for this thread. Defaults to \`false\` — the runtime does \*not\* drive client-side tool callbacks on its own. Set to \`true\` to have the runtime construct a \`ToolInvocationTracker\` and feed every snapshot through it, so tool callbacks fire automatically for tool-call parts in \`messages\`. Opt-in by default because most external-store runtimes either run tools entirely server-side, or already wire their own client-side dispatch path. Enabling the embedded tracker on top of an existing dispatch path would cause tool callbacks to run twice. When enabled, client-side tool results (from \`execute()\` returning, or from \`streamCall\` resolving) flow back through \`adapter.onAddToolResult\` like any other tool result, with \`modelContent\` populated when present.

  - `setToolStatuses?`: `((statuses: Record<string, ToolExecutionStatus>) => void)` — Receives the current per-tool-call execution status map whenever it changes. Only invoked when \`unstable\_enableToolInvocations\` is \`true\` — the runtime maintains the map via the embedded tracker. Wire this into local React state and feed it into the converter's \`metadata.toolStatuses\` so the UI can render \`executing\` spinners and human-input prompts.

### useExternalStoreSharedOptions

- `options`: `ExternalStoreSharedOptions`

  - `suggestions?`: `readonly ThreadSuggestion[]`
  - `isDisabled?`: `boolean` — Whether the entire thread is disabled. When \`true\`, the composer's input is also disabled (the user cannot type, attach files, or submit). For a narrower gate that keeps the input usable but blocks only sending, use \`isSendDisabled\`.
  - `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` becomes a no-op and the thread composer's \`canSend\` is \`false\`. Use this to gate sending on external React state (e.g. while tool config is loading) without disabling the input itself the way \`isDisabled\` does. Edit composers (saving message edits) intentionally ignore this flag.
  - `unstable_capabilities?`: `ExternalStoreSharedOptions["unstable_capabilities"]`
    - `copy?`: `boolean`