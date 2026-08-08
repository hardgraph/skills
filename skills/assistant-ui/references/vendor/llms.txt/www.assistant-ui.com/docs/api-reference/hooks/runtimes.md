# Runtime Hooks
URL: /docs/api-reference/hooks/runtimes

Runtime creation hooks for local, remote, cloud, external-store, and AI SDK powered assistant-ui chat experiences.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### useCloudThreadListRuntime

- `options`: `CloudThreadListAdapter`

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

  - `runtimeHook`: `() => AssistantRuntime`

  - `create?`: `() => Promise<ThreadData>`

  - `delete?`: `(threadId: string) => Promise<void>`

### useLocalRuntime

- `chatModel`: `ChatModelAdapter`
  - `run`: `(options: ChatModelRunOptions) => Promise<ChatModelRunResult> | AsyncGenerator<ChatModelRunResult, void>`

- `options?`: `LocalRuntimeOptions`

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

### useRemoteThreadListRuntime

- `options`: `RemoteThreadListOptions`

  - `runtimeHook`: `() => AssistantRuntime`

  - `adapter`: `RemoteThreadListAdapter`

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

  - `initialThreadId?`: `string` (deprecated: Use \`threadId\` instead, which also reacts to subsequent changes.) — When provided, the runtime starts on this thread instead of creating a new empty thread. Useful for URL-based routing (e.g. \`/chat/\[threadId]\`) where the initial thread is known at mount time.

  - `threadId?`: `string` — The current thread ID to display. When this value changes, the runtime automatically switches to the specified thread. Set to \`undefined\` to switch to a new thread.

  - `onThreadIdChange?`: `((threadId: string | undefined) => void)` — Called whenever the runtime changes the active thread's canonical (remote) ID, so the value can be treated as a managed/controlled variable (e.g. synced to a URL query param). Changes initiated by the controlled \`threadId\` option are not echoed back. Together these options form the controlled pattern: \`threadId\` in, \`onThreadIdChange\` out. Only the settled remote ID is emitted: while a freshly created thread is still optimistic (no remote ID yet) the value is \`undefined\`, and the real ID is emitted once the thread is initialized. The transient local ID is never surfaced.

  - `allowNesting?`: `boolean` — When true, if this runtime is used inside another RemoteThreadListRuntime, it becomes a no-op and simply calls the runtimeHook directly. This allows wrapping runtimes that internally use RemoteThreadListRuntime.