# @assistant-ui/react-ai-sdk
URL: /docs/api-reference/integrations/react-ai-sdk

Vercel AI SDK runtime hooks, chat transports, and message conversion utilities for assistant-ui React applications.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### AISDKToolkit

- `constructor?`: `(options: AISDKToolkitOptions) => AISDKToolkit`
- `#toolkit?`: `Toolkit`
- `#mcpClients?`: `Map<string, Promise<MCPClient>>`
- `tools?`: `(options: AISDKToolkitToolsOptions = {}) => Promise<ToolSet>`
- `close?`: `() => Promise<void>`
- `#mcpTools?`: `() => Promise<McpToolSet>`
- `#mcpClient?`: `(name: string, config: McpServerConfig, startedAt: number) => Promise<MCPClient>`

### AssistantChatTransport

- `constructor?`: `(initOptions?: AssistantChatTransportInitOptions<UI_MESSAGE>) => AssistantChatTransport`
- `setRuntime?`: `(runtime: AssistantRuntime) => void`
- `getResumableAdapter?`: `() => AssistantChatResumableOptions`
- `__internal_setGetThreadListItem?`: `(getter: () => InitializableThreadListItem) => void`

### createResumableSessionStorage

`sessionStorage`-backed storage for the pending resumable stream id. See the [Resumable Streams](/docs/guides/resumable-streams) guide for end-to-end wiring.

- `options?`: `{ key?: string | (() => string | undefined); }`
  - `key?`: `string | (() => string | undefined)` — Storage key for the pending stream id. A static string namespaces per route or chat surface. A getter is read lazily on every access, so the key can be derived from the active thread's identity; while the getter returns \`undefined\`, reads report no pending stream and writes are dropped, so a thread whose identity is not known yet never touches another thread's key. Under a remote thread list with more than one thread, scope the key per thread and create one storage instance per thread runtime rather than a single shared one. A shared key is written and cleared by whichever thread acts last, so one conversation's stream can resume inside another.

### frontendTools

```
const frontendTools: (tools: FrontendTools) => ToolSet;
```

### getThreadMessageTokenUsage

- `message`: `TokenUsageExtractableMessage`

  - `role?`: `string`
  - `metadata?`: `unknown`

### injectQuoteContext

Injects quote context into messages as markdown blockquotes.

Use this in your route handler before `convertToModelMessages` so the LLM sees the quoted text that the user is referring to.

```
import { convertToModelMessages, streamText } from "ai";
import { injectQuoteContext } from "@assistant-ui/react-ai-sdk";

export async function POST(req: Request) {
  const { messages } = await req.json();
  const result = streamText({
    model: myModel,
    messages: await convertToModelMessages(injectQuoteContext(messages)),
  });
  return result.toUIMessageStreamResponse();
}
```

- `messages`: `UIMessage<unknown, UIDataTypes, UITools>[]`

### RESUMABLE\_STREAM\_ID\_HEADER

Response header used by the [Resumable Streams](/docs/guides/resumable-streams) server and client wiring.

```
const RESUMABLE_STREAM_ID_HEADER: "x-resumable-stream-id";
```

### unstable\_injectInteractableContext

Injects interactable state snapshots into messages as model-visible text.

Mirrors [injectQuoteContext](/docs/api-reference/integrations/react-ai-sdk#injectquotecontext): reads the frozen snapshot stamped on a user message's `metadata.custom.interactables` (by the interactables scope at send time) and prepends a text part. Run this in your route handler before `convertToModelMessages`, which otherwise ignores `metadata.custom`.

Wording is consumer-owned — pass `format` to control how each snapshot reads. A snapshot may originate from a user edit or an agent `update_*` call, so the default phrasing is neutral. Keep the instance id visible in custom wording: the model needs it to address the `update_*` tool's `id` parameter. A custom `format` must also handle entries with `partial: true`, whose `state` carries only the fields that changed since the model's last known state.

```
import { convertToModelMessages, streamText } from "ai";
import { unstable_injectInteractableContext } from "@assistant-ui/react-ai-sdk";

export async function POST(req: Request) {
  const { messages } = await req.json();
  const result = streamText({
    model: myModel,
    messages: await convertToModelMessages(unstable_injectInteractableContext(messages)),
  });
  return result.toUIMessageStreamResponse();
}
```

- `messages`: `UIMessage<unknown, UIDataTypes, UITools>[]`
- `format?`: `(item: Unstable_InteractableSnapshotEntry) => string`

### useAISDKChat

The underlying `useChat` helpers object, for advanced views — reach `resumeStream`, `clearError`, or the raw `sendMessage` without forking the runtime hook. `undefined` when the current thread is not backed by the AI SDK runtime.

```
const useAISDKChat: <UI_MESSAGE extends UIMessage = UIMessage<unknown, UIDataTypes, UITools>>() => UseChatHelpers<UI_MESSAGE>;
```

### useAISDKError

Read the last AI SDK chat error object from the runtime extras. `undefined` when there is no error or the current thread is not backed by the AI SDK runtime.

```
const useAISDKError: () => Error;
```

### useAISDKRuntime

- `chatHelpers`: `UseChatHelpers<UI_MESSAGE>`

  - `id`: `string` — The id of the chat.

  - `setMessages`: `(messages: UI_MESSAGE[] | ((messages: UI_MESSAGE[]) => UI_MESSAGE[])) => void` — Update the \`messages\` state locally. This is useful when you want to edit the messages on the client, and then trigger the \`reload\` method manually to regenerate the AI response.

  - `error?`: `Error`

    - `name`: `string`
    - `message`: `string`
    - `stack?`: `string`
    - `cause?`: `unknown`

  - `status`: `ChatStatus` — Hook status: - \`submitted\`: The message has been sent to the API and we're awaiting the start of the response stream. - \`streaming\`: The response is actively streaming in from the API, receiving chunks of data. - \`ready\`: The full response has been received and processed; a new user message can be submitted. - \`error\`: An error occurred during the API request, preventing successful completion.

  - `addToolResult`: `ChatAddToolOutputFunction<UI_MESSAGE>` (deprecated: Use addToolOutput)

  - `stop`: `() => Promise<void>` — Abort the current request immediately, keep the generated tokens if any.

  - `messages`: `UI_MESSAGE[]`

  - `sendMessage`: `(message?: (CreateUIMessage<UI_MESSAGE> & { text?: never; files?: never; messageId?: string; }) | { text: string; files?: FileList | FileUIPart[]; metadata?: InferUIMessageMetadata<UI_MESSAGE>; parts?: never; messageId?: string; } | { files: FileList | FileUIPart[]; metadata?: InferUIMessageMetadata<UI_MESSAGE>; parts?: never; messageId?: string; }, options?: ChatRequestOptions) => Promise<void>` — Appends or replaces a user message to the chat list. This triggers the API call to fetch the assistant's response. If a messageId is provided, the message will be replaced.

  - `regenerate`: `({ messageId, ...options }?: { messageId?: string; } & ChatRequestOptions) => Promise<void>` — Regenerate the assistant message with the provided message id. If no message id is provided, the last assistant message will be regenerated.

  - `resumeStream`: `(options?: ChatRequestOptions) => Promise<void>` — Attempt to resume an ongoing streaming response.

  - `addToolOutput`: `ChatAddToolOutputFunction<UI_MESSAGE>`

  - `addToolApprovalResponse`: `ChatAddToolApproveResponseFunction`

  - `clearError`: `() => void` — Clear the error state and set the status to ready if the chat is in an error state.

- `adapter?`: `AISDKRuntimeAdapter`

  - `suggestions?`: `readonly ThreadSuggestion[]`

  - `isDisabled?`: `boolean` — Whether the entire thread is disabled. When \`true\`, the composer's input is also disabled (the user cannot type, attach files, or submit). For a narrower gate that keeps the input usable but blocks only sending, use \`isSendDisabled\`.

  - `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` becomes a no-op and the thread composer's \`canSend\` is \`false\`. Use this to gate sending on external React state (e.g. while tool config is loading) without disabling the input itself the way \`isDisabled\` does. Edit composers (saving message edits) intentionally ignore this flag.

  - `unstable_capabilities?`: `AISDKRuntimeAdapter["unstable_capabilities"]`
    - `copy?`: `boolean`

  - `adapters?`: `(NonNullable<ExternalStoreAdapter["adapters"]> & { history?: ThreadHistoryAdapter | undefined; suggestion?: SuggestionAdapter | undefined; })`

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

    - `history?`: `ThreadHistoryAdapter`

      - `load`: `() => Promise<ExportedMessageRepository & { state?: ReadonlyJSONValue; unstable_resume?: boolean; }>`
      - `resume?`: `(options: ChatModelRunOptions) => AsyncGenerator<ChatModelRunResult, void, unknown>`
      - `append`: `(item: ExportedMessageRepositoryItem) => Promise<void>`
      - `update?`: `(item: ExportedMessageRepositoryItem) => Promise<void>` — Rewrites a previously appended message in place, keyed by its message id. Adapters that implement this let a runtime persist a run paused for tool approval and finalize the same message once the run resumes. An update may arrive for an id whose earlier write failed; treat it as an upsert keyed on the message id rather than assuming the entry exists.
      - `delete?`: `(items: ExportedMessageRepositoryItem[]) => Promise<void>`
      - `withFormat?`: `<TMessage, TStorageFormat extends Record<string, unknown>>(formatAdapter: MessageFormatAdapter<TMessage, TStorageFormat>) => GenericThreadHistoryAdapter<TMessage>` — Required when used with \`useAISDKRuntime\` / \`useChatRuntime\`.

    - `suggestion?`: `SuggestionAdapter`
      - `generate`: `( options: SuggestionAdapterGenerateOptions, ) => | Promise<readonly ThreadSuggestion[]> | AsyncGenerator<readonly ThreadSuggestion[], void>`

  - `toCreateMessage?`: `CustomToCreateMessageFunction`

  - `cancelPendingToolCallsOnSend`: `boolean` (default `true`) — Whether to automatically cancel pending interactive tool calls when the user sends a new message. When enabled (default), the pending tool calls will be marked as failed with an error message indicating the user cancelled the tool call by sending a new message.

  - `onResume?`: `ExternalStoreAdapter["onResume"]` — Called when \`runtime.thread.resumeRun(config)\` is invoked. When omitted, \`resumeRun\` throws \`"Runtime does not support resuming runs."\`. Provide this to bridge resume invocations into a custom replay channel (for example, an SSE reconnect endpoint keyed by turn id).

  - `onResumeToolCall?`: `ExternalStoreAdapter["onResumeToolCall"]` — Called when \`runtime.thread.resumeToolCall(options)\` is invoked for a tool call the in-process tracker does not own. When omitted, \`resumeToolCall\` throws \`"Tool call ${toolCallId} is not waiting for resume."\`. Provide this to bridge resume-tool-call invocations into a custom handler.

  - `joinStrategy?`: `JoinStrategy` — How consecutive assistant messages are rendered. \`"concat-content"\` (the default) merges them into a single thread message. \`"none"\` keeps each assistant message as its own thread message, which is useful when a backend persists proactive or consecutive assistant messages as separate entries.

### useChatRuntime

- `options?`: `UseChatRuntimeOptions<UI_MESSAGE>`

  - `id?`: `string` — A unique identifier for the chat. If not provided, a random one will be generated.

  - `messageMetadataSchema?`: `FlexibleSchema<InferUIMessageMetadata<UI_MESSAGE>>`

  - `dataPartSchemas?`: `UIDataTypesToSchemas<InferUIMessageData<UI_MESSAGE>>`

  - `messages?`: `UI_MESSAGE[]`

  - `generateId?`: `IdGenerator` — A way to provide a function that is going to be used for ids for messages and the chat. If not provided the default AI SDK \`generateId\` is used.

  - `transport?`: `ChatTransport<UI_MESSAGE>`

    - `sendMessages`: `(options: { /** The type of message submission - either new message or regeneration */ trigger: 'submit-message' | 'regenerate-message'; /** Unique identifier for the chat session */ chatId: string; /** ID of the message to regenerate, or undefined for new messages */ messageId: string | undefined; /** Array of UI messages representing the conversation history */ messages: UI_MESSAGE[]; /** Signal to abort the request if needed */ abortSignal: AbortSignal | undefined; } & ChatRequestOptions) => Promise<ReadableStream<UIMessageChunk>>` — Sends messages to the chat API endpoint and returns a streaming response. This method handles both new message submission and message regeneration. It supports real-time streaming of responses through UIMessageChunk events.
    - `reconnectToStream`: `(options: { /** Unique identifier for the chat session to reconnect to */ chatId: string; /** Signal to abort the reconnection request if needed */ abortSignal?: AbortSignal; } & ChatRequestOptions) => Promise<ReadableStream<UIMessageChunk> | null>` — Reconnects to an existing streaming response for the specified chat session. This method is used to resume streaming when a connection is interrupted or when resuming a chat session. It's particularly useful for maintaining continuity in long-running conversations or recovering from network issues.

  - `onError?`: `ChatOnErrorCallback` — Callback function to be called when an error is encountered.

  - `onToolCall?`: `ChatOnToolCallCallback<UI_MESSAGE>` — Optional callback function that is invoked when a tool call is received. Intended for automatic client-side tool execution. To add the tool output, call \`addToolOutput\` without awaiting it inside this callback. The callback's return value is not used.

  - `onFinish?`: `ChatOnFinishCallback<UI_MESSAGE>` — Function that is called when the assistant response has finished streaming.

  - `onData?`: `ChatOnDataCallback<UI_MESSAGE>` — Optional callback function that is called when a data part is received.

  - `sendAutomaticallyWhen?`: `(options: { messages: UI_MESSAGE[]; }) => boolean | PromiseLike<boolean>` — When provided, this function will be called when the stream is finished or a tool call is added to determine if the current messages should be resubmitted.

  - `suggestions?`: `readonly ThreadSuggestion[]`

  - `isDisabled?`: `boolean` — Whether the entire thread is disabled. When \`true\`, the composer's input is also disabled (the user cannot type, attach files, or submit). For a narrower gate that keeps the input usable but blocks only sending, use \`isSendDisabled\`.

  - `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` becomes a no-op and the thread composer's \`canSend\` is \`false\`. Use this to gate sending on external React state (e.g. while tool config is loading) without disabling the input itself the way \`isDisabled\` does. Edit composers (saving message edits) intentionally ignore this flag.

  - `unstable_capabilities?`: `UseChatRuntimeOptions["unstable_capabilities"]`
    - `copy?`: `boolean`

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

  - `adapters?`: `AISDKRuntimeAdapter["adapters"]`

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

    - `history?`: `ThreadHistoryAdapter`

      - `load`: `() => Promise<ExportedMessageRepository & { state?: ReadonlyJSONValue; unstable_resume?: boolean; }>`
      - `resume?`: `(options: ChatModelRunOptions) => AsyncGenerator<ChatModelRunResult, void, unknown>`
      - `append`: `(item: ExportedMessageRepositoryItem) => Promise<void>`
      - `update?`: `(item: ExportedMessageRepositoryItem) => Promise<void>` — Rewrites a previously appended message in place, keyed by its message id. Adapters that implement this let a runtime persist a run paused for tool approval and finalize the same message once the run resumes. An update may arrive for an id whose earlier write failed; treat it as an upsert keyed on the message id rather than assuming the entry exists.
      - `delete?`: `(items: ExportedMessageRepositoryItem[]) => Promise<void>`
      - `withFormat?`: `<TMessage, TStorageFormat extends Record<string, unknown>>(formatAdapter: MessageFormatAdapter<TMessage, TStorageFormat>) => GenericThreadHistoryAdapter<TMessage>` — Required when used with \`useAISDKRuntime\` / \`useChatRuntime\`.

    - `suggestion?`: `SuggestionAdapter`
      - `generate`: `( options: SuggestionAdapterGenerateOptions, ) => | Promise<readonly ThreadSuggestion[]> | AsyncGenerator<readonly ThreadSuggestion[], void>`

  - `toCreateMessage?`: `CustomToCreateMessageFunction`

  - `onResume?`: `AISDKRuntimeAdapter["onResume"]`

  - `onResumeToolCall?`: `AISDKRuntimeAdapter["onResumeToolCall"]`

  - `onResumeError?`: `((error: unknown) => void)` — Called when an automatic resumable stream reconnect fails. Use this to surface a toast, report telemetry, or mark the thread as needing a retry. The failed stream id is cleared after the callback unless a newer id has replaced it.

  - `joinStrategy?`: `AISDKRuntimeAdapter["joinStrategy"]`

  - `onThreadIdChange?`: `((threadId: string | undefined) => void)`

### useThreadTokenUsage

```
function useThreadTokenUsage(): ThreadTokenUsage;
```

### generativeTools

> [!warn]
>
> **Deprecated.** Use [AISDKToolkit](/docs/api-reference/integrations/react-ai-sdk#aisdktoolkit) instead: `new AISDKToolkit({ toolkit }).tools({ frontend })`. It is a strict superset (it also opens MCP server connections), so it replaces `generativeTools` everywhere. The `frontendTools` option is named `frontend` on `.tools()`, and `.tools()` is async. `generativeTools` will be removed in a future version.

Builds an AI SDK `ToolSet` for server-side use with `streamText` / `generateText` from a generative `toolkit` and the frontend-uploaded tools.

Each toolkit tool's `execute` runs on the server. Pair this with the `"use generative"` compiler: import the toolkit in a server route (where it resolves to the server build — schema + `execute`, with `render` stripped) and pass it here. Tools without an `execute` are still exposed to the model but left for the client to fulfill. `frontendTools` lets the client contribute tools that aren't in the static toolkit.

```
// Define once at module scope so any MCP connections pool across requests.
const aiToolkit = new AISDKToolkit({ toolkit: docsToolkit });

// In your route handler:
const { tools } = await req.json();
streamText({
  model,
  messages,
  tools: await aiToolkit.tools({ frontend: tools }),
});
```

```
const generativeTools: (options: GenerativeToolsOptions) => ToolSet;
```