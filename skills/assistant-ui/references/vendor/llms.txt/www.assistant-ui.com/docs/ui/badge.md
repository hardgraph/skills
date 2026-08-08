# Badge
URL: /docs/ui/badge

A small label component for displaying status, categories, or metadata.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime. Use it anywhere in your application.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/badge
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/badge.json
```

Or install manually:

```bash
npm install @base-ui/react class-variance-authority
```

Then copy these source files from GitHub:

- [components/assistant-ui/badge.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/badge.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/badge.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/badge.tsx
```

## Usage

```
import { Badge } from "@/components/assistant-ui/badge";

export function Example() {
  return <Badge>Label</Badge>;
}
```

## Examples

### Variants

Use the `variant` prop to change the visual style.

\[interactive preview omitted]

```
<Badge variant="outline" />     // Border (default)
<Badge variant="secondary" />   // Secondary background
<Badge variant="muted" />       // Muted background
<Badge variant="ghost" />       // No background
<Badge variant="info" />        // Blue/info style
<Badge variant="warning" />     // Amber/warning style
<Badge variant="success" />     // Green/success style
<Badge variant="destructive" /> // Red/error style
```

### Sizes

Use the `size` prop to change the badge size.

\[interactive preview omitted]

```
<Badge size="sm" />      // Small
<Badge size="default" /> // Default
<Badge size="lg" />      // Large
```

### With Icons

Badges automatically style SVG icons.

\[interactive preview component BadgeWithIconSample omitted]

Code for BadgeWithIconSample preview:

```tsx
import { ArrowUpRight, Check, X, AlertCircle, Loader2 } from "lucide-react";
import { Badge } from "@/components/assistant-ui/badge";

function BadgeWithIconSample() {
  return (
    <Badge variant="secondary">
      <Check />
      Success
    </Badge>
    <Badge variant="destructive">
      <X />
      Failed
    </Badge>
    <Badge variant="muted">
      <AlertCircle />
      Pending
    </Badge>
  );
}
```

### As Link

Compose with `render` (Base UI) or `asChild` (Radix) to render the badge as a different element, like a link.

\[interactive preview component BadgeAsLinkSample omitted]

Code for BadgeAsLinkSample preview:

```tsx
import { ArrowUpRight, Check, X, AlertCircle, Loader2 } from "lucide-react";
import { Badge } from "@/components/assistant-ui/badge";

function BadgeAsLinkSample() {
  return (
    <Badge
      variant="muted"
      render={
        <a
          href="https://github.com/assistant-ui/assistant-ui"
          target="_blank"
          rel="noopener noreferrer"
        />
      }
    >
      GitHub
      <ArrowUpRight />
    </Badge>
    <Badge
      variant="outline"
      render={
        <a
          href="https://www.npmjs.com/package/@assistant-ui/react"
          target="_blank"
          rel="noopener noreferrer"
        />
      }
    >
      npm
      <ArrowUpRight />
    </Badge>
  );
}
```

### Animated

Combine with CSS transitions for scroll and color animations.

\[interactive preview component BadgeAnimatedSample omitted]

Code for BadgeAnimatedSample preview:

```tsx
import { useEffect, useState } from "react";
import { ArrowUpRight, Check, X, AlertCircle, Loader2 } from "lucide-react";
import { Badge } from "@/components/assistant-ui/badge";
import { cn } from "@/lib/utils";

function BadgeAnimatedSample() {
  const [status, setStatus] = useState<"loading" | "success">("loading");

  useEffect(() => {
    const interval = setInterval(() => {
      setStatus((prev) => (prev === "loading" ? "success" : "loading"));
    }, 2000);
    return () => clearInterval(interval);
  }, []);

  return (
    <Badge
      variant={status === "loading" ? "muted" : "success"}
      className="overflow-hidden"
    >
      <span className="relative inline-flex h-4 overflow-hidden">
        <span
          className={cn(
            "invisible overflow-hidden transition-[max-width] duration-500",
            status === "loading" ? "max-w-24" : "max-w-0",
          )}
        >
          <span className="flex items-center gap-1 whitespace-nowrap">
            <Loader2 className="shrink-0" />
            Loading
          </span>
        </span>
        <span
          className={cn(
            "invisible overflow-hidden transition-[max-width] duration-500",
            status === "success" ? "max-w-40" : "max-w-0",
          )}
        >
          <span className="flex items-center gap-1 whitespace-nowrap">
            <Check className="shrink-0" />
            Mission Success
          </span>
        </span>

        <span
          className={cn(
            "absolute inset-y-0 start-0 flex items-center gap-1 whitespace-nowrap transition-all duration-500",
            status === "loading"
              ? "translate-y-0 opacity-100"
              : "-translate-y-4 opacity-0",
          )}
        >
          <Loader2 className="shrink-0 animate-spin" />
          Loading
        </span>
        <span
          className={cn(
            "absolute inset-y-0 start-0 flex items-center gap-1 whitespace-nowrap transition-all duration-500",
            status === "success"
              ? "translate-y-0 opacity-100"
              : "translate-y-4 opacity-0",
          )}
        >
          <Check className="shrink-0" />
          Mission Success
        </span>
      </span>
    </Badge>
  );
}
```

## API Reference

### Badge

- `variant`: `"outline" | "secondary" | "muted" | "ghost" | "info" | "warning" | "success" | "destructive"` (default `"outline"`) — The visual style of the badge.
- `size`: `"sm" | "default" | "lg"` (default `"default"`) — The size of the badge.
- `render?`: `ReactElement | function` — Base UI: compose as a different element instead of rendering a span.
- `asChild`: `boolean` (default `false`) — Radix: merge props with a child element instead of rendering a span.
- `className?`: `string` — Additional CSS classes.

### Style Variants (CVA)

| Export          | Description                     |
| --------------- | ------------------------------- |
| `badgeVariants` | Styles for the badge component. |

```
import { badgeVariants } from "@/components/assistant-ui/badge";

<span className={badgeVariants({ variant: "muted", size: "sm" })}>
  Custom Badge
</span>
```