# Thread Component
URL: /docs/ui/thread

Stream-ready React chat container with message list, composer, auto-scroll, and accessibility built in. Drop into any AI chat UI built with assistant-ui.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A complete chat interface that combines message rendering, auto-scrolling, composer input, attachments, and conditional UI states. Fully customizable and composable.

\[interactive preview omitted]

## Anatomy

The `Thread` component is built with the following primitives:

```
import { ThreadPrimitive, AuiIf } from "@assistant-ui/react";

<ThreadPrimitive.Root>
  <ThreadPrimitive.Viewport>
    <AuiIf condition={(s) => s.thread.isEmpty}>
      <ThreadWelcome />
      {/* ThreadWelcome includes ThreadPrimitive.Suggestions */}
    </AuiIf>

    <ThreadPrimitive.Messages>
      {({ message }) => {
        if (message.role === "user") return <UserMessage />;
        return <AssistantMessage />;
      }}
    </ThreadPrimitive.Messages>

    <ThreadPrimitive.ViewportFooter>
      <ThreadPrimitive.ScrollToBottom />
      <Composer />
    </ThreadPrimitive.ViewportFooter>
  </ThreadPrimitive.Viewport>
</ThreadPrimitive.Root>
```

## Getting Started

1. ### Add the component

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/thread
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/thread.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-markdown class-variance-authority remark-gfm tw-shimmer zustand
   ```

   ```bash
   npx shadcn@latest add button dialog tooltip avatar collapsible
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/thread.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/thread.tsx)
   - [components/assistant-ui/attachment.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/attachment.tsx)
   - [components/assistant-ui/tooltip-icon-button.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx)
   - [components/assistant-ui/follow-up-suggestions.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx)
   - [components/assistant-ui/markdown-text.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/markdown-text.tsx)
   - [components/assistant-ui/reasoning.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/reasoning.tsx)
   - [components/assistant-ui/tool-fallback.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx)
   - [components/assistant-ui/tool-group.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tool-group.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/thread.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/thread.tsx \
     -o components/assistant-ui/attachment.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/attachment.tsx \
     -o components/assistant-ui/tooltip-icon-button.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tooltip-icon-button.tsx \
     -o components/assistant-ui/follow-up-suggestions.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx \
     -o components/assistant-ui/markdown-text.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/markdown-text.tsx \
     -o components/assistant-ui/reasoning.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/reasoning.tsx \
     -o components/assistant-ui/tool-fallback.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-fallback.tsx \
     -o components/assistant-ui/tool-group.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tool-group.tsx
   ```

   This adds a `/components/assistant-ui/thread.tsx` file to your project, which you can adjust as needed.

2. ### Use in your application

   ```
   import { Thread } from "@/components/assistant-ui/thread";

   export default function Chat() {
     return (
       <div className="h-full">
         <Thread />
       </div>
     );
   }
   ```

## Examples

### Welcome Screen

```
<AuiIf condition={(s) => s.thread.isEmpty}>
  <ThreadWelcome />
</AuiIf>
```

### Viewport Spacer

```
<AuiIf condition={(s) => !s.thread.isEmpty}>
  <div className="min-h-8 grow" />
</AuiIf>
```

### Conditional Send/Cancel Button

```
<AuiIf condition={(s) => !s.thread.isRunning}>
  <ComposerPrimitive.Send>
    Send
  </ComposerPrimitive.Send>
</AuiIf>

<AuiIf condition={(s) => s.thread.isRunning}>
  <ComposerPrimitive.Cancel>
    Cancel
  </ComposerPrimitive.Cancel>
</AuiIf>
```

### Suggestions

Display suggested prompts using the Suggestions API. See the [Suggestions guide](/docs/guides/suggestions) for detailed configuration.

```
import type { ReactNode } from "react";
import {
  AssistantRuntimeProvider,
  AuiConfig,
  SuggestionPrimitive,
  Suggestions,
  ThreadPrimitive,
} from "@assistant-ui/react";
import { useChatRuntime } from "@assistant-ui/react-ai-sdk";

// Configure suggestions in your runtime provider
const config = AuiConfig({
  suggestions: Suggestions(["What's the weather?", "Tell me a joke"]),
});

const App = ({ children }: { children: ReactNode }) => {
  const runtime = useChatRuntime();
  return (
    <AssistantRuntimeProvider runtime={runtime} config={config}>
      {children}
    </AssistantRuntimeProvider>
  );
};

// Display suggestions in your thread component
const ThreadSuggestions = () => (
  <ThreadPrimitive.Suggestions>
    {() => <SuggestionItem />}
  </ThreadPrimitive.Suggestions>
);

// Custom suggestion item
const SuggestionItem = () => (
  <SuggestionPrimitive.Trigger send asChild>
    <button>
      <SuggestionPrimitive.Title />
    </button>
  </SuggestionPrimitive.Trigger>
);
```

## Component Overrides

`Thread` accepts an optional `components` prop that swaps parts of the rendering without editing the copied file. All slots are optional; omitted slots keep the built-in rendering.

```
import { Thread, type ThreadComponents } from "@/components/assistant-ui/thread";

const THREAD_COMPONENTS: ThreadComponents = {
  ToolFallback: MyToolFallback,
  ToolGroup: MyToolGroup,
};

export default function Chat() {
  return <Thread components={THREAD_COMPONENTS} />;
}
```

- `AssistantMessage?`: `ComponentType` — Replaces the entire assistant message, including the action bar and branch picker.
- `Welcome?`: `ComponentType` — Replaces the welcome screen shown for a new chat.
- `ToolFallback?`: `ToolCallMessagePartComponent` — Renders tool calls that have no tool UI registered by name. Registered tool UIs take precedence over this slot.
- `ToolGroup?`: `ComponentType<PropsWithChildren<{ group: ThreadGroupPart }>>` — Wraps runs of consecutive tool calls. Receives the group part (\`indices\`, \`status\`) and the rendered children.
- `ReasoningGroup?`: `ComponentType<PropsWithChildren<{ group: ThreadGroupPart }>>` — Wraps runs of consecutive reasoning parts. Receives the group part and the rendered children.

Define the `components` object once at module scope (or memoize it) so message subtrees do not re-render whenever the parent re-renders.

For per-tool UI, prefer registering a renderer by tool name over overriding `ToolFallback`: put `render` on the matching toolkit entry (see [Tool UI](/docs/tools/tool-ui)). `data` message parts render through renderers registered with `useAssistantDataUI`; parts without a registered renderer are not displayed.

## API Reference

The following primitives are used within the Thread component and can be customized in your `/components/assistant-ui/thread.tsx` file.

### Root

Contains all parts of the thread.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.
- `className?`: `string` — CSS class name.

This primitive renders a `<div>` element unless `asChild` is set.

### Viewport

The scrollable area containing all messages. Automatically scrolls to the bottom as new messages are added.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper div.

- `autoScroll`: `boolean` (default `true (false when turnAnchor is "top")`) — Whether to automatically scroll to the bottom when new messages are added while the viewport was previously scrolled to the bottom.

- `turnAnchor`: `"top" | "bottom"` (default `"bottom"`) — Controls scroll anchoring behavior for new messages. "top" anchors new user messages at the top of the viewport.

- `topAnchorMessageClamp`: `{ tallerThan?: string; visibleHeight?: string }` (default `{ tallerThan: "10em", visibleHeight: "6em" }`) — Clamps tall user messages when turnAnchor is "top". Messages up to \`tallerThan\` stay fully visible; taller messages show only \`visibleHeight\` of their bottom edge above the assistant response.

  - `tallerThan`: `string` (default `"10em"`) — Clamp messages taller than this CSS length.
  - `visibleHeight`: `string` (default `"6em"`) — Visible portion of a clamped message's bottom edge.

- `scrollToBottomOnRunStart`: `boolean` (default `true`) — Whether to scroll to bottom when a new run starts.

- `scrollToBottomOnInitialize`: `boolean` (default `true`) — Whether to scroll to bottom when thread history is first loaded.

- `scrollToBottomOnThreadSwitch`: `boolean` (default `true`) — Whether to scroll to bottom when switching to a different thread.

- `className?`: `string` — CSS class name.

This primitive renders a `<div>` element unless `asChild` is set.

### Messages

Renders all messages in the thread. This primitive renders a separate component for each message.

```
<ThreadPrimitive.Messages>
  {({ message }) => {
    if (message.role === "user") return <UserMessage />;
    return <AssistantMessage />;
  }}
</ThreadPrimitive.Messages>
```

- `components`: `MessageComponents` — Components to render for different message types.

  - `Message?`: `ComponentType` — Default component for all messages.
  - `UserMessage?`: `ComponentType` — Component for user messages.
  - `EditComposer?`: `ComponentType` — Component for user messages being edited.
  - `AssistantMessage?`: `ComponentType` — Component for assistant messages.
  - `SystemMessage?`: `ComponentType` — Component for system messages.

### MessageByIndex

Renders a single message at the specified index.

```
<ThreadPrimitive.MessageByIndex
  index={0}
  components={{
    UserMessage: UserMessage,
    AssistantMessage: AssistantMessage
  }}
/>
```

- `index`: `number` — The index of the message to render.
- `components?`: `MessageComponents` — Components to render for different message types.

### Empty

Renders children only when there are no messages in the thread.

- `children?`: `ReactNode` — Content to display when the thread is empty.

### ScrollToBottom

A button to scroll the viewport to the bottom. Disabled when the viewport is already at the bottom.

- `asChild`: `boolean` (default `false`) — Merge props with child element instead of rendering a wrapper button.
- `className?`: `string` — CSS class name.

This primitive renders a `<button>` element unless `asChild` is set.

### Suggestions

Renders all configured suggestions. Configure suggestions using the `Suggestions()` API in your runtime provider.

```
<ThreadPrimitive.Suggestions>
  {() => <CustomSuggestionComponent />}
</ThreadPrimitive.Suggestions>
```

- `components?`: `{ Suggestion: ComponentType }` — Custom component to render each suggestion.

> [!info]
>
> See the [Suggestions guide](/docs/guides/suggestions) for detailed information on configuring and customizing suggestions.

### AuiIf

Conditionally renders children based on assistant state. This is a generic component that can access thread, message, composer, and other state.

```
import { AuiIf } from "@assistant-ui/react";

<AuiIf condition={(s) => s.thread.isEmpty}>
  <WelcomeScreen />
</AuiIf>

<AuiIf condition={(s) => s.thread.isRunning}>
  <LoadingIndicator />
</AuiIf>

<AuiIf condition={(s) => s.message.role === "assistant"}>
  <AssistantAvatar />
</AuiIf>
```

- `condition`: `(state: AssistantState) => boolean` — A function that receives the assistant state and returns whether to render children.

> [!info]
>
> The condition function receives an `AssistantState` object with access to `thread`, `message`, `composer`, `part`, and `attachment` state depending on context.

## Related Components

- [ThreadList](/docs/ui/thread-list) - List of threads, with or without sidebar
- [Quoting guide](/docs/guides/quoting) - Quote selected text from messages
- [SelectionToolbarPrimitive](/docs/api-reference/primitives/selection-toolbar) - Floating toolbar API reference