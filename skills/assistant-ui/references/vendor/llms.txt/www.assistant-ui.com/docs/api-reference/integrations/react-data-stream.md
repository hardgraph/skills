# @assistant-ui/react-data-stream
URL: /docs/api-reference/integrations/react-data-stream

Data stream runtime hook and message conversion utilities for custom assistant-ui streaming backends.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### useDataStreamRuntime

- `options`: `UseDataStreamRuntimeOptions`

  - `api`: `string`

  - `protocol?`: `DataStreamProtocol` — Defaults to response-header detection, then "ui-message-stream".

  - `onData?`: `(data: { type: string; name: string; data: unknown; transient?: boolean; }) => void` — Callback for data-\* parts (ui-message-stream only).

  - `onResponse?`: `(response: Response) => void | Promise<void>`

  - `onFinish?`: `(message: ThreadMessage) => void`

  - `onError?`: `(error: Error) => void`

  - `onCancel?`: `() => void`

  - `credentials?`: `RequestCredentials`

  - `headers?`: `HeadersValue | (() => Promise<HeadersValue>)`

    - `append`: `(name: string, value: string) => void` — The \*\*\`append()\`\*\* method of the Headers interface appends a new value onto an existing header inside a Headers object, or adds the header if it does not already exist. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/append)
    - `delete`: `(name: string) => void` — The \*\*\`delete()\`\*\* method of the Headers interface deletes a header from the current Headers object. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/delete)
    - `get`: `(name: string) => string | null` — The \*\*\`get()\`\*\* method of the Headers interface returns a byte string of all the values of a header within a Headers object with a given name. If the requested header doesn't exist in the Headers object, it returns null. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/get)
    - `getSetCookie`: `() => string[]` — The \*\*\`getSetCookie()\`\*\* method of the Headers interface returns an array containing the values of all Set-Cookie headers associated with a response. This allows Headers objects to handle having multiple Set-Cookie headers, which wasn't possible prior to its implementation. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/getSetCookie)
    - `has`: `(name: string) => boolean` — The \*\*\`has()\`\*\* method of the Headers interface returns a boolean stating whether a Headers object contains a certain header. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/has)
    - `set`: `(name: string, value: string) => void` — The \*\*\`set()\`\*\* method of the Headers interface sets a new value for an existing header inside a Headers object, or adds the header if it does not already exist. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/set)
    - `forEach`: `(callbackfn: (value: string, key: string, parent: Headers) => void, thisArg?: any) => void`
    - `entries`: `() => HeadersIterator<[string, string]>` — Returns an iterator allowing to go through all key/value pairs contained in this object.
    - `keys`: `() => HeadersIterator<string>` — Returns an iterator allowing to go through all keys of the key/value pairs contained in this object.
    - `values`: `() => HeadersIterator<string>` — Returns an iterator allowing to go through all values of the key/value pairs contained in this object.

  - `body?`: `object | (() => Promise<object | undefined>)`

  - `sendExtraMessageFields?`: `boolean`

  - `maxSteps?`: `number`

  - `unstable_humanToolNames?`: `string[]` — Names of tools that pause the run until a result is supplied via \`addToolResult\`.

  - `unstable_enableMessageQueue?`: `boolean` — Opt in to message queuing: a message sent during a run is held in \`composer.queue\` and sent once the run settles. Steering runs it next.

  - `unstable_queueClearOnRewind?`: `boolean` (deprecated: Removal after 2026-11-05 — the queue will always survive rewinds.) — Auto-clear the message queue when the thread rewinds (message edit). Defaults to \`true\`.

  - `unstable_queueClearOnCancel?`: `boolean` (deprecated: Removal after 2026-11-05 — cancel will always pause the queue and keep the items.) — Auto-clear the message queue when the user cancels the run. Defaults to \`true\`. When \`false\`, cancel pauses the queue and keeps the pending items; the next send drains them.

  - `cloud?`: `AssistantCloud`

    - `threads`: `AssistantCloudThreads`

      - `messages`: `AssistantCloudThreadMessages`
      - `cloud`: `AssistantCloudAPI`
      - `list`: `(query?: AssistantCloudThreadsListQuery) => Promise<AssistantCloudThreadsListResponse>`
      - `get`: `(threadId: string) => Promise<CloudThread>`
      - `create`: `(body: AssistantCloudThreadsCreateBody) => Promise<AssistantCloudThreadsCreateResponse>`
      - `update`: `(threadId: string, body: AssistantCloudThreadsUpdateBody) => Promise<void>`
      - `delete`: `(threadId: string) => Promise<void>`

    - `projects`: `AssistantCloudProjects`
      - `threads`: `AssistantCloudProjectThreads`

    - `auth`: `__object`
      - `tokens`: `AssistantCloudAuthTokens`

    - `runs`: `AssistantCloudRuns`

      - `cloud`: `AssistantCloudAPI`
      - `stream`: `(body: AssistantCloudRunsStreamBody) => Promise<AssistantStream>`
      - `report`: `(body: AssistantCloudRunReport) => Promise<{ run_id: string; }>`

    - `files`: `AssistantCloudFiles`

      - `cloud`: `AssistantCloudAPI`
      - `pdfToImages`: `(body: PdfToImagesRequestBody) => Promise<PdfToImagesResponse>`
      - `generatePresignedUploadUrl`: `(body: GeneratePresignedUploadUrlRequestBody) => Promise<GeneratePresignedUploadUrlResponse>`

    - `telemetry`: `AssistantCloudTelemetryConfig`

      - `enabled?`: `boolean`
      - `beforeReport?`: `( report: AssistantCloudRunReport, ) => AssistantCloudRunReport | null` — Called before each telemetry report is sent. Return a modified report to enrich it (e.g. add \`model\_id\`), or return \`null\` to skip the report.

  - `initialMessages?`: `readonly ThreadMessageLike[]`

  - `adapters?`: `Omit<LocalRuntimeOptionsBase["adapters"], "chatModel">`

    - `attachments?`: `AttachmentAdapter`

      - `accept`: `string`
      - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
      - `remove`: `(attachment: Attachment) => Promise<void>`
      - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

    - `suggestion?`: `SuggestionAdapter`
      - `generate`: `( options: SuggestionAdapterGenerateOptions, ) => | Promise<readonly ThreadSuggestion[]> | AsyncGenerator<readonly ThreadSuggestion[], void>`

    - `dictation?`: `DictationAdapter`

      - `listen`: `() => DictationAdapter.Session`
      - `disableInputDuringDictation?`: `boolean`

    - `speech?`: `SpeechSynthesisAdapter`
      - `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

    - `voice?`: `RealtimeVoiceAdapter`
      - `connect`: `(options: { abortSignal?: AbortSignal; }) => RealtimeVoiceAdapter.Session`

    - `feedback?`: `FeedbackAdapter`
      - `submit`: `(feedback: FeedbackAdapterFeedback) => void`

    - `history?`: `ThreadHistoryAdapter`

      - `load`: `() => Promise<ExportedMessageRepository & { state?: ReadonlyJSONValue; unstable_resume?: boolean; }>`
      - `resume?`: `(options: ChatModelRunOptions) => AsyncGenerator<ChatModelRunResult, void, unknown>`
      - `append`: `(item: ExportedMessageRepositoryItem) => Promise<void>`
      - `update?`: `(item: ExportedMessageRepositoryItem) => Promise<void>` — Rewrites a previously appended message in place, keyed by its message id. Adapters that implement this let a runtime persist a run paused for tool approval and finalize the same message once the run resumes. An update may arrive for an id whose earlier write failed; treat it as an upsert keyed on the message id rather than assuming the entry exists.
      - `delete?`: `(items: ExportedMessageRepositoryItem[]) => Promise<void>`
      - `withFormat?`: `<TMessage, TStorageFormat extends Record<string, unknown>>(formatAdapter: MessageFormatAdapter<TMessage, TStorageFormat>) => GenericThreadHistoryAdapter<TMessage>` — Required when used with \`useAISDKRuntime\` / \`useChatRuntime\`.

### toLanguageModelMessages

> [!warn]
>
> **Deprecated.** Use `toGenericMessages` from `assistant-stream` for framework-agnostic conversion. This function is kept for AI SDK compatibility.

- `messages`: `readonly ThreadMessage[]`
- `options?`: `{ unstable_includeId?: boolean | undefined; }`
  - `unstable_includeId?`: `boolean`

### useCloudRuntime

> [!warn]
>
> **Deprecated.** This is under active development and not yet ready for prod use.

- `options`: `UseCloudRuntimeOptions`

  - `body?`: `object | (() => Promise<object | undefined>)`

  - `onError?`: `(error: Error) => void`

  - `onCancel?`: `() => void`

  - `adapters?`: `Omit<LocalRuntimeOptionsBase["adapters"], "chatModel">`

    - `attachments?`: `AttachmentAdapter`

      - `accept`: `string`
      - `add`: `(state: { file: File; }) => Promise<PendingAttachment> | AsyncGenerator<PendingAttachment, void>`
      - `remove`: `(attachment: Attachment) => Promise<void>`
      - `send`: `(attachment: PendingAttachment) => Promise<CompleteAttachment>`

    - `suggestion?`: `SuggestionAdapter`
      - `generate`: `( options: SuggestionAdapterGenerateOptions, ) => | Promise<readonly ThreadSuggestion[]> | AsyncGenerator<readonly ThreadSuggestion[], void>`

    - `dictation?`: `DictationAdapter`

      - `listen`: `() => DictationAdapter.Session`
      - `disableInputDuringDictation?`: `boolean`

    - `speech?`: `SpeechSynthesisAdapter`
      - `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

    - `voice?`: `RealtimeVoiceAdapter`
      - `connect`: `(options: { abortSignal?: AbortSignal; }) => RealtimeVoiceAdapter.Session`

    - `feedback?`: `FeedbackAdapter`
      - `submit`: `(feedback: FeedbackAdapterFeedback) => void`

    - `history?`: `ThreadHistoryAdapter`

      - `load`: `() => Promise<ExportedMessageRepository & { state?: ReadonlyJSONValue; unstable_resume?: boolean; }>`
      - `resume?`: `(options: ChatModelRunOptions) => AsyncGenerator<ChatModelRunResult, void, unknown>`
      - `append`: `(item: ExportedMessageRepositoryItem) => Promise<void>`
      - `update?`: `(item: ExportedMessageRepositoryItem) => Promise<void>` — Rewrites a previously appended message in place, keyed by its message id. Adapters that implement this let a runtime persist a run paused for tool approval and finalize the same message once the run resumes. An update may arrive for an id whose earlier write failed; treat it as an upsert keyed on the message id rather than assuming the entry exists.
      - `delete?`: `(items: ExportedMessageRepositoryItem[]) => Promise<void>`
      - `withFormat?`: `<TMessage, TStorageFormat extends Record<string, unknown>>(formatAdapter: MessageFormatAdapter<TMessage, TStorageFormat>) => GenericThreadHistoryAdapter<TMessage>` — Required when used with \`useAISDKRuntime\` / \`useChatRuntime\`.

  - `maxSteps?`: `number`

  - `unstable_humanToolNames?`: `string[]` — Names of tools that pause the run until a result is supplied via \`addToolResult\`.

  - `unstable_enableMessageQueue?`: `boolean` — Opt in to message queuing: a message sent during a run is held in \`composer.queue\` and sent once the run settles. Steering runs it next.

  - `unstable_queueClearOnRewind?`: `boolean` (deprecated: Removal after 2026-11-05 — the queue will always survive rewinds.) — Auto-clear the message queue when the thread rewinds (message edit). Defaults to \`true\`.

  - `unstable_queueClearOnCancel?`: `boolean` (deprecated: Removal after 2026-11-05 — cancel will always pause the queue and keep the items.) — Auto-clear the message queue when the user cancels the run. Defaults to \`true\`. When \`false\`, cancel pauses the queue and keeps the pending items; the next send drains them.

  - `headers?`: `HeadersValue | (() => Promise<HeadersValue>)`

    - `append`: `(name: string, value: string) => void` — The \*\*\`append()\`\*\* method of the Headers interface appends a new value onto an existing header inside a Headers object, or adds the header if it does not already exist. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/append)
    - `delete`: `(name: string) => void` — The \*\*\`delete()\`\*\* method of the Headers interface deletes a header from the current Headers object. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/delete)
    - `get`: `(name: string) => string | null` — The \*\*\`get()\`\*\* method of the Headers interface returns a byte string of all the values of a header within a Headers object with a given name. If the requested header doesn't exist in the Headers object, it returns null. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/get)
    - `getSetCookie`: `() => string[]` — The \*\*\`getSetCookie()\`\*\* method of the Headers interface returns an array containing the values of all Set-Cookie headers associated with a response. This allows Headers objects to handle having multiple Set-Cookie headers, which wasn't possible prior to its implementation. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/getSetCookie)
    - `has`: `(name: string) => boolean` — The \*\*\`has()\`\*\* method of the Headers interface returns a boolean stating whether a Headers object contains a certain header. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/has)
    - `set`: `(name: string, value: string) => void` — The \*\*\`set()\`\*\* method of the Headers interface sets a new value for an existing header inside a Headers object, or adds the header if it does not already exist. [MDN Reference](https://developer.mozilla.org/docs/Web/API/Headers/set)
    - `forEach`: `(callbackfn: (value: string, key: string, parent: Headers) => void, thisArg?: any) => void`
    - `entries`: `() => HeadersIterator<[string, string]>` — Returns an iterator allowing to go through all key/value pairs contained in this object.
    - `keys`: `() => HeadersIterator<string>` — Returns an iterator allowing to go through all keys of the key/value pairs contained in this object.
    - `values`: `() => HeadersIterator<string>` — Returns an iterator allowing to go through all values of the key/value pairs contained in this object.

  - `cloud`: `AssistantCloud`

    - `threads`: `AssistantCloudThreads`

      - `messages`: `AssistantCloudThreadMessages`
      - `cloud`: `AssistantCloudAPI`
      - `list`: `(query?: AssistantCloudThreadsListQuery) => Promise<AssistantCloudThreadsListResponse>`
      - `get`: `(threadId: string) => Promise<CloudThread>`
      - `create`: `(body: AssistantCloudThreadsCreateBody) => Promise<AssistantCloudThreadsCreateResponse>`
      - `update`: `(threadId: string, body: AssistantCloudThreadsUpdateBody) => Promise<void>`
      - `delete`: `(threadId: string) => Promise<void>`

    - `projects`: `AssistantCloudProjects`
      - `threads`: `AssistantCloudProjectThreads`

    - `auth`: `__object`
      - `tokens`: `AssistantCloudAuthTokens`

    - `runs`: `AssistantCloudRuns`

      - `cloud`: `AssistantCloudAPI`
      - `stream`: `(body: AssistantCloudRunsStreamBody) => Promise<AssistantStream>`
      - `report`: `(body: AssistantCloudRunReport) => Promise<{ run_id: string; }>`

    - `files`: `AssistantCloudFiles`

      - `cloud`: `AssistantCloudAPI`
      - `pdfToImages`: `(body: PdfToImagesRequestBody) => Promise<PdfToImagesResponse>`
      - `generatePresignedUploadUrl`: `(body: GeneratePresignedUploadUrlRequestBody) => Promise<GeneratePresignedUploadUrlResponse>`

    - `telemetry`: `AssistantCloudTelemetryConfig`

      - `enabled?`: `boolean`
      - `beforeReport?`: `( report: AssistantCloudRunReport, ) => AssistantCloudRunReport | null` — Called before each telemetry report is sent. Return a modified report to enrich it (e.g. add \`model\_id\`), or return \`null\` to skip the report.

  - `initialMessages?`: `readonly ThreadMessageLike[]`

  - `onFinish?`: `(message: ThreadMessage) => void`

  - `onData?`: `(data: { type: string; name: string; data: unknown; transient?: boolean; }) => void` — Callback for data-\* parts (ui-message-stream only).

  - `protocol?`: `DataStreamProtocol` — Defaults to response-header detection, then "ui-message-stream".

  - `onResponse?`: `(response: Response) => void | Promise<void>`

  - `credentials?`: `RequestCredentials`

  - `sendExtraMessageFields?`: `boolean`

  - `assistantId`: `string`