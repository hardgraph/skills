# Tabs
URL: /docs/ui/tabs

A multi-variant tabs component for organizing content into switchable panels.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

> [!info]
>
> This is a **standalone component** that does not depend on the assistant-ui runtime. Use it anywhere in your application.

\[interactive preview omitted]

## Installation

With the style-aware registry configured in components.json ("@assistant-ui": "https\://r.assistant-ui.com/styles/{style}/{name}.json"), the flavor resolves from the project style automatically:

```bash
npx shadcn@latest add @assistant-ui/tabs
```

Or add by direct URL without registry configuration:

```bash
npx shadcn@latest add https://r.assistant-ui.com/base/tabs.json
```

Or install manually:

```bash
npm install @base-ui/react class-variance-authority
```

Then copy these source files from GitHub:

- [components/assistant-ui/tabs.tsx](https://github.com/assistant-ui/assistant-ui/blob/main/packages/ui/src/components/assistant-ui/tabs.tsx)

```bash
curl -sSL --create-dirs \
  -o components/assistant-ui/tabs.tsx https://raw.githubusercontent.com/assistant-ui/assistant-ui/main/packages/ui/src/components/assistant-ui/tabs.tsx
```

## Usage

```
import {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContent,
} from "@/components/assistant-ui/tabs";

export function Example() {
  return (
    <Tabs defaultValue="account">
      <TabsList>
        <TabsTrigger value="account">Account</TabsTrigger>
        <TabsTrigger value="password">Password</TabsTrigger>
      </TabsList>
      <TabsContent value="account">Account settings here.</TabsContent>
      <TabsContent value="password">Password settings here.</TabsContent>
    </Tabs>
  );
}
```

## Examples

### Variants

Use the `variant` prop on `TabsList` to change the visual style. Child components inherit the variant automatically.

\[interactive preview omitted]

```
<TabsList variant="default" />  // Muted background with shadow (default)
<TabsList variant="line" />     // Underline indicator
<TabsList variant="ghost" />    // Transparent with hover states
<TabsList variant="pills" />    // Rounded pill buttons
<TabsList variant="outline" />  // Border with background on active
```

### Sizes

Use the `size` prop on `TabsList` to change the tab height. Child components inherit the size automatically.

\[interactive preview omitted]

```
<TabsList size="sm" />      // 32px height
<TabsList size="default" /> // 36px height
<TabsList size="lg" />      // 40px height
```

### With Icons

Tabs automatically style SVG icons placed inside triggers.

\[interactive preview component TabsWithIconsSample omitted]

Code for TabsWithIconsSample preview:

```tsx
import { FileText, Settings, User, Bell, Lock } from "lucide-react";
import {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContent,
} from "@/components/assistant-ui/tabs";

function TabsWithIconsSample() {
  return (
    <Tabs defaultValue="profile" className="w-[400px]">
      <TabsList variant="ghost">
        <TabsTrigger value="profile">
          <User />
          Profile
        </TabsTrigger>
        <TabsTrigger value="notifications">
          <Bell />
          Notifications
        </TabsTrigger>
        <TabsTrigger value="security">
          <Lock />
          Security
        </TabsTrigger>
      </TabsList>
      <TabsContent value="profile" className="p-4">
        <p className="text-muted-foreground text-sm">
          Edit your profile information.
        </p>
      </TabsContent>
      <TabsContent value="notifications" className="p-4">
        <p className="text-muted-foreground text-sm">
          Manage your notification preferences.
        </p>
      </TabsContent>
      <TabsContent value="security" className="p-4">
        <p className="text-muted-foreground text-sm">
          Configure security and privacy settings.
        </p>
      </TabsContent>
    </Tabs>
  );
}
```

### Controlled

Use `value` and `onValueChange` for controlled tab state.

\[interactive preview component TabsControlledSample omitted]

Code for TabsControlledSample preview:

```tsx
import { useState } from "react";
import {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContent,
} from "@/components/assistant-ui/tabs";

function TabsControlledSample() {
  const [activeTab, setActiveTab] = useState("overview");

  return (
    <Tabs
      value={activeTab}
      onValueChange={setActiveTab}
      className="w-[400px]"
    >
      <TabsList variant="pills">
        <TabsTrigger value="overview">Overview</TabsTrigger>
        <TabsTrigger value="analytics">Analytics</TabsTrigger>
        <TabsTrigger value="reports">Reports</TabsTrigger>
      </TabsList>
      <TabsContent value="overview" className="p-4">
        <p className="text-muted-foreground text-sm">Overview content</p>
      </TabsContent>
      <TabsContent value="analytics" className="p-4">
        <p className="text-muted-foreground text-sm">Analytics content</p>
      </TabsContent>
      <TabsContent value="reports" className="p-4">
        <p className="text-muted-foreground text-sm">Reports content</p>
      </TabsContent>
    </Tabs>
    <p className="text-muted-foreground text-sm">
      Current tab: <code className="font-mono">{activeTab}</code>
    </p>
  );
}
```

### As Link

Compose with `render` (Base UI) or `asChild` (Radix) on `TabsTrigger` to render as a different element, like a navigation link.

\[interactive preview component TabsAsLinkSample omitted]

Code for TabsAsLinkSample preview:

```tsx
import { FileText, Settings, User, Bell, Lock } from "lucide-react";
import {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContent,
} from "@/components/assistant-ui/tabs";

function TabsAsLinkSample() {
  return (
    <Tabs defaultValue="docs">
      <TabsList variant="line">
        <TabsTrigger value="docs" render={<a href="#installation" />}>
          <FileText />
          Docs
        </TabsTrigger>
        <TabsTrigger value="api" render={<a href="#api-reference" />}>
          <Settings />
          API
        </TabsTrigger>
      </TabsList>
    </Tabs>
  );
}
```

### Animated Indicator

All variants feature smooth animated indicators that slide between tabs:

| Variant   | Indicator Style                      |
| --------- | ------------------------------------ |
| `default` | Sliding background with shadow       |
| `line`    | Sliding underline                    |
| `ghost`   | Sliding background with hover effect |
| `pills`   | Sliding pill background              |
| `outline` | Sliding border                       |

\[interactive preview component TabsAnimatedIndicatorSample omitted]

Code for TabsAnimatedIndicatorSample preview:

```tsx
import {
  Tabs,
  TabsList,
  TabsTrigger,
  TabsContent,
} from "@/components/assistant-ui/tabs";

function TabsAnimatedIndicatorSample() {
  return (
    <div className="flex flex-col gap-2">
      <span className="text-muted-foreground text-xs">
        Default - Sliding background
      </span>
      <Tabs defaultValue="home">
        <TabsList variant="default">
          <TabsTrigger value="home">Home</TabsTrigger>
          <TabsTrigger value="about">About</TabsTrigger>
          <TabsTrigger value="services">Services</TabsTrigger>
          <TabsTrigger value="contact">Contact</TabsTrigger>
        </TabsList>
      </Tabs>
    </div>
    <div className="flex flex-col gap-2">
      <span className="text-muted-foreground text-xs">
        Line - Sliding underline
      </span>
      <Tabs defaultValue="home">
        <TabsList variant="line">
          <TabsTrigger value="home">Home</TabsTrigger>
          <TabsTrigger value="about">About</TabsTrigger>
          <TabsTrigger value="services">Services</TabsTrigger>
          <TabsTrigger value="contact">Contact</TabsTrigger>
        </TabsList>
      </Tabs>
    </div>
    <div className="flex flex-col gap-2">
      <span className="text-muted-foreground text-xs">
        Ghost - Sliding background with hover effect
      </span>
      <Tabs defaultValue="dashboard">
        <TabsList variant="ghost">
          <TabsTrigger value="dashboard">Dashboard</TabsTrigger>
          <TabsTrigger value="projects">Projects</TabsTrigger>
          <TabsTrigger value="tasks">Tasks</TabsTrigger>
          <TabsTrigger value="team">Team</TabsTrigger>
        </TabsList>
      </Tabs>
    </div>
    <div className="flex flex-col gap-2">
      <span className="text-muted-foreground text-xs">
        Pills - Sliding pill background
      </span>
      <Tabs defaultValue="all">
        <TabsList variant="pills">
          <TabsTrigger value="all">All</TabsTrigger>
          <TabsTrigger value="active">Active</TabsTrigger>
          <TabsTrigger value="completed">Completed</TabsTrigger>
          <TabsTrigger value="archived">Archived</TabsTrigger>
        </TabsList>
      </Tabs>
    </div>
    <div className="flex flex-col gap-2">
      <span className="text-muted-foreground text-xs">
        Outline - Sliding border
      </span>
      <Tabs defaultValue="week">
        <TabsList variant="outline">
          <TabsTrigger value="day">Day</TabsTrigger>
          <TabsTrigger value="week">Week</TabsTrigger>
          <TabsTrigger value="month">Month</TabsTrigger>
          <TabsTrigger value="year">Year</TabsTrigger>
        </TabsList>
      </Tabs>
    </div>
  );
}
```

## API Reference

### Composable API

| Component     | Description                                                    |
| ------------- | -------------------------------------------------------------- |
| `Tabs`        | The root component that manages tab state.                     |
| `TabsList`    | The container for tab triggers. Set `variant` and `size` here. |
| `TabsTrigger` | An individual tab button. Inherits variant/size from TabsList. |
| `TabsContent` | The content panel for a tab.                                   |

### Tabs

The root component that manages tab state.

- `defaultValue?`: `string` — The default active tab value (uncontrolled).
- `value?`: `string` — The controlled active tab value.
- `onValueChange?`: `(value: string) => void` — Callback when the active tab changes.
- `className?`: `string` — Additional CSS classes.

### TabsList

The container for tab triggers. Set `variant` and `size` here to style all child components.

- `variant`: `"default" | "line" | "ghost" | "pills" | "outline"` (default `"default"`) — The visual style of the tabs. Child components inherit this automatically.
- `size`: `"sm" | "default" | "lg"` (default `"default"`) — The size of the tabs. Child components inherit this automatically.
- `className?`: `string` — Additional CSS classes.

### TabsTrigger

An individual tab button.

- `value`: `string` — The unique value for this tab.
- `render?`: `ReactElement | function` — Base UI: compose as a different element instead of rendering a button.
- `asChild`: `boolean` (default `false`) — Radix: merge props with a child element instead of rendering a button.
- `disabled?`: `boolean` — Whether the tab is disabled.
- `className?`: `string` — Additional CSS classes.

### TabsContent

The content panel for a tab.

- `value`: `string` — The value matching the corresponding TabsTrigger.
- `className?`: `string` — Additional CSS classes.

### Style Variants (CVA)

| Export                        | Description                               |
| ----------------------------- | ----------------------------------------- |
| `tabsListVariants`            | Styles for the tabs list container.       |
| `tabsActiveIndicatorVariants` | Styles for the animated active indicator. |

```
import {
  tabsListVariants,
  tabsActiveIndicatorVariants,
} from "@/components/assistant-ui/tabs";

<div className={tabsListVariants({ variant: "ghost", size: "sm" })}>
  Custom Tabs Container
</div>
```