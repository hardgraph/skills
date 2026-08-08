# Model Selector
URL: /docs/ui/model-selector

Composable model picker with reasoning effort levels, search, and runtime integration.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

A picker that lets users switch between AI models and choose a reasoning effort (thinking) level. It is built on Popover + Command, so search, provider grouping, and filtering compose in without being built in. The default export integrates with assistant-ui's `ModelContext` system, so the selection reaches your backend on every request with no extra wiring.

\[interactive preview omitted]

## Getting Started

1. ### Add `model-selector`

   With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

   ```bash
   npx shadcn@latest add @assistant-ui/model-selector
   ```

   Or add by direct URL without registry configuration:

   ```bash
   npx shadcn@latest add https://r.assistant-ui.com/base/model-selector.json
   ```

   Or install manually:

   ```bash
   npm install @assistant-ui/react @base-ui/react class-variance-authority
   ```

   ```bash
   npx shadcn@latest add command popover
   ```

   Then copy these source files from GitHub:

   - [components/assistant-ui/model-selector.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/model-selector.tsx)

   ```bash
   curl -sSL --create-dirs \
     -o components/assistant-ui/model-selector.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/model-selector.tsx
   ```

2. ### Use in your application

   Place the `ModelSelector` inside your thread component, typically in the composer area. Each model needs an `id` and a display `name`; everything else is optional:

   ```
   import { ModelSelector } from "@/components/assistant-ui/model-selector";

   const ComposerAction: FC = () => {
     return (
       <div className="flex items-center gap-1">
         <ModelSelector
           models={[
             { id: "gpt-5.6-luna", name: "GPT-5.6 Luna", description: "Fast and efficient" },
             { id: "gpt-5.6-terra", name: "GPT-5.6 Terra", description: "Balanced performance" },
             { id: "gpt-5.6-sol", name: "GPT-5.6 Sol", description: "Most capable", efforts: true },
           ]}
           defaultValue="gpt-5.6-luna"
           defaultEffort="medium"
           size="sm"
         />
       </div>
     );
   };
   ```

3. ### Read the selection in your API route

   The selected model's `id` arrives as `config.modelName`, and the effort level as `config.reasoningEffort`:

   ```
   export async function POST(req: Request) {
     const { messages, config } = await req.json();

     const result = streamText({
       model: openai(config?.modelName ?? "gpt-5.6-luna"),
       providerOptions: {
         openai:
           config?.reasoningEffort !== undefined
             ? { reasoningEffort: config.reasoningEffort }
             : {},
       },
       messages: await convertToModelMessages(messages),
     });

     return result.toUIMessageStreamResponse();
   }
   ```

   `config.reasoningEffort` is only present when the selected model supports the chosen level, so the route only forwards it when it exists. See [How It Works](#how-it-works).

## Reasoning Efforts

A model that declares `efforts` shows a "Thinking" row at the bottom of the popover. `efforts: true` enables the default Low / Medium / High levels; pass a list of `{ id, name }` objects to define your own:

```
{
  id: "gpt-5.6-sol",
  name: "GPT-5.6 Sol",
  efforts: [
    { id: "minimal", name: "Minimal" },
    { id: "high", name: "High" },
  ],
}
```

Omit `efforts` for models without configurable reasoning. The row is hidden while such a model is selected.

### Sticky Selection

The effort selection survives model switches. Switching to a model that doesn't support the current level omits `reasoningEffort` from the request instead of resetting the user's choice, and the level applies again when the user switches back. The exported `resolveModelEffort` helper applies the same rule if you build your own runtime integration around `ModelSelector.Root`; see [`resolveModelEffort`](#resolvemodeleffort).

### Custom Effort UI

`ModelSelector.Effort` lays the levels out as horizontal segments, which overflows the popover width once a model has more than a few. For those cases, or for a different layout such as a slider or a sub-dropdown, build your own control with the `useModelSelectorEfforts` hook. It exposes the selected model's levels and the active selection:

```
import { useModelSelectorEfforts } from "@/components/assistant-ui/model-selector";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

function EffortDropdown() {
  const { efforts, effort, setEffort } = useModelSelectorEfforts();
  if (!efforts?.length) return null;

  return (
    <div className="flex items-center justify-between gap-3 border-t px-3 py-2">
      <span className="text-muted-foreground text-xs">Thinking</span>
      <DropdownMenu>
        <DropdownMenuTrigger className="text-xs">
          {efforts.find((e) => e.id === effort)?.name ?? "Select"}
        </DropdownMenuTrigger>
        <DropdownMenuContent align="end">
          <DropdownMenuRadioGroup value={effort} onValueChange={setEffort}>
            {efforts.map((option) => (
              <DropdownMenuRadioItem key={option.id} value={option.id}>
                {option.name}
              </DropdownMenuRadioItem>
            ))}
          </DropdownMenuRadioGroup>
        </DropdownMenuContent>
      </DropdownMenu>
    </div>
  );
}
```

Render it inside `ModelSelector.Content` in place of `ModelSelector.Effort`. The same hook supports any shape that reads the levels and writes the selection.

## Provider Logos

Each model's `icon` accepts any `ReactNode` and renders in the trigger and the dropdown items. The optional `logos` registry item ships `OpenAILogo`, `ClaudeLogo`, and `GeminiLogo` marks to plug in:

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/logos
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/logos.json
```

Or install manually:

Then copy these source files from GitHub:

- [components/assistant-ui/logos.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/logos.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/logos.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/logos.tsx
```

```
import { ClaudeLogo, GeminiLogo, OpenAILogo } from "@/components/assistant-ui/logos";

<ModelSelector
  models={[
    { id: "gpt-5.6-sol", name: "GPT-5.6 Sol", icon: <OpenAILogo /> },
    { id: "claude-opus-4.5", name: "Claude Opus 4.5", icon: <ClaudeLogo /> },
    { id: "gemini-3-pro", name: "Gemini 3 Pro", icon: <GeminiLogo /> },
  ]}
/>;
```

Icons are opt-in per model — omit `icon` and the entry renders text-only.

## Search

Search is opt-in. Pass `searchable` to the default component:

```
<ModelSelector models={models} searchable />
```

The same prop works on `ModelSelector.Content` when it renders its default children. Or compose `ModelSelector.Search` into a custom layout. Matching runs against each model's `id`, `name`, and `keywords`; add the provider name to `keywords` so typing "openai" finds its models.

## Composition

All parts are exported individually. The default popover content is `List` + `Effort`; replace it to add search, provider groups, or anything else:

```
import {
  ModelSelectorRoot,
  ModelSelectorTrigger,
  ModelSelectorContent,
  ModelSelectorSearch,
  ModelSelectorList,
  ModelSelectorEmpty,
  ModelSelectorGroup,
  ModelSelectorItem,
  ModelSelectorEffort,
} from "@/components/assistant-ui/model-selector";

<ModelSelectorRoot
  models={models}
  value={modelId}
  onValueChange={setModelId}
  effort={effort}
  onEffortChange={setEffort}
>
  <ModelSelectorTrigger variant="outline" />
  <ModelSelectorContent>
    <ModelSelectorSearch placeholder="Search models..." />
    <ModelSelectorList>
      <ModelSelectorEmpty />
      <ModelSelectorGroup heading="OpenAI">
        {openaiModels.map((model) => (
          <ModelSelectorItem key={model.id} model={model} />
        ))}
      </ModelSelectorGroup>
      <ModelSelectorGroup heading="Anthropic">
        {anthropicModels.map((model) => (
          <ModelSelectorItem key={model.id} model={model} />
        ))}
      </ModelSelectorGroup>
    </ModelSelectorList>
    <ModelSelectorEffort label="Thinking" />
  </ModelSelectorContent>
</ModelSelectorRoot>
```

| Component                   | Description                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------- |
| `ModelSelector`             | Default export with runtime integration                                            |
| `ModelSelector.Root`        | Presentational root (no runtime, controlled state)                                 |
| `ModelSelector.Trigger`     | CVA-styled trigger showing the current selection                                   |
| `ModelSelector.Value`       | Selected model name, icon, and active effort                                       |
| `ModelSelector.Content`     | Popover content wrapping a Command                                                 |
| `ModelSelector.Search`      | Search input that filters the list                                                 |
| `ModelSelector.FocusAnchor` | Visually hidden input that anchors keyboard navigation when there is no search box |
| `ModelSelector.List`        | List of model items (renders all models by default)                                |
| `ModelSelector.Empty`       | Empty state shown when search has no matches                                       |
| `ModelSelector.Group`       | Labeled group of items (e.g. by provider)                                          |
| `ModelSelector.Separator`   | Divider between groups or items                                                    |
| `ModelSelector.Item`        | Individual model option                                                            |
| `ModelSelector.Effort`      | Thinking level row for the selected model                                          |

`ModelSelector.List` is a Command list, so filtering and keyboard navigation work across groups automatically. Custom sorting is plain code: order the models before rendering items.

Keyboard navigation needs a focused input to drive it. When content is unfiltered (`searchable={false}` on `ModelSelector.Content`, or the default children), `ModelSelector.Content` renders a visually hidden `ModelSelector.FocusAnchor` automatically, so custom layouts without a search box stay keyboard-operable. In a custom layout that renders neither `ModelSelector.Search` nor `searchable={false}`, place `ModelSelector.FocusAnchor` yourself to keep the list reachable from the keyboard.

> [!warn]
>
> `ModelSelector.Content` wraps a Command whose root keydown handler claims `Enter` to select the highlighted model and the arrow keys to move through the list. Interactive elements composed inside it (filter chips, custom effort controls) should stop propagation for the keys they handle in their own `onKeyDown` so the focused control responds instead. `ModelSelector.Effort` does this for `Home` / `End` (which cmdk would otherwise use to jump to the first / last model), lets its radiogroup own `ArrowLeft` / `ArrowRight`, and hands `ArrowUp` / `ArrowDown` back to the model list by refocusing cmdk's input, so the highlight only moves while a following `Enter` can act on it.

## Variants

Use the `variant` prop to change the trigger's visual style.

```
<ModelSelector variant="outline" /> // Border (default)
<ModelSelector variant="ghost" />   // No background
<ModelSelector variant="muted" />   // Solid background
```

| Variant   | Description                                  |
| --------- | -------------------------------------------- |
| `outline` | Border with transparent background (default) |
| `ghost`   | No background, subtle hover                  |
| `muted`   | Solid secondary background                   |

## Sizes

Use the `size` prop to control the trigger dimensions.

```
<ModelSelector size="sm" />      // Compact (h-8, text-xs)
<ModelSelector size="default" /> // Standard (h-9)
<ModelSelector size="lg" />      // Large (h-10)
```

## Keyboard Navigation

The picker is fully operable from the keyboard, including when search is disabled.

| Key                        | Action                                                                                                                      |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `ArrowDown` / `ArrowUp`    | Open the popover from the focused trigger; move between models once open, returning focus to the list from the Thinking row |
| `Enter`                    | Select the highlighted model and close                                                                                      |
| `Escape`                   | Close the popover and return focus to the trigger                                                                           |
| `Tab`                      | Move from the model list to the Thinking row                                                                                |
| `ArrowLeft` / `ArrowRight` | Move between reasoning effort levels (Thinking row)                                                                         |
| `Home` / `End`             | Jump to the first / last model (list) or effort level (Thinking row)                                                        |

When `searchable` is set, typing filters the list; otherwise the keys above drive selection directly.

## Accessibility

The picker implements the WAI-ARIA combobox pattern over Popover + Command.

- The trigger is `role="combobox"` with `aria-haspopup="listbox"`; the popover primitive manages `aria-expanded` and `aria-controls`.
- The model list is a Command (cmdk) listbox: each item is `role="option"` with `aria-selected`, and the active item is tracked with `aria-activedescendant`.
- Keyboard navigation works without a visible search box: `ModelSelector.Content` renders a visually hidden input (`ModelSelector.FocusAnchor`) that anchors cmdk's focus so the list stays reachable. Pass `searchable` to surface a real search input instead.
- The Thinking row (`ModelSelector.Effort`) is a `role="radiogroup"` of `role="radio"` toggles with roving tabindex, so it is a single tab stop and `ArrowLeft` / `ArrowRight` move focus and select in one step. `ArrowUp` / `ArrowDown` return focus to the model list.

## How It Works

The default `ModelSelector` export registers the selection with assistant-ui's `ModelContext` system:

1. The component calls `aui.modelContext.register()` with `config.modelName`, plus `config.reasoningEffort` when the selected model supports the chosen level
2. The `AssistantChatTransport` includes `config` in the request body of every chat request
3. Your API route reads `config.modelName` and `config.reasoningEffort`

This works out of the box with `@assistant-ui/react-ai-sdk`. `ModelSelector.Root` performs no registration; it is purely presentational, with controlled and uncontrolled props for the value, effort, and open state.

## API Reference

### ModelSelector

- `models`: `ModelOption[]` — Array of available models to display.
- `defaultValue?`: `string` — Initial model ID for uncontrolled usage. Defaults to the first model, captured on first render; if models loads asynchronously, control the value instead.
- `value?`: `string` — Controlled selected model ID.
- `onValueChange?`: `(value: string) => void` — Callback when selected model changes.
- `defaultEffort?`: `string` — Initial effort level ID for uncontrolled usage.
- `effort?`: `string` — Controlled effort level ID.
- `onEffortChange?`: `(effort: string) => void` — Callback when effort level changes.
- `searchable`: `boolean` (default `false`) — Render a search input above the model list.
- `variant`: `"outline" | "ghost" | "muted"` (default `"outline"`) — Visual style of the trigger button.
- `size`: `"sm" | "default" | "lg"` (default `"default"`) — Size of the trigger button.
- `align`: `"start" | "center" | "end"` (default `"start"`) — Alignment of the dropdown relative to the trigger.
- `contentClassName?`: `string` — Additional class name for the dropdown content.

### ModelOption

- `id`: `string` — Unique identifier sent to the backend as modelName.
- `name`: `string` — Display name shown in trigger and dropdown.
- `description?`: `string` — Optional subtitle shown below the model name.
- `icon?`: `React.ReactNode` — Optional icon displayed before the model name.
- `disabled?`: `boolean` — Disable selection of this model.
- `keywords?`: `string[]` — Extra search terms matched by ModelSelector.Search (e.g. the provider name).
- `efforts?`: `boolean | ModelSelectorEffortOption[]` — Reasoning effort levels. true enables the default Low/Medium/High; pass a custom { id, name } list to override. Omit for models without configurable reasoning.

### `useModelSelectorEfforts`

```
const { efforts, effort, setEffort } = useModelSelectorEfforts();
```

The selected model's effort levels and the active selection, for building a [custom effort UI](#custom-effort-ui) inside `ModelSelector.Content`. `efforts` is `undefined` for models without configurable reasoning.

### `resolveModelEffort`

```
resolveModelEffort(models, modelId, effort); // => string | undefined
```

Returns the effort ID when the given model supports it, otherwise `undefined`. This is the [sticky selection](#sticky-selection) rule the default component applies before registering the selection.

## Related

- [Model Context](/docs/copilots/model-context): How registered context (instructions, tools, config) reaches your backend