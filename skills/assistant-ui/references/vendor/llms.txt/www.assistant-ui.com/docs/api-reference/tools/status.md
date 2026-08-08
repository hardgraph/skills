# Tool Status
URL: /docs/api-reference/tools/status

Read tool arguments, execution status, and result state inside assistant-ui tool UI components.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### useToolArgsStatus

Reads whether each argument field for the current tool-call message part is still streaming or complete.

Use inside a tool-call renderer to avoid showing incomplete argument values as final.

```
function WeatherToolUI({
  args,
}: ToolCallMessagePartProps<{ city: string }>) {
  const { propStatus } = useToolArgsStatus<{ city: string }>();

  return (
    <span>
      {propStatus.city === "streaming" ? "Reading city..." : args.city}
    </span>
  );
}
```

```
const useToolArgsStatus: <TArgs extends Record<string, unknown> = Record<string, unknown>>() => ToolArgsStatus<TArgs>;
```

### useToolCallElapsed

Hook that returns the elapsed wall-clock time of the current tool call in milliseconds, ticking once per second while the call runs.

Reads `part.timing`. Returns `undefined` when the part is not a tool call, carries no timing, ended without a recorded completion (the duration is unknown), or when no message part scope is available (so kit components stay renderable standalone, e.g. in docs previews).

```
function ToolDuration() {
  const elapsedMs = useToolCallElapsed();
  if (elapsedMs === undefined) return null;
  return <span>{(elapsedMs / 1000).toFixed(1)}s</span>;
}
```

```
const useToolCallElapsed: () => number;
```