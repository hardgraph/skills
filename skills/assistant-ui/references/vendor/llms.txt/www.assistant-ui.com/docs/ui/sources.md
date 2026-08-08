# Sources
URL: /docs/ui/sources

Display URL sources with favicon, title, and external link.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

\[interactive preview omitted]

## Getting Started

1. ### Add `sources`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/sources
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/sources.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @base-ui/react class-variance-authority
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/sources.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/sources.tsx)
   - [components/assistant-ui/badge.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/badge.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/sources.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/sources.tsx \
     -o components/assistant-ui/badge.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/badge.tsx
   ```

2. ### Use in your application

   Pass `Sources` to `MessagePrimitive.Parts`:

   ```
   import { Sources } from "@/components/assistant-ui/sources";

   const AssistantMessage: FC = () => {
     return (
       <MessagePrimitive.Root className="...">
         <MessagePrimitive.Parts>
           {({ part }) => {
             if (part.type === "source") return <Sources {...part} />;
             return null;
           }}
         </MessagePrimitive.Parts>
       </MessagePrimitive.Root>
     );
   };
   ```

## Variants

Use the `variant` prop to change the visual style. The default is `outline`.

```
<Source variant="outline" />     // Border (default)
<Source variant="ghost" />       // No background
<Source variant="muted" />       // Solid muted background
<Source variant="secondary" />   // Secondary background
<Source variant="info" />        // Blue
<Source variant="warning" />     // Amber
<Source variant="success" />     // Emerald
<Source variant="destructive" /> // Red
```

## Sizes

Use the `size` prop to change the size.

```
<Source size="sm" />      // Small
<Source size="default" /> // Default
<Source size="lg" />      // Large
```

## API Reference

### `Sources`

The default export used as a `SourceMessagePartComponent`. Renders a single source part when `sourceType === "url"`. Also exposes compound sub-components for custom layouts.

| Prop         | Type                  | Default | Description                                        |
| ------------ | --------------------- | ------- | -------------------------------------------------- |
| `url`        | `string`              | —       | The URL of the source (provided by the runtime)    |
| `title`      | `string \| undefined` | —       | Display title; falls back to the domain if omitted |
| `sourceType` | `string`              | —       | Must be `"url"` to render; other types are ignored |

#### Compound sub-components

```
import { Sources } from "@/components/assistant-ui/sources";

<Sources.Root href="https://example.com">
  <Sources.Icon url="https://example.com" />
  <Sources.Title>Example</Sources.Title>
</Sources.Root>
```

| Sub-component   | Equivalent named export | Description                          |
| --------------- | ----------------------- | ------------------------------------ |
| `Sources.Root`  | `Source`                | Root anchor element                  |
| `Sources.Icon`  | `SourceIcon`            | Favicon with domain initial fallback |
| `Sources.Title` | `SourceTitle`           | Truncated title text                 |

### `Source`

Root container rendered as an `<a>` tag. Accepts all `<a>` props plus `variant` and `size`.

| Prop        | Type                                                                                                  | Default                 | Description                                                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `href`      | `string`                                                                                              | —                       | URL the link points to                                                                                                        |
| `variant`   | `"outline" \| "ghost" \| "muted" \| "secondary" \| "info" \| "warning" \| "success" \| "destructive"` | `"outline"`             | Visual style                                                                                                                  |
| `size`      | `"sm" \| "default" \| "lg"`                                                                           | `"default"`             | Size of the badge                                                                                                             |
| `target`    | `string`                                                                                              | `"_blank"`              | Link target                                                                                                                   |
| `rel`       | `string`                                                                                              | `"noopener noreferrer"` | Link rel attribute                                                                                                            |
| `asChild`   | `boolean`                                                                                             | `false`                 | Merge props with a child element instead of rendering an anchor. Effective with the Radix badge; the Base UI badge ignores it |
| `className` | `string`                                                                                              | —                       | Additional CSS classes                                                                                                        |

### `SourceIcon`

Displays the favicon for the given URL. Falls back to the domain initial inside a muted box when the favicon fails to load.

| Prop         | Type                         | Default                     | Description                                                                                   |
| ------------ | ---------------------------- | --------------------------- | --------------------------------------------------------------------------------------------- |
| `url`        | `string`                     | —                           | URL used to derive the favicon and fallback initial                                           |
| `faviconUrl` | `(domain: string) => string` | DuckDuckGo's `ip3` endpoint | Override the favicon source — useful in environments where the default service is unreachable |
| `className`  | `string`                     | —                           | Additional CSS classes applied to the `<img>` or fallback `<span>`                            |

#### Customizing the favicon provider

The default `<Sources>` component renders `<SourceIcon>` without forwarding `faviconUrl`, so the prop only applies when you compose `Sources.Icon` directly:

```
<Sources.Root href={url}>
  <Sources.Icon
    url={url}
    faviconUrl={(domain) => `https://my-favicon-proxy.example.com/${domain}.ico`}
  />
  <Sources.Title>{title}</Sources.Title>
</Sources.Root>
```

To change the default for every source in your app, edit the `defaultFaviconUrl` constant at the top of your copied `sources.tsx` — that is the single source of truth used by `<Sources>`.

### `SourceTitle`

Truncated title text rendered as a `<span>`.

| Prop        | Type        | Default | Description                                             |
| ----------- | ----------- | ------- | ------------------------------------------------------- |
| `children`  | `ReactNode` | —       | Title content to display                                |
| `className` | `string`    | —       | Additional CSS classes (default max-width is `37.5rem`) |

### `sourceVariants`

The underlying CVA variant function used to generate badge class names. Use this when building custom source-like components that need to match the built-in styling.

```
import { sourceVariants } from "@/components/assistant-ui/sources";

<span className={sourceVariants({ variant: "info", size: "sm" })}>
  Custom badge
</span>
```

## Composable API

Use the named exports to build fully custom source layouts:

```
import { Source, SourceIcon, SourceTitle } from "@/components/assistant-ui/sources";

<Source href="https://example.com" variant="muted" className="gap-2">
  <SourceIcon url="https://example.com" className="size-4" />
  <SourceTitle className="max-w-none font-medium">Example</SourceTitle>
</Source>
```

## Related Components

- [PartGrouping](/docs/ui/part-grouping) - Group sources by parentId