# AssistantRuntimeProvider
URL: /docs/api-reference/context-providers/assistant-runtime-provider

Root React provider that connects an assistant-ui runtime to primitives, hooks, threads, and composer state.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### AssistantRuntimeProvider

```
import { AssistantRuntimeProvider } from "@assistant-ui/react";
import { useChatRuntime, AssistantChatTransport } from "@assistant-ui/react-ai-sdk";

const MyApp = () => {
  const runtime = useChatRuntime({
    transport: new AssistantChatTransport({
      api: "/api/chat",
    }),
  });

  return (
    <AssistantRuntimeProvider runtime={runtime}>
      {/* your app */}
    </AssistantRuntimeProvider>
  );
};
```

- `runtime`: `AssistantRuntime` — The assistant runtime to expose to descendants. Build one with \`useLocalRuntime\`, \`useExternalStoreRuntime\`, or \`useAssistantTransportRuntime\`.

  - `threads`: `ThreadListRuntime` — The threads in this assistant.

    - `getState`: `() => ThreadListState`

    - `subscribe`: `(callback: () => void) => Unsubscribe`

    - `main`: `ThreadRuntime`

      - `path`: `ThreadRuntimePath` — The selector for the thread runtime.
      - `composer`: `ThreadComposerRuntime` — The thread composer runtime.
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

    - `getById`: `(threadId: string) => ThreadRuntime`

    - `mainItem`: `ThreadListItemRuntime`

      - `path`: `ThreadListItemRuntimePath`
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

    - `getItemById`: `(threadId: string) => ThreadListItemRuntime`

    - `getItemByIndex`: `(idx: number) => ThreadListItemRuntime`

    - `getArchivedItemByIndex`: `(idx: number) => ThreadListItemRuntime`

    - `switchToThread`: `(threadId: string, options?: { unarchive?: boolean; }) => Promise<void>`

    - `switchToNewThread`: `() => Promise<void>`

    - `getLoadThreadsPromise`: `() => Promise<void>`

    - `reload`: `() => Promise<void>`

    - `reloadMainThread`: `() => Promise<void>` — Refetches the open thread's remote state, for state that changed out of band and so never reached the stream. When the runtime declares the in-place capability (\`unstable\_refetchThread\`), composer drafts survive, existing messages stay rendered during the refetch, and the promise settles with the refetch, rejecting if it fails; that runtime also owns what happens to a run in progress, since this does not stop one. Runtimes without the capability have their hook remounted instead, which discards unsent composer input and ends any run, and the promise resolves once the new runtime attaches. A thread that has not been sent yet is left alone.

    - `loadMore`: `() => Promise<void>`

  - `thread`: `ThreadRuntime` — The currently selected main thread. Equivalent to \`threads.main\`.

    - `path`: `ThreadRuntimePath` — The selector for the thread runtime.

      - `ref`: `string`
      - `threadSelector`: `{ readonly type: "main" } | { readonly type: "threadId"; readonly threadId: string; }`

    - `composer`: `ThreadComposerRuntime` — The thread composer runtime.

      - `path`: `ComposerRuntimePath`
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

  - `registerModelContextProvider`: `(provider: ModelContextProvider) => Unsubscribe` — Register a model context provider. Model context providers are configuration such as system message, temperature, etc. that are set in the frontend.

- `aui?`: `AssistantClient` — Optional parent \`AssistantClient\` whose scopes are inherited by the client created for this runtime. Use this when nesting an \`AssistantRuntimeProvider\` inside another assistant context. Omit this prop when there is no parent client.

  - `thread`: `AssistantClientAccessor<"thread">`
  - `message`: `AssistantClientAccessor<"message">`
  - `threads`: `AssistantClientAccessor<"threads">`
  - `threadListItem`: `AssistantClientAccessor<"threadListItem">`
  - `part`: `AssistantClientAccessor<"part">`
  - `composer`: `AssistantClientAccessor<"composer">`
  - `attachment`: `AssistantClientAccessor<"attachment">`
  - `modelContext`: `AssistantClientAccessor<"modelContext">`
  - `suggestions`: `AssistantClientAccessor<"suggestions">`
  - `suggestion`: `AssistantClientAccessor<"suggestion">`
  - `chainOfThought`: `AssistantClientAccessor<"chainOfThought">`
  - `queueItem`: `AssistantClientAccessor<"queueItem">`
  - `tools`: `AssistantClientAccessor<"tools">`
  - `dataRenderers`: `AssistantClientAccessor<"dataRenderers">`
  - `interactables`: `AssistantClientAccessor<"interactables">`
  - `unstable_interactables`: `AssistantClientAccessor<"unstable_interactables">`
  - `subscribe`: `(listener: () => void) => Unsubscribe`
  - `on`: `<TEvent extends AssistantEventName>(selector: AssistantEventSelector<TEvent>, callback: AssistantEventCallback<TEvent>) => Unsubscribe`

- `config?`: `AuiConfig` — Optional extra scopes provided alongside the runtime's \`threads\` scope; build with \`AuiConfig\`.

  - `thread?`: `ClientElement<"thread"> | DerivedElement<"thread">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `message?`: `ClientElement<"message"> | DerivedElement<"message">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `threads?`: `ClientElement<"threads"> | DerivedElement<"threads">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `threadListItem?`: `ClientElement<"threadListItem"> | DerivedElement<"threadListItem">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `part?`: `ClientElement<"part"> | DerivedElement<"part">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `composer?`: `ClientElement<"composer"> | DerivedElement<"composer">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `attachment?`: `ClientElement<"attachment"> | DerivedElement<"attachment">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `modelContext?`: `ClientElement<"modelContext"> | DerivedElement<"modelContext">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `suggestions?`: `ClientElement<"suggestions"> | DerivedElement<"suggestions">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `suggestion?`: `ClientElement<"suggestion"> | DerivedElement<"suggestion">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `chainOfThought?`: `ClientElement<"chainOfThought"> | DerivedElement<"chainOfThought">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `queueItem?`: `ClientElement<"queueItem"> | DerivedElement<"queueItem">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `tools?`: `ClientElement<"tools"> | DerivedElement<"tools">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `dataRenderers?`: `ClientElement<"dataRenderers"> | DerivedElement<"dataRenderers">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `interactables?`: `ClientElement<"interactables"> | DerivedElement<"interactables">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `unstable_interactables?`: `ClientElement<"unstable_interactables"> | DerivedElement<"unstable_interactables">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

### AuiProvider

Supplies an `AssistantClient` to the React tree.

Place near the root of any subtree that uses [useAui](/docs/api-reference/hooks/state#useaui) or the primitives built on it. Components rendered outside an `AuiProvider` receive a default client whose scope accessors throw on use, so missing-provider mistakes surface at the point of use.

`config` is required and must be built with [AuiConfig](/docs/api-reference/utilities/miscellaneous#auiconfig). At the top level, `config` alone creates this subtree's own client. Under a parent provider, `extends` is mandatory: pass `extends={aui}` to extend the parent client or `extends={null}` to isolate from it (enforced with a dev error). Configs are identity-insensitive — a fresh object per render is safe. A config whose scopes are all Derived keeps its scope set fixed at mount (dev-enforced); configs with a root scope, and empty configs, may grow and shrink scopes across renders. `ref` receives the resulting client after mount.

When mounting a runtime built with one of the runtime hooks, use [AssistantRuntimeProvider](/docs/api-reference/context-providers/assistant-runtime-provider#assistantruntimeprovider) — it installs an `AuiProvider` internally — rather than wiring `AuiProvider` yourself.

```
function MessageScope({ index, children }) {
  const aui = useAui();
  const config = AuiConfig({
    message: Derived({
      source: "thread",
      query: { index },
      get: (aui) => aui.thread.message({ index }),
    }),
  });
  return (
    <AuiProvider extends={aui} config={config}>
      {children}
    </AuiProvider>
  );
}
```

- `config`: `AuiConfig` — Scopes to create the client from; built with [AuiConfig](/docs/api-reference/utilities/miscellaneous#auiconfig).

  - `thread?`: `ClientElement<"thread"> | DerivedElement<"thread">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `message?`: `ClientElement<"message"> | DerivedElement<"message">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `threads?`: `ClientElement<"threads"> | DerivedElement<"threads">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `threadListItem?`: `ClientElement<"threadListItem"> | DerivedElement<"threadListItem">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `part?`: `ClientElement<"part"> | DerivedElement<"part">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `composer?`: `ClientElement<"composer"> | DerivedElement<"composer">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `attachment?`: `ClientElement<"attachment"> | DerivedElement<"attachment">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `modelContext?`: `ClientElement<"modelContext"> | DerivedElement<"modelContext">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `suggestions?`: `ClientElement<"suggestions"> | DerivedElement<"suggestions">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `suggestion?`: `ClientElement<"suggestion"> | DerivedElement<"suggestion">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `chainOfThought?`: `ClientElement<"chainOfThought"> | DerivedElement<"chainOfThought">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `queueItem?`: `ClientElement<"queueItem"> | DerivedElement<"queueItem">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `tools?`: `ClientElement<"tools"> | DerivedElement<"tools">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `dataRenderers?`: `ClientElement<"dataRenderers"> | DerivedElement<"dataRenderers">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `interactables?`: `ClientElement<"interactables"> | DerivedElement<"interactables">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

  - `unstable_interactables?`: `ClientElement<"unstable_interactables"> | DerivedElement<"unstable_interactables">`

    - `hook`: `(...args: any[]) => V`
    - `args`: `readonly unknown[]`
    - `key?`: `string | number`
    - `deps?`: `readonly unknown[]`

- `ref?`: `React.Ref<AssistantClient>` — Receives the resulting client after mount.

- `extends?`: `never`

- `value?`: `never`

- `children?`: `React.ReactNode` — Subtree that may read from the client.