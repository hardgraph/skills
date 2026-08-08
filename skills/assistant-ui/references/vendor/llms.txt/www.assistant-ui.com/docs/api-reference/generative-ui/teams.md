# Microsoft Teams
URL: /docs/api-reference/generative-ui/teams

Convert generative UI trees to Teams Adaptive Cards and decode Action.Submit payloads back into $action payloads.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### AdaptiveCardResult

The card and non-fatal warnings produced by [toAdaptiveCard](/docs/api-reference/generative-ui/teams#toadaptivecard).

- `card`: `TeamsAdaptiveCard`

  - `$schema`: `"http://adaptivecards.io/schemas/adaptive-card.json"`
  - `type`: `"AdaptiveCard"`
  - `version`: `"1.5"`
  - `body`: `readonly TeamsCardElement[]`
  - `actions`: `readonly TeamsCardAction[]`

- `warnings`: `TeamsConversionWarning[]`

### decodeSubmitData

Decodes the merged `activity.value` object a Teams bot receives on Action.Submit, inverting the `aui` envelope buildSubmitAction nests the resume action under. Adaptive Cards fold every other same-card input's value into the same submit object keyed by its `id`, so every top-level key besides `aui` is collected into `$input` (omitted when there are none). `aui` is reserved for the envelope: `toAdaptiveCard` renames any input whose id would collide to an unused id derived from it before encoding, so a same-card input value can never land on this key. `$input` and `type` are reserved on the way out: a `$input` key inside the envelope payload is dropped so the slot only ever reflects same-card input values, and the envelope's own `type` wins over a payload key of the same name. Every collected key (in `payload` or `$input`) is kept as an own data property of the returned object, even a `__proto__` or `constructor` key, since the object is always built by spreading and `Object.fromEntries` rather than by keyed assignment. Envelope keys are read as own properties only, so prototype-inherited `aui`, `type`, or `payload` values never dispatch. Returns `undefined` for a missing or malformed `aui` envelope; never throws.

- `value`: `unknown`

### TeamsAttachmentsResult

The attachments and non-fatal warnings produced by [toTeamsAttachments](/docs/api-reference/generative-ui/teams#toteamsattachments).

- `attachments`: `TeamsCardAttachment[]`
- `attachmentLayout?`: `"carousel"`
- `warnings`: `TeamsConversionWarning[]`

### TeamsConversionWarning

A non-fatal downgrade reported during Teams conversion.

- `code`: `"clamped" | "dropped" | "fallback"`
- `component`: `string`
- `detail`: `string`

### toAdaptiveCard

Converts a generative-ui tree into a Microsoft Teams Adaptive Card and non-fatal downgrade warnings. Sizes, weights, and colors map to Adaptive Card's semantic enums rather than raw values. An Input/Select/RadioGroup/ Checkbox/DatePicker whose id would be the reserved RESERVED\_INPUT\_ID is renamed with a warning (see `decodeSubmitData`). Never throws: an unknown `$type` is skipped with a "dropped" warning, and a malformed input resolves to an empty card plus a "dropped" warning instead of throwing.

- `node`: `unknown`
- `_options?`: `ToAdaptiveCardOptions`

### ToAdaptiveCardOptions

Options accepted by [toAdaptiveCard](/docs/api-reference/generative-ui/teams#toadaptivecard) and [toTeamsAttachments](/docs/api-reference/generative-ui/teams#toteamsattachments). Reserved for future extension; no options are currently defined.

```
export interface ToAdaptiveCardOptions {}
```

### toTeamsAttachments

Converts a generative-ui tree into Teams bot-framework message attachments. A root-level `Carousel` becomes one attachment per `Card` child (clamped to CAROUSEL\_ATTACHMENT\_CAP) with `attachmentLayout: "carousel"`; anything else converts through [toAdaptiveCard](/docs/api-reference/generative-ui/teams#toadaptivecard)'s engine into a single attachment. Never throws.

- `node`: `unknown`
- `_options?`: `ToAdaptiveCardOptions`