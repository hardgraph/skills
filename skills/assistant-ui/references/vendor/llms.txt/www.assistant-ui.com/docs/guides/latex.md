# LaTeX in Chat Messages
URL: /docs/guides/latex

Render LaTeX math expressions in AI chat messages with KaTeX — drop-in equation support for React chat UIs built on assistant-ui.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

Render LaTeX mathematical expressions in chat messages using KaTeX.

> [!warn]
>
> LaTeX rendering is not enabled by default.

Choose one:

**react-markdown**

1. ### Install dependencies

   - packages

     - katex
     - rehype-katex
     - remark-math

2. ### Add KaTeX CSS to your layout

   ```
   import "katex/dist/katex.min.css";
   ```

3. ### Update `markdown-text.tsx`

   ```
   import { MarkdownTextPrimitive } from "@assistant-ui/react-markdown";
   import remarkMath from "remark-math";
   import rehypeKatex from "rehype-katex";

   const MarkdownTextImpl = () => {
     return (
       <MarkdownTextPrimitive
         remarkPlugins={[remarkGfm, remarkMath]} // add remarkMath
         rehypePlugins={[rehypeKatex]}           // add rehypeKatex
         className="aui-md"
         components={defaultComponents}
       />
     );
   };

   export const MarkdownText = memo(MarkdownTextImpl);
   ```

**Streamdown**

> [!info]
>
> Using [Streamdown](/docs/ui/streamdown) as your renderer? Math support is a first-party plugin — no remark or rehype packages needed.

1. ### Install dependencies

   - packages

     - @streamdown/math
     - katex

2. ### Add KaTeX CSS to your layout

   ```
   import "katex/dist/katex.min.css";
   ```

3. ### Pass the `math` plugin to `StreamdownTextPrimitive`

   ```
   import { math } from "@streamdown/math";
   import "katex/dist/katex.min.css";

   <StreamdownTextPrimitive plugins={{ math }} />
   ```

## Supported Formats

By default, remark-math (react-markdown path) supports:

- `$...$` for inline math
- `$$...$$` for display math
- Fenced code blocks with the `math` language identifier

## Supporting Alternative LaTeX Delimiters

Many language models emit math in delimiters that remark-math does not recognize:

- `\(...\)` for inline math and `\[...\]` for display math
- custom tags like `[/math]...[/math]` and `[/inline]...[/inline]`

`@assistant-ui/react-markdown` exports `normalizeMathDelimiters`, which rewrites these to the `$...$` and `$$...$$` form remark-math parses. Pass it to the `preprocess` prop of `MarkdownTextPrimitive`:

```
import {
  MarkdownTextPrimitive,
  normalizeMathDelimiters,
} from "@assistant-ui/react-markdown";

const MarkdownTextImpl = () => {
  return (
    <MarkdownTextPrimitive
      remarkPlugins={[remarkGfm, remarkMath]}
      rehypePlugins={[rehypeKatex]}
      preprocess={normalizeMathDelimiters}
      className="aui-md"
      components={defaultComponents}
    />
  );
};
```

The individual transforms `rewriteLatexBracketDelimiters` and `rewriteCustomMathTags` are exported too, for finer control over which delimiters are normalized.

> [!info]
>
> Using [Streamdown](/docs/ui/streamdown) as your renderer? The same helpers are exported from `@assistant-ui/react-streamdown` and accepted by the `preprocess` prop of `StreamdownTextPrimitive`.

### Currency amounts

With single-dollar inline math enabled (the default on the react-markdown path), remark-math reads a lone `$` as a math delimiter, so prose such as `$5 ... $10` is parsed as math. `escapeCurrencyDollars` escapes a `$` immediately followed by a digit so currency survives, while leaving the `$$` of display math intact. Compose it with the delimiter normalization:

```
import {
  normalizeMathDelimiters,
  escapeCurrencyDollars,
} from "@assistant-ui/react-markdown";

<MarkdownTextPrimitive
  preprocess={(text) => escapeCurrencyDollars(normalizeMathDelimiters(text))}
  // ...
/>;
```

> [!tip]
>
> Inside `MarkdownTextPrimitive`, the streamed text first passes through `preprocess` (delimiter normalization) and then through `useSmooth` (character by character accumulation), and only then reaches the markdown parser. Both run before remark-math sees the text, so delimiter replacement and the streaming smoothing stay streaming safe; a partially received delimiter is accumulated in the smoothing buffer rather than parsed mid fragment.