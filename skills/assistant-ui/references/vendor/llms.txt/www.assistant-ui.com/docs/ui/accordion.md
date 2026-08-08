# Accordion
URL: /docs/ui/accordion

A vertically stacked set of interactive headings that reveal or hide content sections.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime. Use it anywhere in your application.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/accordion
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/accordion.json
```

Or install manually:

```bash
npm install @base-ui/react class-variance-authority
```

Then copy these source files from GitHub:

- [components/assistant-ui/accordion.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/accordion.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/accordion.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/accordion.tsx
```

## Usage

```
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
} from "@/components/assistant-ui/accordion";

export function Example() {
  return (
    <Accordion type="single" collapsible>
      <AccordionItem value="item-1">
        <AccordionTrigger>Section 1</AccordionTrigger>
        <AccordionContent>Content for section 1.</AccordionContent>
      </AccordionItem>
      <AccordionItem value="item-2">
        <AccordionTrigger>Section 2</AccordionTrigger>
        <AccordionContent>Content for section 2.</AccordionContent>
      </AccordionItem>
    </Accordion>
  );
}
```

## Examples

### Variants

Use the `variant` prop on `Accordion` to change the visual style. Child components inherit the variant automatically.

\[interactive preview omitted]

```
// Default - border-bottom separator
<Accordion type="single" collapsible variant="default">
  <AccordionItem value="item-1">
    <AccordionTrigger>...</AccordionTrigger>
    <AccordionContent>...</AccordionContent>
  </AccordionItem>
</Accordion>

// Outline - bordered container
<Accordion type="single" collapsible variant="outline">
  <AccordionItem value="item-1">
    <AccordionTrigger>...</AccordionTrigger>
    <AccordionContent>...</AccordionContent>
  </AccordionItem>
</Accordion>

// Ghost - separated cards
<Accordion type="single" collapsible variant="ghost">
  <AccordionItem value="item-1">
    <AccordionTrigger>...</AccordionTrigger>
    <AccordionContent>...</AccordionContent>
  </AccordionItem>
</Accordion>
```

### Multiple Items Open

Use `type="multiple"` to allow multiple items to be open simultaneously.

\[interactive preview omitted]

```
<Accordion type="multiple">
  <AccordionItem value="item-1">
    <AccordionTrigger>First Section</AccordionTrigger>
    <AccordionContent>Content 1</AccordionContent>
  </AccordionItem>
  <AccordionItem value="item-2">
    <AccordionTrigger>Second Section</AccordionTrigger>
    <AccordionContent>Content 2</AccordionContent>
  </AccordionItem>
</Accordion>
```

### With Icons

Add icons or custom elements inside the trigger.

\[interactive preview component AccordionWithIconsSample omitted]

Code for AccordionWithIconsSample preview:

```tsx
import { CreditCard, Settings, User, HelpCircle } from "lucide-react";
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
} from "@/components/assistant-ui/accordion";

function AccordionWithIconsSample() {
  return (
    <Accordion variant="outline" className="w-[400px]">
      <AccordionItem value="account">
        <AccordionTrigger>
          <span className="flex items-center gap-2">
            <User className="size-4" />
            Account Settings
          </span>
        </AccordionTrigger>
        <AccordionContent>
          Manage your account details, profile picture, and personal
          information.
        </AccordionContent>
      </AccordionItem>
      <AccordionItem value="billing">
        <AccordionTrigger>
          <span className="flex items-center gap-2">
            <CreditCard className="size-4" />
            Billing
          </span>
        </AccordionTrigger>
        <AccordionContent>
          View your billing history, manage payment methods, and update
          subscription.
        </AccordionContent>
      </AccordionItem>
      <AccordionItem value="preferences">
        <AccordionTrigger>
          <span className="flex items-center gap-2">
            <Settings className="size-4" />
            Preferences
          </span>
        </AccordionTrigger>
        <AccordionContent>
          Customize your experience with notification and display settings.
        </AccordionContent>
      </AccordionItem>
    </Accordion>
  );
}
```

### Controlled

Use `value` and `onValueChange` for controlled accordion state.

\[interactive preview component AccordionControlledSample omitted]

Code for AccordionControlledSample preview:

```tsx
import { useState } from "react";
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
} from "@/components/assistant-ui/accordion";

function AccordionControlledSample() {
  const [value, setValue] = useState<string[]>(["item-1"]);

  return (
    <Accordion
      value={value}
      onValueChange={(next) => setValue(next as string[])}
      className="w-[400px]"
    >
      <AccordionItem value="item-1">
        <AccordionTrigger>Overview</AccordionTrigger>
        <AccordionContent>
          This is the overview section content.
        </AccordionContent>
      </AccordionItem>
      <AccordionItem value="item-2">
        <AccordionTrigger>Details</AccordionTrigger>
        <AccordionContent>
          This is the details section content.
        </AccordionContent>
      </AccordionItem>
      <AccordionItem value="item-3">
        <AccordionTrigger>Advanced</AccordionTrigger>
        <AccordionContent>
          This is the advanced section content.
        </AccordionContent>
      </AccordionItem>
    </Accordion>
    <p className="text-muted-foreground text-sm">
      Current value: <code className="font-mono">{value ?? "none"}</code>
    </p>
  );
}
```

### FAQ Section

A practical example of using accordion for a FAQ section.

\[interactive preview component AccordionFAQSample omitted]

Code for AccordionFAQSample preview:

```tsx
import { CreditCard, Settings, User, HelpCircle } from "lucide-react";
import {
  Accordion,
  AccordionItem,
  AccordionTrigger,
  AccordionContent,
} from "@/components/assistant-ui/accordion";

function AccordionFAQSample() {
  return (
    <div className="w-[500px]">
      <div className="mb-4 flex items-center gap-2">
        <HelpCircle className="size-5" />
        <h3 className="text-lg font-semibold">Frequently Asked Questions</h3>
      </div>
      <Accordion>
        <AccordionItem value="faq-1">
          <AccordionTrigger>
            What payment methods do you accept?
          </AccordionTrigger>
          <AccordionContent>
            We accept all major credit cards (Visa, MasterCard, American
            Express), PayPal, and bank transfers for annual subscriptions.
          </AccordionContent>
        </AccordionItem>
        <AccordionItem value="faq-2">
          <AccordionTrigger>
            Can I cancel my subscription anytime?
          </AccordionTrigger>
          <AccordionContent>
            Yes, you can cancel your subscription at any time. Your access
            will continue until the end of your current billing period.
          </AccordionContent>
        </AccordionItem>
        <AccordionItem value="faq-3">
          <AccordionTrigger>Do you offer refunds?</AccordionTrigger>
          <AccordionContent>
            We offer a 30-day money-back guarantee for all new subscriptions.
            Contact our support team to request a refund.
          </AccordionContent>
        </AccordionItem>
        <AccordionItem value="faq-4">
          <AccordionTrigger>How do I contact support?</AccordionTrigger>
          <AccordionContent>
            You can reach our support team via email at support@example.com or
            through the live chat feature in the bottom right corner.
          </AccordionContent>
        </AccordionItem>
      </Accordion>
    </div>
  );
}
```

## API Reference

### Composable API

| Component          | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| `Accordion`        | The root component that manages accordion state and variant. |
| `AccordionItem`    | A single collapsible section container.                      |
| `AccordionTrigger` | The clickable header that toggles content visibility.        |
| `AccordionContent` | The collapsible content panel.                               |

### Accordion

The root component that manages accordion state. Set `variant` here to style all child components.

- `type`: `"single" | "multiple"` — Whether only one or multiple items can be open at once.
- `collapsible`: `boolean` (default `false`) — When type is 'single', allows closing the open item by clicking it again.
- `defaultValue?`: `string | string[]` — The default open item(s) (uncontrolled).
- `value?`: `string | string[]` — The controlled open item(s).
- `onValueChange?`: `(value: string | string[]) => void` — Callback when the open item(s) change.
- `variant`: `"default" | "outline" | "ghost"` (default `"default"`) — The visual style of the accordion. Child components inherit this automatically.
- `className?`: `string` — Additional CSS classes.

### AccordionItem

A single collapsible section container.

- `value`: `string` — A unique identifier for this item.
- `disabled?`: `boolean` — Whether the item is disabled.
- `className?`: `string` — Additional CSS classes.

### AccordionTrigger

The clickable header that toggles content visibility.

- `className?`: `string` — Additional CSS classes.

### AccordionContent

The collapsible content panel.

- `className?`: `string` — Additional CSS classes.

### Style Variants (CVA)

| Export              | Description                         |
| ------------------- | ----------------------------------- |
| `accordionVariants` | Styles for the accordion container. |

```
import { accordionVariants } from "@/components/assistant-ui/accordion";

<div className={accordionVariants({ variant: "outline" })}>
  Custom Accordion Container
</div>
```