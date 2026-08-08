# Select
URL: /docs/ui/select

A dropdown select component with composable sub-components.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime. Use it anywhere in your application.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/select
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/select.json
```

Or install manually:

```bash
npm install @base-ui/react class-variance-authority
```

Then copy these source files from GitHub:

- [components/assistant-ui/select.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/select.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/select.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/select.tsx
```

## Usage

```
import { Select } from "@/components/assistant-ui/select";

const options = [
  { value: "apple", label: "Apple" },
  { value: "banana", label: "Banana" },
  { value: "orange", label: "Orange" },
];

export function FruitPicker() {
  const [value, setValue] = useState("apple");

  return (
    <Select
      value={value}
      onValueChange={setValue}
      options={options}
      placeholder="Select a fruit..."
    />
  );
}
```

## Examples

### Variants

Use the `variant` prop on `SelectTrigger` to change the visual style.

\[interactive preview omitted]

```
<SelectTrigger variant="outline" /> // Border (default)
<SelectTrigger variant="ghost" />   // No border
<SelectTrigger variant="muted" />   // Solid background
```

### Sizes

Use the `size` prop on `SelectTrigger` to change the height.

\[interactive preview omitted]

```
<SelectTrigger size="default" /> // 36px (default)
<SelectTrigger size="sm" />      // 32px
```

### Scrollable

Long lists automatically become scrollable.

\[interactive preview component SelectScrollableSample omitted]

Code for SelectScrollableSample preview:

```tsx
import { useState } from "react";
import {
  Select,
  SelectRoot,
  SelectTrigger,
  SelectContent,
  SelectItem,
  SelectGroup,
  SelectLabel,
  SelectValue,
  SelectSeparator,
} from "@/components/assistant-ui/select";

function SelectScrollableSample() {
  const [value, setValue] = useState("est");

  return (
    <SelectRoot
      value={value}
      onValueChange={(value) => value !== null && setValue(value)}
      items={[...northAmericaTimezones, ...europeTimezones, ...asiaTimezones]}
    >
      <SelectTrigger className="w-64">
        <SelectValue placeholder="Select a timezone..." />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectLabel>North America</SelectLabel>
          {northAmericaTimezones.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
        <SelectGroup>
          <SelectLabel>Europe</SelectLabel>
          {europeTimezones.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
        <SelectGroup>
          <SelectLabel>Asia</SelectLabel>
          {asiaTimezones.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </SelectRoot>
  );
}
```

### Groups

Use the composable API for grouped options:

\[interactive preview component SelectGroupsSample omitted]

Code for SelectGroupsSample preview:

```tsx
import { useState } from "react";
import {
  Select,
  SelectRoot,
  SelectTrigger,
  SelectContent,
  SelectItem,
  SelectGroup,
  SelectLabel,
  SelectValue,
  SelectSeparator,
} from "@/components/assistant-ui/select";

function SelectGroupsSample() {
  const [value, setValue] = useState("react");

  return (
    <SelectRoot
      value={value}
      onValueChange={(value) => value !== null && setValue(value)}
      items={[...frameworks, ...backends]}
    >
      <SelectTrigger className="w-48">
        <SelectValue placeholder="Select a framework..." />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectLabel>Frontend</SelectLabel>
          {frameworks.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
        <SelectSeparator />
        <SelectGroup>
          <SelectLabel>Backend</SelectLabel>
          {backends.map((item) => (
            <SelectItem key={item.value} value={item.value}>
              {item.label}
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </SelectRoot>
  );
}
```

### Disabled Items

\[interactive preview component SelectDisabledItemsSample omitted]

Code for SelectDisabledItemsSample preview:

```tsx
import { useState } from "react";
import {
  Select,
  SelectRoot,
  SelectTrigger,
  SelectContent,
  SelectItem,
  SelectGroup,
  SelectLabel,
  SelectValue,
  SelectSeparator,
} from "@/components/assistant-ui/select";

function SelectDisabledItemsSample() {
  const [value, setValue] = useState("free");

  return (
    <Select
      value={value}
      onValueChange={setValue}
      options={[
        { value: "free", label: "Free" },
        { value: "pro", label: "Pro" },
        { value: "enterprise", label: "Enterprise", disabled: true },
      ]}
    />
  );
}
```

### With Placeholder

\[interactive preview component SelectPlaceholderSample omitted]

Code for SelectPlaceholderSample preview:

```tsx
import { useState } from "react";
import {
  Select,
  SelectRoot,
  SelectTrigger,
  SelectContent,
  SelectItem,
  SelectGroup,
  SelectLabel,
  SelectValue,
  SelectSeparator,
} from "@/components/assistant-ui/select";

function SelectPlaceholderSample() {
  const [value, setValue] = useState("");

  return (
    <Select
      value={value}
      onValueChange={setValue}
      options={fruits}
      placeholder="Choose an option..."
    />
  );
}
```

### Disabled Select

\[interactive preview component SelectDisabledSample omitted]

Code for SelectDisabledSample preview:

```tsx
import { useState } from "react";
import {
  Select,
  SelectRoot,
  SelectTrigger,
  SelectContent,
  SelectItem,
  SelectGroup,
  SelectLabel,
  SelectValue,
  SelectSeparator,
} from "@/components/assistant-ui/select";

function SelectDisabledSample() {
  const [value, setValue] = useState("apple");

  return (
    <Select
      value={value}
      onValueChange={setValue}
      options={fruits}
      disabled
    />
  );
}
```

## API Reference

### Composable API

| Component                | Description                                                             |
| ------------------------ | ----------------------------------------------------------------------- |
| `Select`                 | A convenience component that renders a complete select with options.    |
| `SelectRoot`             | The root component that manages state.                                  |
| `SelectTrigger`          | The button that opens the dropdown. Accepts `variant` and `size` props. |
| `SelectValue`            | Renders the selected value or placeholder.                              |
| `SelectContent`          | The dropdown content container with animations.                         |
| `SelectItem`             | An individual selectable item.                                          |
| `SelectGroup`            | Groups related items together.                                          |
| `SelectLabel`            | A label for a group of items.                                           |
| `SelectSeparator`        | A visual separator between items or groups.                             |
| `SelectScrollUpButton`   | Scroll indicator for long lists.                                        |
| `SelectScrollDownButton` | Scroll indicator for long lists.                                        |

With the Base UI flavor, `SelectValue` resolves the trigger label from the `items` prop on `SelectRoot`; pass the same array you map into `SelectItem` children. The Radix flavor reads the label from the selected item's text automatically.

### Select

A convenience component that renders a complete select with options.

- `value`: `string` — The controlled value of the select.
- `onValueChange`: `(value: string) => void` — Callback when the selected value changes.
- `options`: `SelectOption[]` — Array of options to display.
- `placeholder?`: `string` — Placeholder text when no value is selected.
- `className?`: `string` — Additional CSS classes for the trigger.
- `disabled?`: `boolean` — Whether the select is disabled.

### SelectOption

- `value`: `string` — The value of the option.
- `label`: `ReactNode` — The display label for the option.
- `textValue?`: `string` — Optional text value for typeahead. Defaults to label if it's a string.
- `disabled?`: `boolean` — Whether the option is disabled.

### SelectTrigger

The button that opens the dropdown.

- `variant`: `"outline" | "ghost" | "muted"` (default `"outline"`) — The visual style of the trigger.
- `size`: `"sm" | "default" | "lg"` (default `"default"`) — The size of the trigger.
- `className?`: `string` — Additional CSS classes.

### Style Variants (CVA)

| Export                  | Description                           |
| ----------------------- | ------------------------------------- |
| `selectTriggerVariants` | Styles for the select trigger button. |

```
import { selectTriggerVariants } from "@/components/assistant-ui/select";

<button className={selectTriggerVariants({ variant: "ghost", size: "sm" })}>
  Custom Trigger
</button>
```