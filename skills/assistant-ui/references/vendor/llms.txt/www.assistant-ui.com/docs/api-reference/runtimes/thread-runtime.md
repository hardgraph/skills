# ThreadRuntime
URL: /docs/api-reference/runtimes/thread-runtime

ThreadRuntime state and actions for controlling assistant-ui messages, composers, suggestions, model context, and the full thread lifecycle.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ThreadComposerRuntime

- `path`: `ComposerRuntimePath`

  - `ref`: `string`
  - `threadSelector`: `ThreadComposerRuntime["path"]["threadSelector"]`
    - `type`: `"main"`
  - `composerSource`: `"thread"`

- `type`: `"edit" | "thread"`

- `addAttachment`: `(fileOrAttachment: File | CreateAttachment) => Promise<void>` — Add an attachment to the composer. Accepts either a standard File object (processed through the AttachmentAdapter) or a CreateAttachment descriptor for external-source attachments (URLs, API data, CMS references). External descriptors bypass the adapter's \`add()\` step but still respect \`adapter.accept\` when an adapter is configured; without an adapter they are added as-is.

- `setText`: `(text: string) => void` — Set the text of the composer.

- `setRole`: `(role: MessageRole) => void` — Set the role of the composer. For instance, if you'd like a specific message to have the 'assistant' role, you can do so here.

- `setRunConfig`: `(runConfig: RunConfig) => void` — Set the run config of the composer. This is used to send custom configuration data to the model. Within your backend, you can use the \`runConfig\` object. Example: \`\`\`ts composerRuntime.setRunConfig({ custom: { customField: "customValue" } }); \`\`\`

- `reset`: `() => Promise<void>` — Reset the composer. This will clear the entire state of the composer, including all text and attachments.

- `clearAttachments`: `() => Promise<void>` — Clear all attachments from the composer.

- `send`: `(options?: SendOptions) => void` — Send a message. This will send whatever text or attachments are in the composer.

- `cancel`: `() => void` — Cancel the current run. In edit mode, this will exit edit mode.

- `steerQueueItem`: `(queueItemId: string) => void` (deprecated: Use \`moveQueueItem(queueItemId, { lane: "steer", insertAfter: null })\` instead. Removal after 2026-11-05.)

- `moveQueueItem`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane.

- `removeQueueItem`: `(queueItemId: string) => void` — Remove a queued message.

- `subscribe`: `(callback: () => void) => Unsubscribe` — Listens for changes to the composer state.

- `startDictation`: `() => void` — Start dictation to convert voice to text input. Requires a DictationAdapter to be configured.

- `stopDictation`: `() => void` — Stop the current dictation session.

- `setQuote`: `(quote: QuoteInfo | undefined) => void` — Set a quote for the next message. Pass undefined to clear.

- `unstable_on`: `<E extends ComposerRuntimeEventType>(event: E, callback: ComposerRuntimeEventCallback<E>) => Unsubscribe` (deprecated: This API is still under active development and might change without notice.)

- `getState`: `() => ThreadComposerState`

- `getAttachmentByIndex`: `(idx: number) => AttachmentRuntime & { source: "thread-composer"; }`

### ThreadComposerState

- `canCancel`: `boolean`

- `canSend`: `boolean`

- `isEditing`: `boolean`

- `isEmpty`: `boolean`

- `text`: `string`

- `role`: `MessageRole`

- `attachments`: `readonly Attachment[]`

- `runConfig`: `RunConfig`
  - `custom?`: `Record<string, unknown>`

- `attachmentAccept`: `string`

- `dictation?`: `DictationState` — The current state of dictation. Undefined when dictation is not active.

  - `status`: `DictationAdapter.Status`
    - `type`: `"starting" | "running"`
  - `transcript?`: `string`
  - `inputDisabled?`: `boolean`

- `quote?`: `QuoteInfo` — The currently quoted text, if any. Undefined when no quote is set.

  - `text`: `string`
  - `messageId`: `string`

- `queue`: `readonly QueueItemState[]` — Messages waiting to be processed. Empty unless the \`queue\` capability is set.

- `type`: `"thread"`

### ThreadRuntime

- `path`: `ThreadRuntimePath` — The selector for the thread runtime.

  - `ref`: `string`
  - `threadSelector`: `ThreadRuntimePath["threadSelector"]`
    - `type`: `"main"`

- `composer`: `ThreadComposerRuntime` — The thread composer runtime.

  - `path`: `ComposerRuntimePath`

    - `ref`: `string`
    - `threadSelector`: `ThreadComposerRuntime["path"]["threadSelector"]`
      - `type`: `"main"`
    - `composerSource`: `"thread"`

  - `type`: `"edit" | "thread"`

  - `addAttachment`: `(fileOrAttachment: File | CreateAttachment) => Promise<void>` — Add an attachment to the composer. Accepts either a standard File object (processed through the AttachmentAdapter) or a CreateAttachment descriptor for external-source attachments (URLs, API data, CMS references). External descriptors bypass the adapter's \`add()\` step but still respect \`adapter.accept\` when an adapter is configured; without an adapter they are added as-is.

  - `setText`: `(text: string) => void` — Set the text of the composer.

  - `setRole`: `(role: MessageRole) => void` — Set the role of the composer. For instance, if you'd like a specific message to have the 'assistant' role, you can do so here.

  - `setRunConfig`: `(runConfig: RunConfig) => void` — Set the run config of the composer. This is used to send custom configuration data to the model. Within your backend, you can use the \`runConfig\` object. Example: \`\`\`ts composerRuntime.setRunConfig({ custom: { customField: "customValue" } }); \`\`\`

  - `reset`: `() => Promise<void>` — Reset the composer. This will clear the entire state of the composer, including all text and attachments.

  - `clearAttachments`: `() => Promise<void>` — Clear all attachments from the composer.

  - `send`: `(options?: SendOptions) => void` — Send a message. This will send whatever text or attachments are in the composer.

  - `cancel`: `() => void` — Cancel the current run. In edit mode, this will exit edit mode.

  - `steerQueueItem`: `(queueItemId: string) => void` (deprecated: Use \`moveQueueItem(queueItemId, { lane: "steer", insertAfter: null })\` instead. Removal after 2026-11-05.)

  - `moveQueueItem`: `(queueItemId: string, placement: QueuePlacement) => void` — Move a queued message between lanes or within a lane.

  - `removeQueueItem`: `(queueItemId: string) => void` — Remove a queued message.

  - `subscribe`: `(callback: () => void) => Unsubscribe` — Listens for changes to the composer state.

  - `startDictation`: `() => void` — Start dictation to convert voice to text input. Requires a DictationAdapter to be configured.

  - `stopDictation`: `() => void` — Stop the current dictation session.

  - `setQuote`: `(quote: QuoteInfo | undefined) => void` — Set a quote for the next message. Pass undefined to clear.

  - `unstable_on`: `<E extends ComposerRuntimeEventType>(event: E, callback: ComposerRuntimeEventCallback<E>) => Unsubscribe` (deprecated: This API is still under active development and might change without notice.)

  - `getState`: `() => ThreadComposerState`

  - `getAttachmentByIndex`: `(idx: number) => AttachmentRuntime & { source: "thread-composer"; }`

- `getState`: `() => ThreadState` — Gets a snapshot of the thread state.

- `append`: `(message: CreateAppendMessage) => void` — Append a new message to the thread.

- `deleteMessage`: `(messageId: string) => void | Promise<void>`

- `startRun`: `(config: CreateStartRunConfig) => void` — Start a new run with the given configuration.

- `resumeRun`: `(config: CreateResumeRunConfig) => void` — Resume a run with the given configuration.

- `exportExternalState`: `() => any` — Export the thread state in the external store format. For AI SDK runtimes, this returns the AI SDK message format. For other runtimes, this may return different formats or throw an error.

- `importExternalState`: `(state: any) => void` — Import thread state from the external store format. For AI SDK runtimes, this accepts AI SDK messages. For other runtimes, this may accept different formats or throw an error.

- `subscribe`: `(callback: () => void) => Unsubscribe`

- `cancelRun`: `() => void`

- `getModelContext`: `() => ModelContext`

- `export`: `() => ExportedMessageRepository`

- `import`: `(repository: ExportedMessageRepository) => void`

- `reset`: `(initialMessages?: readonly ThreadMessageLike[]) => void` — Reset the thread with optional initial messages.

- `getMessageByIndex`: `(idx: number) => MessageRuntime`

- `getMessageById`: `(messageId: string) => MessageRuntime`

- `stopSpeaking`: `() => void` (deprecated: This API is still under active development and might change without notice.)

- `connectVoice`: `() => void`

- `disconnectVoice`: `() => void`

- `getVoiceVolume`: `() => number`

- `subscribeVoiceVolume`: `(callback: () => void) => Unsubscribe`

- `muteVoice`: `() => void`

- `unmuteVoice`: `() => void`

- `unstable_on`: `<E extends ThreadRuntimeEventType>(event: E, callback: ThreadRuntimeEventCallback<E>) => Unsubscribe`

### ThreadState

- `threadId`: `string` (deprecated: This field is deprecated and will be removed in 0.12.0. Use \`useThreadListItem().id\` instead.) — The thread ID.

- `metadata`: `ThreadListItemState` (deprecated: Use \`useThreadListItem()\` instead. This field is deprecated and will be removed in 0.12.0.) — The thread metadata.

  - `isMain`: `boolean`
  - `isRunning`: `boolean` — Whether this thread has a run in progress, including a run that continues after the user switches to another thread.
  - `id`: `string`
  - `remoteId?`: `string`
  - `externalId?`: `string`
  - `status`: `ThreadListItemStatus`
  - `title?`: `string`
  - `lastMessageAt?`: `Date`
  - `custom?`: `Record<string, unknown>`

- `isDisabled`: `boolean` — Whether the thread is disabled. Disabled threads cannot receive new messages.

- `isLoading`: `boolean` — Whether the thread is loading its history.

- `isRunning`: `boolean` — Whether the thread is running. A thread is considered running when there is an active stream connection to the backend.

- `capabilities`: `RuntimeCapabilities` — The capabilities of the thread, such as whether the thread supports editing, branch switching, etc.

  - `switchToBranch`: `boolean`
  - `switchBranchDuringRun`: `boolean`
  - `edit`: `boolean`
  - `reload`: `boolean`
  - `refetchThread`: `boolean` — Whether the runtime can refetch this thread's remote state in place.
  - `delete`: `boolean`
  - `cancel`: `boolean`
  - `unstable_copy`: `boolean`
  - `speech`: `boolean`
  - `dictation`: `boolean`
  - `voice`: `boolean`
  - `attachments`: `boolean`
  - `feedback`: `boolean`
  - `queue`: `boolean`

- `messages`: `readonly ThreadMessage[]` — The messages in the currently selected branch of the thread.

- `state`: `ReadonlyJSONValue` (deprecated: This feature is experimental) — The thread state.

- `suggestions`: `readonly ThreadSuggestion[]` — Follow up message suggestions to show the user.

- `extras`: `unknown` — Custom extra information provided by the runtime.

- `speech?`: `SpeechState` (deprecated: This API is still under active development and might change without notice.)

  - `messageId`: `string`
  - `status`: `SpeechSynthesisAdapter.Status`
    - `type`: `"starting" | "running"`

- `voice?`: `VoiceSessionState`

  - `status`: `RealtimeVoiceAdapter.Status`
    - `type`: `"starting" | "running"`
  - `isMuted`: `boolean`
  - `mode`: `RealtimeVoiceAdapter.Mode`

### ThreadViewportState

- `isAtBottom`: `boolean`

- `scrollToBottom`: `(config?: { behavior?: ScrollBehavior | undefined; }) => void`

- `onScrollToBottom`: `( callback: ({ behavior }: { behavior: ScrollBehavior }) => void, ) => Unsubscribe`

- `turnAnchor`: `"top" | "bottom"` — Controls scroll anchoring: "top" anchors user messages at top, "bottom" is classic behavior

- `topAnchorMessageClamp`: `ThreadViewportState["topAnchorMessageClamp"]` — Clamps tall user messages so the assistant response stays in view.

  - `tallerThan`: `string`
  - `visibleHeight`: `string`

- `height`: `ThreadViewportState["height"]` — Raw height values from registered elements

  - `viewport`: `number` — Total viewport height
  - `inset`: `number` — Total content inset height (footer, anchor message, etc.)

- `element`: `ThreadViewportState["element"]` — Current DOM elements used for geometry-based top anchoring

  - `viewport`: `HTMLElement | null`
  - `anchor`: `HTMLElement | null`
  - `target`: `HTMLElement | null`

- `targetConfig`: `ThreadViewportState["targetConfig"]` — Numeric clamp configuration for the active top-anchor target message

  - `tallerThan`: `number`
  - `visibleHeight`: `number`

- `topAnchorTurn`: `ThreadViewportState["topAnchorTurn"]` — The current top-anchor turn activated in this viewport session. History-loaded messages do not populate this; it is set when a run creates a live user/assistant pair and remains after the run completes.

  - `anchorId`: `string`
  - `targetId`: `string`

- `registerViewport`: `() => SizeHandle` — Register a viewport and get a handle to update its height

- `registerContentInset`: `() => SizeHandle` — Register a content inset (footer, anchor message, etc.) and get a handle to update its height

- `registerViewportElement`: `( element: HTMLElement | null, ) => Unsubscribe` — Register the scroll viewport element

- `registerAnchorElement`: `(element: HTMLElement | null) => Unsubscribe` — Register the current anchor user message element

- `registerAnchorTargetElement`: `( element: HTMLElement | null, config?: { readonly tallerThan: number; readonly visibleHeight: number }, ) => Unsubscribe` — Register the current top-anchor target (last assistant response) element along with its numeric clamp configuration. When unregistered, both \`element.target\` and \`targetConfig\` clear together.

- `setTopAnchorTurn`: `( turn: { readonly anchorId: string; readonly targetId: string } | null, ) => void`