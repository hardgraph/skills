# SuggestionPrimitive
URL: /docs/api-reference/primitives/suggestion

Suggestion primitives for rendering starter prompts, follow-up actions, and composer suggestions in assistant-ui threads.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

For examples and usage patterns, see [Suggestion](/docs/primitives/suggestion).

## API Reference

### Title

Renders the title of the suggestion.

This primitive renders a `<span>` element unless `asChild` is set.

```
<SuggestionPrimitive.Title />
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Description

Renders the description/label of the suggestion.

This primitive renders a `<span>` element unless `asChild` is set.

```
<SuggestionPrimitive.Description />
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`

### Trigger

A button that triggers the suggestion action (send or insert into composer).

This primitive renders a `<button>` element unless `asChild` is set.

```
<SuggestionPrimitive.Trigger send>
  Click me
</SuggestionPrimitive.Trigger>
```

- `asChild`: `boolean` (default `false`) — Change the default rendered element for the one passed as a child, merging their props and behavior.\
  \
  Read the [Composition](/docs/api-reference/primitives/composition) guide for more details.
- `render?`: `ReactElement`
- `send?`: `boolean` — When true, automatically sends the message. When false, replaces or appends the composer text with the suggestion - depending on the value of \`clearComposer\`.
- `clearComposer`: `boolean` (default `true`) — Whether to clear the composer after sending. A send queued while a run is in progress never clears the composer. When send is set to false, determines if composer text is replaced with suggestion (true, default), or if it's appended to the composer text (false).