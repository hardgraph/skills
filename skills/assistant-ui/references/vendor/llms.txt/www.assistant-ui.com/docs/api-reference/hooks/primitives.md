# Primitive Hooks
URL: /docs/api-reference/hooks/primitives

Primitive hooks for reading scoped assistant-ui runtime state, viewport behavior, timing, and message part data inside React components.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### useCloudThreadListAdapter

- `adapter`: `CloudThreadListAdapterOptions`

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

  - `create?`: `(() => Promise<ThreadData>)`

  - `delete?`: `((threadId: string) => Promise<void>)`

### useMessageQuote

Hook that returns the quote info for the current message, if any.

Reads from `message.metadata.custom.quote`.

```
function QuoteBlock() {
  const quote = useMessageQuote();
  if (!quote) return null;
  return <blockquote>{quote.text}</blockquote>;
}
```

```
const useMessageQuote: () => QuoteInfo;
```

### useMessageTiming

Hook that returns timing information for the current assistant message.

Reads from `message.metadata.timing`.

```
function MessageStats() {
  const timing = useMessageTiming();
  if (!timing) return null;
  return <span>{timing.tokensPerSecond?.toFixed(1)} tok/s</span>;
}
```

```
const useMessageTiming: () => MessageTiming;
```

### useRuntimeAdapters

```
type RuntimeAdapters = {
  modelContext?: ModelContextProvider | undefined;
  history?: ThreadHistoryAdapter | undefined;
  attachments?: AttachmentAdapter | undefined;
};

const useRuntimeAdapters: () => RuntimeAdapters | null;
```

### useScrollLock

Locks scroll position during collapsible/height animations and hides scrollbar.

This utility prevents page jumps when content height changes during animations, providing a smooth user experience. It finds the nearest scrollable ancestor and temporarily locks its scroll position while the animation completes.

- Prevents forced reflows: no layout reads, mutations scoped to scrollable parent only
- Reactive: only intercepts scroll events when browser actually adjusts
- Cleans up automatically after animation duration

```
const collapsibleRef = useRef<HTMLDivElement>(null);
const lockScroll = useScrollLock(collapsibleRef, 200);

const handleCollapse = () => {
  lockScroll(); // Lock scroll before collapsing
  setIsOpen(false);
};
```

- `animatedElementRef`: `RefObject<T | null>`
- `animationDuration`: `number`

### useThreadViewport

```
const useThreadViewport: { (): ThreadViewportState; <TSelected>(selector: (state: ThreadViewportState) => TSelected): TSelected; (options: { optional: true; }): ThreadViewportState | null; <TSelected>(options: { optional: true; selector?: (state: ThreadViewportState) => TSelected; }): TSelected | null; };
```

### useThreadViewportAutoScroll

- `options`: `useThreadViewportAutoScroll.Options`

  - `autoScroll?`: `boolean` — Whether to automatically scroll to the bottom when new messages are added. When enabled, the viewport will automatically scroll to show the latest content. Default false if \`turnAnchor\` is "top", otherwise defaults to true.
  - `scrollToBottomOnRunStart?`: `boolean` — Whether to scroll to bottom when a new run starts. Defaults to true.
  - `scrollToBottomOnInitialize?`: `boolean` — Whether to scroll to bottom when messages first appear in the thread. Defaults to true.
  - `scrollToBottomOnThreadSwitch?`: `boolean` — Whether to scroll to bottom when switching to a different thread. Defaults to true.

### useThreadViewportStore

```
const useThreadViewportStore: { (): ReadonlyStore<ThreadViewportState>; (options: { optional: true; }): ReadonlyStore<ThreadViewportState> | null; };
```

### unstable\_useComposerInput

> [!tip]
>
> **Experimental.** Under active development and might change without notice.

Headless bridge to the composer's text value and send action, for building a custom composer input without `ComposerPrimitive.Input`. It is a thin bridge, not a second input: it does not own keyboard behavior, autosize, IME or contentEditable sync, paste/drop attachments, focus management, or rich-text state. Spread `unstable_useTriggerPopoverAriaProps()` onto your element for trigger-popover combobox semantics.

```
const { value, setText, send, isDisabled, canSend } = unstable_useComposerInput();
<textarea
  value={value}
  disabled={isDisabled}
  onChange={(e) => setText(e.target.value)}
  onKeyDown={(e) => {
    if (e.key === "Enter" && !e.shiftKey && canSend) {
      e.preventDefault();
      send();
    }
  }}
/>
```

- `options?`: `Unstable_UseComposerInputOptions`
  - `disabled?`: `boolean` — Disables the input in addition to the composer's own disabled sources (thread disabled, active dictation). When disabled, \`isDisabled\` is \`true\` and \`canSend\` is \`false\`.

### unstable\_useComposerInputHistory

> [!tip]
>
> **Experimental.** Under active development and might change without notice.

Terminal-style input history for the thread composer: ArrowUp on an empty draft recalls previously sent user messages (newest first), ArrowDown steps back toward the newest and finally restores the draft that was being typed when browsing started.

Recall only triggers when the caret is on the first/last line with no selection, so multi-line editing keeps native arrow behavior. The handler yields to an open mention/slash popover, to IME composition, to modifier keys, and to consumer handlers that already called `preventDefault`. It is inert on edit composers.

```
const history = unstable_useComposerInputHistory();
<ComposerPrimitive.Input {...history} />
```

```
function unstable_useComposerInputHistory(): Unstable_ComposerInputHistory;
```

### unstable\_useMessageStallDetection

> [!tip]
>
> **Experimental.** Under active development and might change without notice.

Detects mid-run output stalls on the current message: while the message is running, watches a fingerprint of its content (part count plus text, argument, and result sizes) and reports a stall once the fingerprint stops changing for `thresholdMs`. Useful for re-surfacing a "still working" indicator during tool think-time or provider stalls, after the first tokens have already streamed.

Must be used inside a message scope.

- `options?`: `Unstable_MessageStallDetectionOptions`
  - `thresholdMs`: `number` (default `2000`) — Milliseconds of unchanged message content before the message counts as stalled.

### unstable\_useThreadMessageIds

> [!tip]
>
> **Experimental.** Unstable / Experimental - may change in any release.

Returns the ids of the messages in the current thread, in order.

The returned array keeps a stable identity across content-only updates (e.g. streaming), changing reference only when the id sequence itself changes. Pair with `ThreadPrimitive.Unstable_MessageById` to drive a virtualized or custom message list.

```
const unstable_useThreadMessageIds: () => readonly string[];
```

### unstable\_useTriggerPopoverAriaProps

> [!tip]
>
> **Experimental.** Under active development and might change without notice.

ARIA combobox attributes for the focused element (typically the composer input) describing the open trigger popover, per the WAI-ARIA editable combobox pattern. Returns an empty object outside a `TriggerPopoverRoot` or when no popover is open. Spread these last so they take precedence over any matching ARIA props you set yourself, mirroring `ComposerPrimitive.Input`.

```
const aria = unstable_useTriggerPopoverAriaProps();
<textarea {...aria} />
```

```
function unstable_useTriggerPopoverAriaProps(): Unstable_TriggerPopoverAriaProps;
```

### useMessagePartData

> [!warn]
>
> **Deprecated.** Use [useAuiState](/docs/api-reference/hooks/state#useauistate) to select and narrow `s.part`. Return `null` for optional rendering. Do not throw inside the selector: selectors run inside `useSyncExternalStore`'s `getSnapshot`, so a transient part mismatch during thread switches can unmount the React root.

```
const part = useAuiState((s) =>
  s.part.type === "data" && (!name || s.part.name === name)
    ? s.part
    : null,
);
```

See the [migration guide](https://assistant-ui.com/docs/migrations/v0-12).

- `name?`: `string`

### useMessagePartFile

> [!warn]
>
> **Deprecated.** Use [useAuiState](/docs/api-reference/hooks/state#useauistate) to select and narrow `s.part`. Return `null` for optional rendering. Do not throw inside the selector: selectors run inside `useSyncExternalStore`'s `getSnapshot`, so a transient part mismatch during thread switches can unmount the React root.

```
const file = useAuiState((s) => {
  if (s.part.type !== "file") return null;
  return s.part;
});
```

See the [migration guide](https://assistant-ui.com/docs/migrations/v0-12).

```
const useMessagePartFile: () => FileMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; };
```

### useMessagePartImage

> [!warn]
>
> **Deprecated.** Use [useAuiState](/docs/api-reference/hooks/state#useauistate) to select and narrow `s.part`. Return `null` for optional rendering. Do not throw inside the selector: selectors run inside `useSyncExternalStore`'s `getSnapshot`, so a transient part mismatch during thread switches can unmount the React root.

```
const image = useAuiState((s) => {
  if (s.part.type !== "image") return null;
  return s.part;
});
```

See the [migration guide](https://assistant-ui.com/docs/migrations/v0-12).

```
const useMessagePartImage: () => ImageMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; };
```

### useMessagePartReasoning

> [!warn]
>
> **Deprecated.** Use [useAuiState](/docs/api-reference/hooks/state#useauistate) to select and narrow `s.part`. Return `null` for optional rendering. Do not throw inside the selector: selectors run inside `useSyncExternalStore`'s `getSnapshot`, so a transient part mismatch during thread switches can unmount the React root.

```
const reasoning = useAuiState((s) => {
  if (s.part.type !== "reasoning") return null;
  return s.part;
});
```

See the [migration guide](https://assistant-ui.com/docs/migrations/v0-12).

```
const useMessagePartReasoning: () => ReasoningMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; };
```

### useMessagePartSource

> [!warn]
>
> **Deprecated.** Use [useAuiState](/docs/api-reference/hooks/state#useauistate) to select and narrow `s.part`. Return `null` for optional rendering. Do not throw inside the selector: selectors run inside `useSyncExternalStore`'s `getSnapshot`, so a transient part mismatch during thread switches can unmount the React root.

```
const source = useAuiState((s) => {
  if (s.part.type !== "source") return null;
  return s.part;
});
```

See the [migration guide](https://assistant-ui.com/docs/migrations/v0-12).

```
const useMessagePartSource: () => ({ readonly type: "source"; readonly sourceType: "url"; readonly id: string; readonly url: string; readonly title?: string; readonly providerMetadata?: SourceProviderMetadata; readonly parentId?: string; } & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; }) | ({ readonly type: "source"; readonly sourceType: "document"; readonly id: string; readonly url?: undefined; readonly title: string; readonly mediaType: string; readonly filename?: string; readonly providerMetadata?: SourceProviderMetadata; readonly parentId?: string; } & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; });
```

### useMessagePartText

> [!warn]
>
> **Deprecated.** Use [useAuiState](/docs/api-reference/hooks/state#useauistate) to select and narrow `s.part`. Return `null` for optional rendering. Do not throw inside the selector: selectors run inside `useSyncExternalStore`'s `getSnapshot`, so a transient part mismatch during thread switches can unmount the React root.

```
const text = useAuiState((s) => {
  if (s.part.type !== "text" && s.part.type !== "reasoning") return null;
  return s.part;
});
```

See the [migration guide](https://assistant-ui.com/docs/migrations/v0-12).

```
const useMessagePartText: () => (TextMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; }) | (ReasoningMessagePart & { readonly status: MessagePartStatus | ToolCallMessagePartStatus; });
```