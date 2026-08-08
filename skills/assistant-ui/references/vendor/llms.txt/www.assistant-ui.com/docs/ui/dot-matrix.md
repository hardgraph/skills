# Dot Matrix
URL: /docs/ui/dot-matrix

Tiny 5x5 dot-matrix indicator with 20 state-specific blink patterns.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime. Use it anywhere in your application.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/dot-matrix
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/dot-matrix.json
```

Or install manually:

Then copy these source files from GitHub:

- [components/assistant-ui/dot-matrix.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/dot-matrix.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/dot-matrix.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/dot-matrix.tsx
```

This adds a `/components/assistant-ui/dot-matrix.tsx` file to your project, which you can adjust as needed. The component has no dependencies beyond React.

## Usage

```
import { DotMatrix } from "@/components/assistant-ui/dot-matrix";

export function RunIndicator({ isRunning }: { isRunning: boolean }) {
  return <DotMatrix state={isRunning ? "loading" : "success"} />;
}
```

Dots inherit the surrounding text color, so the matrix renders dark dots on light backgrounds and light dots on dark backgrounds without configuration. Every state is a combination of a dot pattern, a motion, and a color, and switching states cross-fades each dot into its new pattern.

## States

| State         | Pattern                             |
| ------------- | ----------------------------------- |
| `idle`        | Dim static grid                     |
| `loading`     | Randomized twinkle (default)        |
| `thinking`    | Diagonal wave                       |
| `streaming`   | Falling rain, per-column streaks    |
| `searching`   | Horizontal sweep                    |
| `syncing`     | Rotating sweep around the center    |
| `connecting`  | Ripple expanding from the center    |
| `waiting`     | Ellipsis dots blinking in sequence  |
| `uploading`   | Wave rising upward                  |
| `downloading` | Wave falling downward               |
| `listening`   | Slow equalizer columns              |
| `speaking`    | Fast equalizer columns              |
| `recording`   | Red center dot breathing            |
| `success`     | Green check glyph, static           |
| `error`       | Red cross glyph, blinking           |
| `warning`     | Amber exclamation glyph, slow blink |
| `info`        | Blue info glyph, static             |
| `paused`      | Pause bars glyph, static            |
| `stopped`     | Square glyph, static                |
| `offline`     | Very dim static grid                |

The component exports `dotMatrixStates` (the ordered list above) and the `DotMatrixState` union type, so UIs can enumerate or map states without duplicating the list. New states are added by extending the `STATES` record in the component source with a glyph and a per-dot blink function.

## Examples

### State Lifecycle

Drive the `state` prop from your run status; the matrix morphs between patterns instead of swapping components.

\[interactive preview component DotMatrixLifecycleSample omitted]

Code for DotMatrixLifecycleSample preview:

```tsx
import { useEffect, useState } from "react";
import {
  DotMatrix,
  dotMatrixStates,
  type DotMatrixState,
} from "@/components/assistant-ui/dot-matrix";
import { Button } from "@/components/ui/button";

function DotMatrixLifecycleSample() {
  const lifecycle = [
    "idle",
    "connecting",
    "loading",
    "thinking",
    "streaming",
    "success",
  ] as const satisfies readonly DotMatrixState[];
  const [step, setStep] = useState(0);
  const state = lifecycle[step % lifecycle.length]!;

  return (
    <div className="flex items-center gap-3">
      <DotMatrix state={state} className="size-10" />
      <span className="text-muted-foreground font-mono text-sm">{state}</span>
    </div>
    <div className="flex items-center gap-2">
      <Button variant="outline" onClick={() => setStep((s) => s + 1)}>
        Next state
      </Button>
      <Button
        variant="outline"
        onClick={() => setStep(0)}
        disabled={state === "idle"}
      >
        Reset
      </Button>
    </div>
  );
}
```

### Inline With Text

At the default `size-4` the matrix aligns with text like an icon, and the dots adapt to inverted surfaces through `currentColor`.

\[interactive preview component DotMatrixInlineSample omitted]

Code for DotMatrixInlineSample preview:

```tsx
import { useEffect, useState } from "react";
import {
  DotMatrix,
  dotMatrixStates,
  type DotMatrixState,
} from "@/components/assistant-ui/dot-matrix";

function DotMatrixInlineSample() {
  const [running, setRunning] = useState(true);

  useEffect(() => {
    const interval = setInterval(() => setRunning((r) => !r), 3000);
    return () => clearInterval(interval);
  }, []);

  return (
    <div className="flex items-center gap-2 text-sm">
      <DotMatrix state={running ? "loading" : "success"} />
      {running ? "Generating response…" : "Done"}
    </div>
    <div className="bg-foreground text-background flex items-center gap-2 rounded-lg px-4 py-3 text-sm">
      <DotMatrix state={running ? "thinking" : "success"} />
      {running ? "Thinking…" : "Done"}
    </div>
    <div className="flex items-center gap-2 text-sm">
      <DotMatrix state={running ? "recording" : "stopped"} />
      {running ? "Recording…" : "Stopped"}
    </div>
  );
}
```

### Sizes

The matrix is an SVG, so any size utility scales it crisply.

\[interactive preview omitted]

## How It Works

The grid is a 5x5 SVG of `currentColor` circles. Blinking is a single CSS keyframe animation whose high/low opacity bounds come from registered per-dot CSS variables; the animation runs in every state (static states collapse the bounds) and the bounds carry a transition, which is what makes state changes cross-fade per dot. The randomized loading rhythm uses deterministic per-dot delays and durations, so server and client render identical markup and no JavaScript runs after render. With `prefers-reduced-motion`, the dots hold their resting opacity instead of blinking.

The root is a `role="status"` live region whose text content is the state name (or the `label` prop), so screen readers announce state changes; the SVG itself is `aria-hidden`.

## API Reference

### DotMatrix

- `state`: `DotMatrixState` (default `"loading"`) — One of the 20 built-in states listed above, controlling pattern, motion, and color.
- `label?`: `string` — Accessible label announced by screen readers. Defaults to the state name.
- `className?`: `string` — Additional CSS classes. Use size utilities to scale and text color utilities to recolor.

### Styling

Color follows `currentColor`, so `className="text-blue-500"` recolors the whole matrix; the outcome states (`success`, `error`, `warning`, `info`, `recording`, and the muted static states) set their own color which a `className` can override. Dots are targetable via `[data-slot="dot-matrix"]` and `[data-slot="dot-matrix-dot"]`, and the current state is exposed as `data-state` on the root.

## Related Components

- [Number Roll](/docs/ui/number-roll) - Animated rolling number
- [Badge](/docs/ui/badge) - Small status and metadata labels
- [Voice](/docs/ui/voice) - Voice activity visualization