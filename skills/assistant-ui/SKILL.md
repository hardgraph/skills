---
name: assistant-ui
description: assistant-ui — React components for building AI chat interfaces. Use when building a chat UI (thread list, message composer, markdown rendering, tool-call parts, streaming), wiring it to a model runtime (Vercel AI SDK, LangChain, custom), using the CLI to scaffold and add components, adapting a shadcn/Radix/Base UI style, or running it on web, React Native, or the terminal.
---

# assistant-ui

assistant-ui is a set of React components and runtimes for building AI chat
interfaces. It gives you the hard parts of a chat UI — a thread of messages,
composer, streaming, markdown, code blocks, tool-call rendering, attachments —
as primitives you compose, styled to match a shadcn / Radix UI / Base UI design.

## Mental model

Three layers:

| Layer                         | What it is                                                                                                                |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Components**                | `<Thread>`, `<Composer>`, message parts, and the registry. Copied into your project and owned by you.                     |
| **Runtime**                   | The adapter that feeds messages to the UI. `useExternalStoreRuntime`, `useChatRuntime`, or an AI SDK / LangChain runtime. |
| **Cloud services** (optional) | Hosted services for assistants, evals, and sync. Not required to run locally.                                             |

The UI is a consumer of a runtime. Swap the runtime to change the model or
transport; the components stay the same.

## Resolving versions

**Resolve the current version from the registry, not memory.**

```bash
npm view @assistant-ui/react version
```

Components are added via the CLI and live in your codebase, so the registry
version is the runtime/primitives package; the components themselves move with
your project.

## Scaffolding

Use the CLI to create a project and add components:

```bash
npx assistant-ui init
npx assistant-ui add thread composer
```

`add` copies the component source into your repo (like shadcn), so you can edit
it directly. Re-running `add` for the same component overwrites your local copy —
review the diff before accepting.

## Runtimes

A runtime implements the message-store protocol the components read. The most
flexible entry point is `useExternalStoreRuntime`, which lets you drive messages
from any source:

```tsx
import { useExternalStoreRuntime, Thread } from "@assistant-ui/react";

const runtime = useExternalStoreRuntime({
  messages: myMessages,
  onNew: async (message) => sendToModel(message),
});

return <Thread runtime={runtime} />;
```

For Vercel AI SDK integrations, use the AI SDK runtime so `useChat`-style
streaming maps directly onto assistant-ui's message parts.

## Message parts

Messages are part-based, mirroring the AI SDK `UIMessage` model. Render by part
type — text, tool-call, image, file — rather than treating a message as one
string. Tool-call parts have states (`input-available`, `output-available`,
`running`) so the UI can show progress.

## Styling

The registry serves components in shadcn, Radix UI, and Base UI flavors. Pick the
flavor that matches your design system; the CLI wires the correct primitives. All
styling is Tailwind-based and editable in place.

## Current vs deprecated

- `.md` is the canonical docs URL suffix; `.mdx` is a backwards-compatible alias
  for agents that request source-style URLs — both return the same markdown.
- Prefer the AI SDK runtime over hand-rolled streaming adapters when integrating
  with `useChat`.
- Use the DevTools to inspect runtime state, context, and events during
  development.

## References

- [Architecture](https://www.assistant-ui.com/docs/architecture.md)
- [Installation](https://www.assistant-ui.com/docs/installation.md)
- [CLI](https://www.assistant-ui.com/docs/cli.md)
- [Documentation index](https://www.assistant-ui.com/docs.md)
- [Radix UI and Base UI](https://www.assistant-ui.com/docs/base-ui.md)
- [DevTools](https://www.assistant-ui.com/docs/devtools.md)
- [npm: @assistant-ui/react](https://www.npmjs.com/package/@assistant-ui/react)
