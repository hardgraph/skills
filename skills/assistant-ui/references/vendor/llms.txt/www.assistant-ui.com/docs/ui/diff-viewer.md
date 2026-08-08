# Diff Viewer
URL: /docs/ui/diff-viewer

Render code diffs with syntax highlighting for additions and deletions.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/diff-viewer
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/diff-viewer.json
```

Or install manually:

```bash
npm install @assistant-ui/react-markdown class-variance-authority diff parse-diff
```

Then copy these source files from GitHub:

- [components/assistant-ui/diff-viewer.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/diff-viewer.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/diff-viewer.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/diff-viewer.tsx
```

## Usage

```
import { DiffViewer } from "@/components/assistant-ui/diff-viewer";

// With a unified diff patch
<DiffViewer patch={diffString} />

// With file comparison
<DiffViewer
  oldFile={{ content: "old content", name: "file.txt" }}
  newFile={{ content: "new content", name: "file.txt" }}
/>
```

### As Markdown Language Override

Integrate with `MarkdownTextPrimitive` to render diff code blocks:

```
import { DiffViewer } from "@/components/assistant-ui/diff-viewer";

const MarkdownTextImpl = () => {
  return (
    <MarkdownTextPrimitive
      remarkPlugins={[remarkGfm]}
      className="aui-md"
      components={defaultComponents}
      componentsByLanguage={{
        diff: {
          SyntaxHighlighter: ({ code }) => <DiffViewer patch={code} />
        },
      }}
    />
  );
};

export const MarkdownText = memo(MarkdownTextImpl);
```

## Examples

### Unified View

Shows all changes in a single column with `+`/`-` indicators. This is the default mode.

\[interactive preview omitted]

```
<DiffViewer patch={diffString} viewMode="unified" />
```

### Split View

Shows old content on the left, new content on the right side-by-side.

\[interactive preview omitted]

```
<DiffViewer patch={diffString} viewMode="split" />
```

### Interactive Mode Toggle

\[interactive preview component DiffViewerViewModesSample omitted]

Code for DiffViewerViewModesSample preview:

```tsx
import { useState } from "react";
import { DiffViewer } from "@/components/assistant-ui/diff-viewer";
import { cn } from "@/lib/utils";

function DiffViewerViewModesSample() {
  const [viewMode, setViewMode] = useState<"unified" | "split">("unified");

  return (
    <div className="flex h-full w-full flex-col items-center justify-center gap-4 p-4">
      <div className="flex gap-2">
        <button
          type="button"
          onClick={() => setViewMode("unified")}
          className={cn("rounded-md px-3 py-1.5 text-sm transition-colors", {
            "bg-primary text-primary-foreground": viewMode === "unified",
            "bg-muted hover:bg-muted/80": viewMode !== "unified",
          })}
        >
          Unified
        </button>
        <button
          type="button"
          onClick={() => setViewMode("split")}
          className={cn("rounded-md px-3 py-1.5 text-sm transition-colors", {
            "bg-primary text-primary-foreground": viewMode === "split",
            "bg-muted hover:bg-muted/80": viewMode !== "split",
          })}
        >
          Split
        </button>
      </div>
      <div className="w-full max-w-3xl">
        <DiffViewer patch={SAMPLE_PATCH} viewMode={viewMode} />
      </div>
    </div>
  );
}
```

### Variants

\[interactive preview omitted]

### Sizes

\[interactive preview omitted]

### Theming

DiffViewer uses CSS variables for colors. Override them in your CSS:

```
[data-slot="diff-viewer"] {
  --diff-add-bg: rgba(46, 160, 67, 0.15);
  --diff-add-text: #1a7f37;
  --diff-add-text-dark: #3fb950;
  --diff-del-bg: rgba(248, 81, 73, 0.15);
  --diff-del-text: #cf222e;
  --diff-del-text-dark: #f85149;
}
```

| Variable               | Description                               |
| ---------------------- | ----------------------------------------- |
| `--diff-add-bg`        | Background for added lines                |
| `--diff-add-text`      | Text color for added lines (light mode)   |
| `--diff-add-text-dark` | Text color for added lines (dark mode)    |
| `--diff-del-bg`        | Background for deleted lines              |
| `--diff-del-text`      | Text color for deleted lines (light mode) |
| `--diff-del-text-dark` | Text color for deleted lines (dark mode)  |

## API Reference

### DiffViewer

The main component for rendering diffs.

- `patch?`: `string` — Unified diff string (e.g., output from git diff).
- `code?`: `string` — Alias for patch (for markdown integration).
- `oldFile?`: `{ content: string; name?: string }` — Old file for direct comparison.
- `newFile?`: `{ content: string; name?: string }` — New file for direct comparison.
- `viewMode`: `"unified" | "split"` (default `"unified"`) — Display mode for the diff.
- `variant`: `"default" | "ghost" | "muted"` (default `"default"`) — Visual style variant.
- `size`: `"sm" | "default" | "lg"` (default `"default"`) — Font size.
- `showLineNumbers`: `boolean` (default `true`) — Show line numbers.
- `showIcon`: `boolean` (default `true`) — Show file extension badge in header.
- `showStats`: `boolean` (default `true`) — Show addition/deletion counts in header.
- `className?`: `string` — Additional CSS classes.

### Composable API

| Component             | Description                                |
| --------------------- | ------------------------------------------ |
| `DiffViewer`          | Main component that renders the diff.      |
| `DiffViewerFile`      | Wrapper for each file in multi-file diffs. |
| `DiffViewerHeader`    | File name header with icon and stats.      |
| `DiffViewerContent`   | Scrollable content area.                   |
| `DiffViewerLine`      | Individual line in unified mode.           |
| `DiffViewerSplitLine` | Side-by-side line pair in split mode.      |
| `DiffViewerFileBadge` | File extension badge (e.g., "TS").         |
| `DiffViewerStats`     | Addition/deletion count display.           |

### Style Variants (CVA)

| Export                 | Description                       |
| ---------------------- | --------------------------------- |
| `diffViewerVariants`   | Styles for the root container.    |
| `diffLineVariants`     | Background styles for diff lines. |
| `diffLineTextVariants` | Text color styles for diff lines. |

```
import {
  diffViewerVariants,
  diffLineVariants,
  diffLineTextVariants,
} from "@/components/assistant-ui/diff-viewer";

// Use variants directly
<div className={diffViewerVariants({ variant: "ghost", size: "sm" })}>
  Custom diff container
</div>
```

### Utilities

| Export                  | Description                                     |
| ----------------------- | ----------------------------------------------- |
| `parsePatch(patch)`     | Parse unified diff string into structured data. |
| `computeDiff(old, new)` | Compute diff between two strings.               |
| `ParsedLine`            | Type for a single diff line.                    |
| `ParsedFile`            | Type for a parsed file with lines and stats.    |
| `SplitLinePair`         | Type for a side-by-side line pair.              |

## Styling

### Data Attributes

Use data attributes for custom styling:

| Attribute        | Values                                                              | Description              |
| ---------------- | ------------------------------------------------------------------- | ------------------------ |
| `data-slot`      | `"diff-viewer"`, `"diff-viewer-header"`, `"diff-viewer-line"`, etc. | Component identification |
| `data-type`      | `"add"`, `"del"`, `"normal"`, `"empty"`                             | Line type                |
| `data-view-mode` | `"unified"`, `"split"`                                              | Current view mode        |
| `data-variant`   | `"default"`, `"ghost"`, `"muted"`                                   | Current variant          |

### Custom CSS Example

```
[data-slot="diff-viewer"][data-view-mode="split"] {
  /* Custom split view styles */
}

[data-slot="diff-viewer-line"][data-type="add"] {
  /* Custom addition styles */
}

[data-slot="diff-viewer-line"][data-type="del"] {
  /* Custom deletion styles */
}
```

## Related Components

- [Markdown](/docs/ui/markdown) - Rich text rendering where diff viewer can be integrated
- [Syntax Highlighting](/docs/ui/syntax-highlighting) - Code highlighting for other languages