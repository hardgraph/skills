# Slack Block Kit
URL: /docs/api-reference/generative-ui/slack

Convert generative UI trees to Slack Block Kit payloads and decode block_actions interactions back into $action payloads.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### decodeBlockAction

Decodes one structural entry from a Slack `block_actions` payload. `$input` is reserved for the runtime selection: a `$input` key inside the button's JSON `value` payload is dropped rather than spread into the result.

- `action`: `unknown`

### fromSlackBlocks

Converts Slack Block Kit JSON back into a generative-ui IR tree, inverting [toSlackBlocks](/docs/api-reference/generative-ui/slack#toslackblocks). Accepts a blocks array or a `{ blocks }` wrapper; malformed input resolves to an empty node list plus a warning, and an unrecognized block or action element is skipped with a "dropped" warning. Every array read along the way (blocks, fields, options, elements, cards, rows, cells) is bounded to its site's cap before it is mapped over, so a hostile array (sparse or proxied with a fabricated `length`) is clamped with a "clamped" warning instead of stalling the event loop. Never throws.

The Slack shape is lossier than the IR, so some information cannot be recovered: a context block's elements always decode as `Caption` (the original `Badge` vs `Caption` distinction does not survive the round trip), a button's `buttonStyle` is only recovered for the `primary`/`danger` Slack styles, a native alert block's `title` and `description` merge into a single `description` string, and a card's layout collapses to its `title`, a flat `children` list (image, then text, then caption, in that order), and `confirm`/`cancel` footer buttons. A section field's label and value are split on the first `*`+newline delimiter, so a label or value that itself contains that exact delimiter cannot be split back unambiguously. A data-table cell that Slack carries as `raw_text` always decodes as a string, even when its text is the stringified form of a boolean, since the Slack shape does not distinguish a boolean from ordinary text.

- `blocks`: `unknown`

### FromSlackBlocksResult

The IR nodes and non-fatal warnings produced by converting Slack Block Kit JSON back into generative UI.

- `nodes`: `UIElement[]`
- `warnings`: `SlackConversionWarning[]`

### SlackBlocksResult

The blocks and non-fatal warnings produced by Slack conversion.

- `blocks`: `SlackBlock[]`
- `warnings`: `SlackConversionWarning[]`

### SlackConversionWarning

A non-fatal downgrade reported during Slack conversion.

- `code`: `"clamped" | "dropped" | "fallback"`
- `component`: `string`
- `detail`: `string`

### toSlackBlocks

Converts a generative-UI tree into Slack Block Kit JSON and downgrade warnings.

- `node`: `unknown`
- `options?`: `ToSlackBlocksOptions`
  - `surface?`: `"message" | "modal"`

### ToSlackBlocksOptions

Options that select the target Slack surface.

- `surface?`: `"message" | "modal"`