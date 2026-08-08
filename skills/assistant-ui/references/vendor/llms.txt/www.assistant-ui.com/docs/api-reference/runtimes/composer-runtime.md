# ComposerRuntime
URL: /docs/api-reference/runtimes/composer-runtime

ComposerRuntime state and actions for controlling assistant-ui composer text, attachments, submission, cancellation, and pending input.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ComposerRuntime

- `path`: `ComposerRuntimePath`

  - `ref`: `string`
  - `threadSelector`: `ComposerRuntimePath["threadSelector"]`
    - `type`: `"main"`
  - `composerSource`: `"thread"`

- `type`: `"edit" | "thread"`

- `getState`: `() => ComposerState` — Get the current state of the composer. Includes any data that has been added to the composer.

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

- `getAttachmentByIndex`: `(idx: number) => AttachmentRuntime` — Get an attachment by index.

- `startDictation`: `() => void` — Start dictation to convert voice to text input. Requires a DictationAdapter to be configured.

- `stopDictation`: `() => void` — Stop the current dictation session.

- `setQuote`: `(quote: QuoteInfo | undefined) => void` — Set a quote for the next message. Pass undefined to clear.

- `unstable_on`: `<E extends ComposerRuntimeEventType>(event: E, callback: ComposerRuntimeEventCallback<E>) => Unsubscribe` (deprecated: This API is still under active development and might change without notice.)

### ComposerState

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

### EditComposerRuntime

- `path`: `ComposerRuntimePath`

  - `ref`: `string`
  - `threadSelector`: `EditComposerRuntime["path"]["threadSelector"]`
    - `type`: `"main"`
  - `messageSelector`: `EditComposerRuntime["path"]["messageSelector"]`
    - `type`: `"messageId"`
  - `composerSource`: `"edit"`

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

- `getState`: `() => EditComposerState`

- `beginEdit`: `() => void`

- `getAttachmentByIndex`: `(idx: number) => AttachmentRuntime & { source: "edit-composer"; }`

### EditComposerState

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

- `type`: `"edit"`

- `parentId`: `string | null`

- `sourceId`: `string | null`