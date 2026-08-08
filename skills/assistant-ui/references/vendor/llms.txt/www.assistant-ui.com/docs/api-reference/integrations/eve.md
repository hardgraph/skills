# @assistant-ui/eve
URL: /docs/api-reference/integrations/eve

Eve runtime hook and message conversion utilities for assistant-ui React applications.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### convertEveMessage

Converts a single Eve message into an assistant-ui thread message.

```
const convertEveMessage: (message: EveMessage, index: number, messages: readonly EveMessage[], options?: ConvertEveMessagesOptions) => ThreadMessage;
```

### convertEveMessages

Converts the full Eve message data object into assistant-ui thread messages.

```
const convertEveMessages: (data: EveMessageData, options?: ConvertEveMessagesOptions) => ThreadMessage[];
```

### getEveMessageContent

Converts an assistant-ui append message into the message payload accepted by Eve's `send` API.

```
const getEveMessageContent: (message: AppendMessage) => NonNullable<SendTurnPayload["message"]>;
```

### toEveInputResponse

Converts an assistant-ui tool approval response into an Eve input response.

```
const toEveInputResponse: (response: RespondToToolApprovalOptions) => InputResponse;
```

### useEveAgentRuntime

Connects Eve's `useEveAgent` hook to assistant-ui's runtime contract.

The runtime renders Eve messages, forwards new user messages to the Eve session, supports cancellation, and maps Eve input requests to assistant-ui tool approval UI.

- `options?`: `UseEveAgentRuntimeOptions`

  - `onError?`: `(error: Error) => void`

  - `headers?`: `HeadersValue`

  - `onFinish?`: `(snapshot: EveAgentStoreSnapshot<TData>) => void`

  - `agent?`: `string` — Named agent mounted by a framework integration such as \`withEve({ agents })\`. \`agent: "support"\` targets same-origin routes under \`/eve/agents/support/eve/v1/...\`. Do not combine with \`host\`.

  - `auth?`: `ClientAuth`

    - `basic`: `ClientAuth["basic"]`

      - `username`: `string`
      - `password`: `TokenValue`

    - `bearer`: `TokenValue`

    - `vercelOidc`: `ClientAuth["vercelOidc"]`
      - `token`: `TokenValue`

  - `host`: `string` (default `""`) — Base URL for eve client requests. Do not combine with \`agent\`. Defaults to same-origin eve routes such as \`/eve/v1/...\`. Pass a same-origin prefix such as \`/api\` for an app-owned proxy, or an absolute origin to talk to an eve server directly.

  - `initialEvents?`: `readonly HandleMessageStreamEvent[]`

  - `initialSession?`: `SessionState`

    - `continuationToken?`: `string`
    - `sessionId?`: `string`
    - `streamIndex`: `number`

  - `optimistic`: `boolean` (default `true`) — Project submitted user messages before eve confirms them with a \`message.received\` stream event. Optimistic events are reducer-facing projection events only. They are not exposed through \`events\`, which remains the authoritative eve stream.

  - `session?`: `ClientSession`

    - `#private`: `any`

    - `state`: `SessionState` — Current session cursor. The assigned session ID appears as soon as a send is accepted; the continuation token and stream index advance as its event stream is consumed. Serialize this to persist and resume later.

      - `continuationToken?`: `string`
      - `sessionId?`: `string`
      - `streamIndex`: `number`

    - `send`: `<TOutput = unknown>(input: SendTurnInput<TOutput>) => Promise<MessageResponse<TOutput>>` — Sends one turn payload to the agent. Pass a string as shorthand for \`{ message }\`, or pass an object to submit follow-up text, HITL results, client context, output schema, signal, and headers in a single request.

    - `cancel`: `(options?: { turnId?: string; }) => Promise<CancelSessionResult>` — Requests cooperative cancellation of this session's active turn. Both \`accepted\` and \`no\_active\_turn\` are successful outcomes. The latter means the active turn settled before the request arrived or the session is already parked. \`turnId\` limits the request to the turn the caller observed; a stale guard is consumed as a benign no-op. Credentials are resolved immediately before the request.

    - `reset`: `() => Promise<ResetResult>` — Terminally retires the session that owns this handle's continuation token. Unlike cancel, reset does not merely stop an active turn: it releases the durable workflow owner so the next send creates a fresh conversation and initializes a new session-scoped sandbox on first sandbox use. Resetting a never-started handle is a successful no-op. After a successful reset, this handle has no session state.

    - `stream`: `(options?: StreamOptions) => AsyncIterable<HandleMessageStreamEvent>` — Opens this session's event stream for the current session ID. Resumes from the session's stored stream cursor unless \`options.startIndex\` overrides it. By default, the stream reconnects from its cursor when the connection ends; pass \`streamReconnectPolicy: { reconnect: false }\` to use one connection. Negative indices read relative to the current tail on one connection and do not advance the stored absolute cursor. Pass \`follow: false\` for a bounded read: yields events up to the durable tail observed when the stream opens, then returns instead of following. The stored cursor still advances past the consumed events.

  - `onEvent?`: `(event: HandleMessageStreamEvent) => void`

  - `onSessionChange?`: `(session: SessionState) => void`

  - `prepareSend?`: `PrepareSend`

  - `suggestions?`: `readonly ThreadSuggestion[]`

  - `isDisabled?`: `boolean` — Whether the entire thread is disabled. When \`true\`, the composer's input is also disabled (the user cannot type, attach files, or submit). For a narrower gate that keeps the input usable but blocks only sending, use \`isSendDisabled\`.

  - `isSendDisabled?`: `boolean` — Whether sending new messages is currently disabled. When \`true\`, the thread composer's input remains usable but \`send()\` becomes a no-op and the thread composer's \`canSend\` is \`false\`. Use this to gate sending on external React state (e.g. while tool config is loading) without disabling the input itself the way \`isDisabled\` does. Edit composers (saving message edits) intentionally ignore this flag.

  - `unstable_capabilities?`: `UseEveAgentRuntimeOptions["unstable_capabilities"]`
    - `copy?`: `boolean`

  - `adapters?`: `UseEveAgentRuntimeOptions["adapters"]`

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