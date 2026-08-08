# Follow-Up Suggestions
URL: /docs/ui/follow-up-suggestions

Render runtime-generated follow-up prompt chips after an assistant response.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

`ThreadFollowupSuggestions` renders the current thread's runtime suggestions as clickable prompt chips. Use it for follow-up prompts that arrive after a response, such as suggested next questions, refinements, or task shortcuts.

> [!info]
>
> This component reads from `thread.suggestions`. For static welcome-screen prompts, use `ThreadPrimitive.Suggestions` instead.

## Getting Started

1. ### Add `follow-up-suggestions`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/follow-up-suggestions
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/follow-up-suggestions.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/follow-up-suggestions.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/follow-up-suggestions.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/follow-up-suggestions.tsx
   ```

   This adds a `/components/assistant-ui/follow-up-suggestions.tsx` file to your project. The default `thread` component already renders `ThreadFollowupSuggestions`, so installing it standalone is only needed when you build your own thread layout.

2. ### Provide runtime suggestions

   Pass suggestions through your runtime. External-store runtimes, AI SDK runtimes, and local runtimes can all surface follow-up prompts through `thread.suggestions`.

   ```
   const runtime = useExternalStoreRuntime({
     messages,
     convertMessage,
     onNew: async (message) => {
       // append the message in your store
     },
     suggestions: [
       { prompt: "Summarize this as action items" },
       { prompt: "Write a shorter version" },
     ],
   });
   ```

3. ### Render after messages

   Place `ThreadFollowupSuggestions` near the bottom of your thread viewport, after the assistant message list and before the composer.

   ```
   import { ThreadFollowupSuggestions } from "@/components/assistant-ui/follow-up-suggestions";

   const ThreadViewportFooter = () => {
     return (
       <ThreadPrimitive.ViewportFooter>
         <ThreadPrimitive.ScrollToBottom />
         <ThreadFollowupSuggestions />
         <Composer />
       </ThreadPrimitive.ViewportFooter>
     );
   };
   ```

## Behavior

The component only renders when the thread is not empty, not currently running, and has at least one suggestion. Each suggestion uses `ThreadPrimitive.Suggestion`, replaces the composer text with the prompt, and sends it immediately.

Suggestions stay on a single line. When they overflow, the row scrolls horizontally without a scrollbar, and each edge with clipped content fades out — so no fade shows at the start edge until you scroll.

## Related Components

- [Thread](/docs/ui/thread) - Complete chat interface with message list and composer
- [Suggestions guide](/docs/guides/suggestions) - Runtime and static suggestion patterns