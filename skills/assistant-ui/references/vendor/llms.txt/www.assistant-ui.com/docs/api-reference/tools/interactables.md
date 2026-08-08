# Interactables
URL: /docs/api-reference/tools/interactables

Unstable interactables APIs for model-editable app and message state, including hooks, resources, toolkit helpers, and snapshot utilities.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### unstable\_Interactables

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Registers the unstable interactables store scope.

```
const unstable_Interactables: Resource<ClientOutput<"unstable_interactables">, [(Unstable_InteractablesConfig | undefined)?]>;
```

### unstable\_useInteractable

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Registers an interactable with the AI assistant and returns its live state, like `useState` that the model can also read and update.

Call this once per place that shows the interactable. Other components can read and write the same instance by passing its `id` to `unstable_useInteractableState`.

For tool-created interactables rendered inside tool-call message parts, `version` carries this message's version of the instance — its state as of that point in the conversation, whether it is the most recent tool-driven version, and a `restore()` back to it. Whether older messages render frozen history or stay live-editable is the component's choice. Inside an `update_{name}` part the instance `id` is inferred from the call, so the same component works at the creating call and at update calls.

- `name`: `string`

- `config`: `Unstable_InteractableConfig<TSchema>`

  - `description`: `string`
  - `stateSchema`: `TSchema`
  - `initialState`: `Unstable_InferInteractableState<TSchema>`
  - `id?`: `string` — Unique instance ID; required to address this instance when multiple interactables share a name. Auto-generated if omitted.
  - `updateRender?`: `ToolCallMessagePartComponent` — Component installed as the tool UI for this interactable's \`update\_{name}\` tool calls, so a model edit re-renders the interactable at the message that made it instead of only mutating an earlier one. Prefer \`unstable\_interactableTool\`, which wires this up. Pass a stable component reference; changing identity re-registers the tool UI.

### unstable\_useInteractableState

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Reads and writes the state of an interactable registered elsewhere, by id.

Use this from secondary readers (children, siblings); the owning component registers with `unstable_useInteractable`, which returns state directly. Returns `undefined` until the owning interactable is registered.

- `id`: `string`

### unstable\_useInteractableVersions

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Every version of a tool-created interactable recorded in the current thread, oldest first: the creating tool call, each user edit, and each `update_*` call. Each entry carries the full state as of that version and a `restore()` that sets the live instance back to it — enough for a version picker like an artifact's history dropdown.

- `id`: `string`
- `name`: `string`

### unstable\_interactableTool

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Defines a tool that creates a thread-scoped interactable from its arguments, for assignment to a toolkit entry — the entry key is the interactable name:

```
notepad: unstable_interactableTool({
  description: "A notepad the user can read and edit.",
  stateSchema: notepadSchema,
  render: (props) => <Notepad {...props} />,
}),
```

`render` shows at the creating call and — installed automatically — at every `update_{name}` call, with streaming previews, instance-id wiring, and registration handled. It receives the live `state`/`setState` plus this message's `version`; whether older messages render frozen history or stay live-editable is the render function's choice.

```
const unstable_interactableTool: <TSchema extends Unstable_InteractableStateSchema>(config: Unstable_InteractableToolConfig<TSchema>) => ToolDefinition<Record<string, unknown>, { success: true; }>;
```

### unstable\_getInteractableSnapshots

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Reads the interactable snapshots stamped on a message's `metadata.custom.interactables`, or `undefined` if none. This is the read half of the snapshot channel — integrations use it to surface interactable state to the model (see `unstable_injectInteractableContext` in `@assistant-ui/react-ai-sdk` for the AI SDK implementation).

- `message`: `{ metadata?: unknown; }`
  - `metadata?`: `unknown`

### unstable\_formatInteractableSnapshot

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Canonical model-facing wording for one snapshot entry.

- `entry`: `Unstable_InteractableSnapshotEntry`

  - `id`: `string`
  - `name`: `string`
  - `state`: `unknown`
  - `partial?`: `boolean` — When true, \`state\` carries only the top-level fields that changed since the model's last known state; omitted fields are unchanged. The first snapshot of an instance is always full.

### unstable\_getInteractableVersions

> [!tip]
>
> **Experimental.** Unstable / Experimental (not actually removed).

Every version of interactable `id` recorded in the thread, oldest first, folded chronologically:

- the tool call whose `toolCallId` equals `id` seeds the baseline with its args (`origin: "create"`) — the `id: toolCallId` convention for tool-created interactables,
- a snapshot stamped on a user message is a `"user-edit"` version (a full snapshot replaces the state; a partial one shallow-merges),
- each accepted `update_*` call shallow-merges an `"update"` version.

The last entry is the state the model knows. Partial snapshots and update calls with no baseline to merge into are skipped.

- `messages`: `readonly SnapshotCarrierMessage[]`
- `id`: `string`
- `name`: `string`