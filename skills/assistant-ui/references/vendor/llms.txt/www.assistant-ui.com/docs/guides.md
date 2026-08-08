# Guides
URL: /docs/guides

Practical recipes for building AI chat features in React with assistant-ui — attachments, branching, multi-agent, voice, slash commands, generative UI, and more.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Guides are task-oriented walkthroughs for building specific features on top of assistant-ui. They sit between the [Primitives](/docs/primitives) (low-level building blocks) and [Components](/docs/ui/thread) (pre-styled installs) — pick a guide when you know what you want to build and need the recommended pattern.

## Composer

Extend the message input with file attachments, mentions, slash commands, voice, and quoting.

- [Attachments](/docs/guides/attachments) —

  Let users attach files, images, and documents to messages. Covers upload adapters, validation, and the typed `attachmentAddError` event.

- [Mentions](/docs/guides/mentions) —

  `@`-style triggers that insert directives into the composer. Sync and async search patterns.

- [Slash Commands](/docs/guides/slash-commands) —

  `/`-style commands with arguments, async loading, and combining with mentions.

- [Quoting](/docs/guides/quoting) —

  Selection toolbar, programmatic `setQuote`, and forwarding quote context to the LLM.

- [Dictation](/docs/guides/dictation) —

  Speech-to-text into the composer using `DictationAdapter` and `ComposerPrimitive.Dictate`.

## Messages

Customize how messages render, are edited, branched, and suggested.

- [Suggestions](/docs/guides/suggestions) —

  Welcome-screen prompts, dynamic suggestions, and the `AuiIf` empty-thread pattern.

- [Editing](/docs/guides/editing) —

  Edit user messages with `UserEditComposer`, `beginEdit()`, and edit-during-streaming behavior.

- [Branching](/docs/guides/branching) —

  Navigate alternative message versions with `BranchPickerPrimitive`, reload, and direct branch-id navigation.

- [Message Timing](/docs/guides/message-timing) —

  Display response duration and stream timing with `useMessageTiming` (experimental).

## Tools & Generative UI

Tool calling and generative UI have their own [Tools](/docs/tools) section — defining toolkits, rendering tool calls, interactables, MCP, and multi-agent UIs.

- [Tools](/docs/tools) —

  Define toolkits, render tool calls, connect MCP servers, and build generative UI.

## Display

Render specialized message content.

- [Chain of Thought](/docs/guides/chain-of-thought) —

  Display the assistant's reasoning steps in a collapsible accordion.

- [LaTeX](/docs/guides/latex) —

  Render math via React Markdown or Streamdown, with streaming-safe escape rules.

- [Thread Virtualization](/docs/guides/virtualization) —

  Render very long threads with @tanstack/react-virtual and per-index message rendering.

## Audio

Voice input and output.

- [Voice](/docs/guides/voice) —

  Realtime bidirectional audio with `RealtimeVoiceAdapter` and `createVoiceSession`.

- [Speech](/docs/guides/speech) —

  Text-to-speech for assistant messages via `SpeechSynthesisAdapter`.

## Programmatic

Read and drive runtime state from your own code.

- [Context API](/docs/guides/context-api) —

  Access threads, messages, composer, and tool state via `useAui` and the runtime scope tree.

## Providers

Alternative ways to connect a model.

- [ChatGPT Subscription](/docs/guides/chatgpt-subscription) —

  Run your app locally on a ChatGPT Plus or Pro plan via Codex OAuth, no API key required.

## Environments

Run assistant-ui outside a conventional browser deployment.

- [Electron](/docs/guides/electron) —

  Connect an Electron renderer to a hosted backend or a secure, streaming preload/IPC bridge.