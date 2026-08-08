# ThreadListItemRuntime
URL: /docs/api-reference/runtimes/thread-list-item-runtime

ThreadListItemRuntime state and actions for selecting, archiving, unarchiving, deleting, and renaming assistant-ui conversations.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### ThreadListItemRuntime

- `path`: `ThreadListItemRuntimePath`

  - `ref`: `string`
  - `threadSelector`: `ThreadListItemRuntimePath["threadSelector"]`
    - `type`: `"main"`

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

### ThreadListItemState

- `isMain`: `boolean`
- `isRunning`: `boolean` — Whether this thread has a run in progress, including a run that continues after the user switches to another thread.
- `id`: `string`
- `remoteId?`: `string`
- `externalId?`: `string`
- `status`: `ThreadListItemStatus`
- `title?`: `string`
- `lastMessageAt?`: `Date`
- `custom?`: `Record<string, unknown>`