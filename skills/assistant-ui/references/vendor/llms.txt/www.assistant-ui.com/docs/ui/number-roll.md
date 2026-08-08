# Number Roll
URL: /docs/ui/number-roll

Animated number that rolls digits odometer-style when the value changes.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime. Use it anywhere in your application.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/number-roll
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/number-roll.json
```

Or install manually:

Then copy these source files from GitHub:

- [components/assistant-ui/number-roll.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/number-roll.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/number-roll.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/number-roll.tsx
```

This adds a `/components/assistant-ui/number-roll.tsx` file to your project, which you can adjust as needed. The component has no dependencies beyond React.

## Usage

```
import { NumberRoll } from "@/components/assistant-ui/number-roll";

export function TokenCounter({ count }: { count: number }) {
  return <NumberRoll value={count} format={{ notation: "compact" }} />;
}
```

When `value` changes, each digit rolls in place to its new value. Formatting is handled by `Intl.NumberFormat`, so structural changes animate too: when `700` becomes `1.1K` with compact notation, the surviving ones digit rolls from 0 to 1 while the leading `70` slides out and the decimal point, fraction digit, and `K` suffix slide in.

## Examples

### Compact Notation

Pass any `Intl.NumberFormatOptions` via the `format` prop. Crossing a compact-notation threshold animates the formatting change instead of remounting the whole string.

\[interactive preview component NumberRollCompactSample omitted]

Code for NumberRollCompactSample preview:

```tsx
import { useEffect, useState } from "react";
import { NumberRoll } from "@/components/assistant-ui/number-roll";
import { Button } from "@/components/ui/button";

function NumberRollCompactSample() {
  const [value, setValue] = useState(700);

  return (
    <NumberRoll
      value={value}
      locales="en-US"
      format={{ notation: "compact" }}
      className="text-5xl font-semibold"
    />
    <div className="flex items-center gap-2">
      <Button variant="outline" onClick={() => setValue((v) => v - 400)}>
        -400
      </Button>
      <Button variant="outline" onClick={() => setValue((v) => v + 400)}>
        +400
      </Button>
    </div>
    <span className="text-muted-foreground font-mono text-xs tabular-nums">
      value = {value}
    </span>
  );
}
```

### Formats and Locales

Currencies, percentages, suffixes, and non-English locales all work through the same `Intl.NumberFormat` pipeline. Compact thresholds and suffixes are locale-dependent (`1.2万` in `zh-CN`), so never hardcode them.

\[interactive preview component NumberRollFormatSample omitted]

Code for NumberRollFormatSample preview:

```tsx
import { useEffect, useState } from "react";
import { NumberRoll } from "@/components/assistant-ui/number-roll";
import { Button } from "@/components/ui/button";

function NumberRollFormatSample() {
  const [index, setIndex] = useState(0);
  const values = [1234.56, 98765.43, 412.07];
  const value = values[index % values.length]!;

  return (
    <div className="grid grid-cols-[auto_1fr] items-baseline gap-x-8 gap-y-3 text-xl">
      <span className="text-muted-foreground text-xs">Currency</span>
      <NumberRoll
        value={value}
        locales="en-US"
        format={{ style: "currency", currency: "USD" }}
      />
      <span className="text-muted-foreground text-xs">Percent</span>
      <NumberRoll
        value={value / 100000}
        locales="en-US"
        format={{ style: "percent" }}
      />
      <span className="text-muted-foreground text-xs">zh-CN compact</span>
      <NumberRoll
        value={value * 100}
        locales="zh-CN"
        format={{ notation: "compact" }}
      />
      <span className="text-muted-foreground text-xs">Suffix</span>
      <NumberRoll value={value} locales="en-US" suffix=" tokens" />
    </div>
    <Button variant="outline" onClick={() => setIndex((i) => i + 1)}>
      Shuffle
    </Button>
  );
}
```

### Live Counter

Rapid successive updates retarget the in-flight roll smoothly, which makes the component suitable for streaming token counts and other live metrics.

\[interactive preview component NumberRollLiveSample omitted]

Code for NumberRollLiveSample preview:

```tsx
import { useEffect, useState } from "react";
import { MinusIcon, PauseIcon, PlayIcon, PlusIcon } from "lucide-react";
import { NumberRoll } from "@/components/assistant-ui/number-roll";
import { Button } from "@/components/ui/button";

function NumberRollLiveSample() {
  const [running, setRunning] = useState(false);
  const [tokens, setTokens] = useState(0);

  useEffect(() => {
    if (!running) return;
    const interval = setInterval(() => {
      setTokens((t) => t + Math.floor(Math.random() * 120) + 20);
    }, 400);
    return () => clearInterval(interval);
  }, [running]);

  return (
    <div className="text-muted-foreground flex items-baseline gap-2 text-sm">
      <NumberRoll
        value={tokens}
        locales="en-US"
        format={{ notation: "compact", maximumFractionDigits: 1 }}
        className="text-foreground text-3xl font-semibold"
      />
      tokens
    </div>
    <div className="flex items-center gap-2">
      <Button
        variant="outline"
        size="icon"
        aria-label={running ? "Pause" : "Play"}
        onClick={() => setRunning((r) => !r)}
      >
        {running ? <PauseIcon /> : <PlayIcon />}
      </Button>
      <Button
        variant="outline"
        onClick={() => {
          setRunning(false);
          setTokens(0);
        }}
      >
        Reset
      </Button>
    </div>
  );
}
```

## How It Works

The value is formatted with `Intl.NumberFormat.formatToParts` and split into keyed parts. Integer digits are keyed by place value counted from the right, so when `999` becomes `1,000` the existing columns keep their identity and roll in place while only the new leading digit and separator enter. Symbols (decimal point, group separators, currency signs, compact suffixes) cross-fade and collapse via animated `grid-template-columns`.

Each digit renders a strip of 0 to 9 and animates a registered CSS custom property; CSS `mod()` math wraps the strip into an endless ribbon, so rolling from 9 to 0 continues in the trend direction instead of spinning backwards. There is no JavaScript animation loop and no animation library dependency.

In browsers without CSS `mod()` support, and during server rendering, the component renders the plain formatted string. With `prefers-reduced-motion`, values swap instantly without rolling. When server rendering, pass an explicit `locales`: the server's default locale can differ from the visitor's browser locale, and a mismatched formatted string causes a React hydration error. Locales whose default numbering system is non-Latin (such as `ar-EG`) cross-fade their digits as symbols instead of rolling.

The formatted value is exposed to screen readers as plain text while the animated digits are `aria-hidden`. Value changes are not announced automatically; wrap the component in an `aria-live` region if you need announcements.

## API Reference

### NumberRoll

- `value`: `number` — The number to display.
- `format?`: `Intl.NumberFormatOptions` — Number formatting options, e.g. \`{ notation: "compact" }\` or \`{ style: "currency", currency: "USD" }\`.
- `locales?`: `Intl.LocalesArgument` — Locale(s) passed to \`Intl.NumberFormat\`. Pass an explicit value in server-rendered apps so the server and client format identically.
- `prefix?`: `string` — Static text rendered before the number.
- `suffix?`: `string` — Static text rendered after the number.
- `trend`: `"auto" | "up" | "down"` (default `"auto"`) — Roll direction. \`auto\` follows the sign of the value change; \`up\` and \`down\` force a direction, wrapping digits through 9/0 as needed.
- `duration`: `number` (default `500`) — Digit roll duration in milliseconds. Enter/exit fades run at 60% of this value.
- `className?`: `string` — Additional CSS classes.

### Styling

The root applies `tabular-nums` so digits keep a constant width while rolling; the font you use must provide tabular figures for the layout to stay stable. Size and color are inherited from the surrounding text, so style it like any span:

```
<NumberRoll value={value} className="text-4xl font-semibold" />
```

Change the animation speed via the `duration` prop; it drives both the CSS timing and the cleanup that unmounts exited characters, so overriding the duration variable in CSS alone would remove them mid-fade. The fade and easing variables are safe to override via the `style` prop (the defaults are set as inline styles, so plain `className` overrides do not apply):

| Variable                     | Default                            | Description                                  |
| ---------------------------- | ---------------------------------- | -------------------------------------------- |
| `--aui-number-roll-duration` | `500ms` (from the `duration` prop) | Digit roll duration.                         |
| `--aui-number-roll-fade`     | 60% of the duration                | Enter/exit fade and width-collapse duration. |
| `--aui-number-roll-ease`     | `cubic-bezier(0.23, 1, 0.32, 1)`   | Digit roll easing.                           |

Parts are targetable via data attributes: `[data-slot="number-roll"]`, `[data-slot="number-roll-part"]`, `[data-slot="number-roll-digit"]`, and `[data-slot="number-roll-symbol"]`.

## Related Components

- [Context Display](/docs/ui/context-display) - Live token usage ring and bar
- [Message Timing](/docs/ui/message-timing) - Streaming stats badge
- [Badge](/docs/ui/badge) - Small status and metadata labels