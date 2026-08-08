# Syntax Highlighting
URL: /docs/ui/syntax-highlighting

Code block syntax highlighting with react-shiki or react-syntax-highlighter.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!warn]
>
> Syntax highlighting is not enabled in markdown by default.

\[interactive preview omitted]

> [!info]
>
> `assistant-ui` provides two options for syntax highlighting:
>
> - **react-shiki** (recommended for performance & dynamic language support)
> - **react-syntax-highlighter** (legacy - Prism or Highlight.js based)

---

## react-shiki

1. #### Add `shiki-highlighter`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/shiki-highlighter
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/shiki-highlighter.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @assistant-ui/react-markdown react-shiki
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/shiki-highlighter.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/shiki-highlighter.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/shiki-highlighter.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/shiki-highlighter.tsx
   ```

   This adds a `/components/assistant-ui/shiki-highlighter.tsx` file to your project and installs the `react-shiki` dependency. The highlighter can be customized by editing the config in the `shiki-highlighter.tsx` file. While a message part is still streaming, the component renders the plain code without tokenization and highlights once the part settles, so streaming stays cheap.

2. #### Add it to `defaultComponents` in `markdown-text.tsx`

   ```
   import { SyntaxHighlighter } from "./shiki-highlighter";

   export const defaultComponents = memoizeMarkdownComponents({
     SyntaxHighlighter: SyntaxHighlighter,
     h1: /* ... */,
     // ...other elements...
   });
   ```

### Options

See [react-shiki documentation](https://github.com/AVGVSTVS96/react-shiki#props) for all available options.

Key options:

- `theme` - Shiki theme or multi-theme object (`{ light, dark, ... }`)
- `language` - Language for highlighting (default: `"text"`)
- `defaultColor` - Default color mode (`string | false`, e.g. `light-dark()`)
- `delay` - Delay between highlights, useful for streaming (default: `0`)
- `customLanguages` - Custom languages to preload for dynamic support
- `codeToHastOptions` - All other options accepted by Shiki's [`codeToHast`](https://github.com/shikijs/shiki/blob/main/packages/types/src/options.ts#L121)

### Dual/multi theme support

To use multiple themes, pass a theme object:

```
<ShikiHighlighter
  /* ... */
  theme={{
    light: "github-light",
    dark: "github-dark",
  }}
  defaultColor="light-dark()"
  /* ... */
>
```

> **Note:** The `shiki-highlighter` component sets `defaultColor="light-dark()"` automatically. Only set this manually if using `ShikiHighlighter` directly.

With `defaultColor="light-dark()"`, theme switching is automatic based on your site's `color-scheme`. No custom Shiki CSS overrides are required.

Set `color-scheme` on your app root:

System-based (follows OS/browser preference):

```
:root {
  color-scheme: light dark;
}
```

Class-based theme switching:

```
:root {
  color-scheme: light;
}
:root.dark {
  color-scheme: dark;
}
```

If you need broader support for older browsers, you can still use the manual CSS-variable switching approach from the Shiki dual-theme docs.

For more information:

- [react-shiki multi-theme/reactive themes](https://github.com/AVGVSTVS96/react-shiki)
- [Shiki dual themes + `light-dark()`](https://shiki.style/guide/dual-themes)

### Bundle Optimization

By default, `react-shiki` includes the full Shiki bundle, which contains all supported languages and themes.

To reduce bundle size, you can use the web bundle by changing the import to `react-shiki/web`, to include a smaller bundle of web related languages:

```
import ShikiHighlighter, { type ShikiHighlighterProps } from "react-shiki/web";
```

#### Custom Bundles

For strict bundle size control, `react-shiki` also supports custom bundles created using `createHighlighterCore` from `react-shiki/core` (re-exported from Shiki):

```
import { createHighlighterCore, createOnigurumaEngine } from "react-shiki/core";

// Create the highlighter
// Use dynamic imports to load languages and themes on client on demand
const customHighlighter = await createHighlighterCore({
  themes: [import("@shikijs/themes/nord")],
  langs: [
    import("@shikijs/langs/javascript"),
    import("@shikijs/langs/typescript"),
  ],
  engine: createOnigurumaEngine(import("shiki/wasm")), 
});

// Then pass it to the highlighter prop
<SyntaxHighlighter
  {...props}
  language={language}
  theme={theme}
  highlighter={customHighlighter}
/>;
```

> [!info]
>
> For more information, see [react-shiki - bundle options](https://github.com/avgvstvs96/react-shiki#bundle-options).

---

## react-syntax-highlighter

> [!warn]
>
> This option may be removed in a future release. Consider using [react-shiki](#react-shiki) instead.

1. #### Add `syntax-highlighter`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/syntax-highlighter
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/syntax-highlighter.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react-markdown @assistant-ui/react-syntax-highlighter @types/react-syntax-highlighter react-syntax-highlighter
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/syntax-highlighter.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/syntax-highlighter.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/syntax-highlighter.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/syntax-highlighter.tsx
   ```

   Adds a `/components/assistant-ui/syntax-highlighter.tsx` file to your project and installs the `react-syntax-highlighter` dependency.

2. #### Add it to `defaultComponents` in `markdown-text.tsx`

   ```
   import { SyntaxHighlighter } from "./syntax-highlighter";

   export const defaultComponents = memoizeMarkdownComponents({
     SyntaxHighlighter: SyntaxHighlighter,
     h1: /* ... */,
     // ...other elements...
   });
   ```

### Options

Supports all options from [`react-syntax-highlighter`](https://github.com/react-syntax-highlighter/react-syntax-highlighter#props).

### Bundle Optimization

By default, the syntax highlighter uses a light build that only includes languages you register. To include all languages:

```
import { makePrismAsyncSyntaxHighlighter } from "@assistant-ui/react-syntax-highlighter/full";
```

## Related Components

- [Markdown](/docs/ui/markdown) - Rich text rendering that uses syntax highlighting
- [Mermaid](/docs/ui/mermaid) - Render diagrams instead of code blocks