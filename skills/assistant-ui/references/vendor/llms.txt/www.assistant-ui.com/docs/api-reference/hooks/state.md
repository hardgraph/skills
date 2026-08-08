# State Hooks
URL: /docs/api-reference/hooks/state

State selector and action hooks for reading assistant-ui runtime state and controlling threads, composers, messages, and attachments.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### useAui

Returns the current `AssistantClient` from context.

Read the client supplied by the nearest [AuiProvider](/docs/api-reference/context-providers/assistant-runtime-provider#auiprovider) or [AssistantRuntimeProvider](/docs/api-reference/context-providers/assistant-runtime-provider#assistantruntimeprovider), then access a scope on it — `aui.thread`, `aui.composer`, `aui.message`, and so on. Pair with [useAuiState](/docs/api-reference/hooks/state#useauistate) to read reactive state and [useAuiEvent](/docs/api-reference/hooks/state#useauievent) to subscribe to events. The returned client also exposes lower-level methods such as `aui.on(...)` and `aui.subscribe(...)`; prefer `useAuiEvent` for React event subscriptions.

Rendered outside a provider, the returned client's scope accessors throw a descriptive error whenever they are called.

```
import { useAui } from "@assistant-ui/react";

const aui = useAui();

aui.composer().setText("Hello");
aui.composer().send();
aui.thread().cancelRun();
```

```
function useAui(): AssistantClient;
```

### useAuiEvent

Subscribes to an assistant event for the lifetime of the component.

The subscription is established on mount and re-established whenever the scope or event name changes. The `callback` is wrapped in an effect-event shim, so the latest closure is invoked on each emission — you do not need to memoize it.

```
import { useAuiEvent } from "@assistant-ui/react";

useAuiEvent("thread.modelContextUpdate", ({ threadId }) => {
  console.log("Model context updated", threadId);
});
```

- `selector`: `AssistantEventSelector<TEvent>`

  - `toString`: `(() => string) | (() => string)`
  - `valueOf`: `(() => string) | (() => Object)`

- `callback`: `AssistantEventCallback<TEvent>`

### useAuiState

Subscribes to a slice of [AssistantState](/docs/api-reference/primitives/assistant-if#assistantstate) and re-renders the component whenever that slice changes.

The `selector` is called on every store update; its return value is compared by `Object.is`, and the component re-renders only when the selected slice changes. Returning the entire state object is not supported and throws at runtime — select a specific field instead, or compose multiple `useAuiState` calls. Returning a new object or array literal, including spreading `s.thread` into a new object, causes a re-render on every store update; either select primitives or return a memoized reference.

Scopes that may be unavailable can be read via `s.optional.<scope>`, which resolves to `undefined` instead of throwing.

```
import { useAuiState } from "@assistant-ui/react";

const messages = useAuiState((s) => s.thread.messages);
const isRunning = useAuiState((s) => s.thread.isRunning);
const composerText = useAuiState((s) => s.composer.text);
```

- `selector`: `(state: AssistantState) => T`