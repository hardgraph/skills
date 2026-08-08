---
modificationDate: July 29, 2026
title: Integrate a third-party package with EAS Observe
description: Learn how to add an optional EAS Observe integration to a third-party package.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/observe/integrations/third-party/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/observe/integrations/third-party/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Observe > Integrations
Pages in this section:
- [Expo Router](https://docs.expo.dev/eas/observe/integrations/expo-router.md)
- [React Navigation](https://docs.expo.dev/eas/observe/integrations/react-navigation.md)
- [Third-party](https://docs.expo.dev/eas/observe/integrations/third-party.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Integrate a third-party package with EAS Observe

Learn how to add an optional EAS Observe integration to a third-party package.

Third-party packages can integrate with EAS Observe to send events that help developers identify performance and usage issues. These are often problems that are difficult to detect from application code alone.

Events should describe actionable issues that developers can fix. For example, a package might report:

-   An image that is much larger than the device's screen
-   A background task that takes too long to complete
-   A native resource that loads slowly

## Prerequisites

#### Prerequisites

##### Expo SDK 57 and later

Third-party integrations require SDK 57 and later to access `Observe.registerIntegration()`.

##### An app already using EAS Observe

Follow [Get started](/eas/observe/get-started.md) to install `expo-observe` and create your first build.

## Add `expo-observe` as an optional peer dependency

Add `expo-observe` as an optional peer dependency so your package continues to work when it is not installed. Also, add it as a development dependency for TypeScript types and tests. Do not add it as a required runtime dependency.

```json
{
  "peerDependencies": {
    "expo-observe": ">=57.0.0"
  },
  "peerDependenciesMeta": {
    "expo-observe": {
      "optional": true
    }
  },
  "devDependencies": {
    "expo-observe": "^57.0.0"
  }
}
```

Load the package with `require()` inside a `try/catch` block. Use `typeof import()` to preserve its TypeScript types:

```ts
let observeModule: typeof import('expo-observe') | undefined;

try {
  observeModule = require('expo-observe') as typeof import('expo-observe');
} catch {
  // The integration remains disabled when expo-observe is not installed.
}
```

## Declare the integration configuration

Use declaration merging to add your package's integration key to `expo-observe`:

```ts
export type YourPackageIntegrationConfig = {
  thresholdMs?: number;
};

declare module 'expo-observe' {
  interface ObserveIntegrationsConfig {
    'your-package'?: boolean | YourPackageIntegrationConfig;
  }
}
```

Export this declaration from your package entry point so TypeScript loads it when users import your package.

Developers using your package can then enable the integration with `Observe.configure()`:

```tsx
import { Observe } from 'expo-observe';

Observe.configure({
  integrations: {
    'your-package': true,
  },
});
```

If the integration accepts options, developers can pass a configuration object instead of `true`:

```tsx
Observe.configure({
  integrations: {
    'your-package': {
      thresholdMs: 1500,
    },
  },
});
```

## Register the integration

Call `Observe.registerIntegration()` with your integration key. The callback receives the configuration for your integration:

```ts
export function initObserveIntegration() {
  // The `typeof window` check skips initialization during server-side rendering on web.
  if (typeof window !== 'undefined' && observeModule) {
    const { Observe } = observeModule;

    Observe.registerIntegration('your-package', config => {
      if (config) {
        enableObserveIntegration(config === true ? {} : config);
      }
    });
  }
}

let enabled = false;

function enableObserveIntegration() {
  // Initialize the integration here
  // For example:
  enabled = true;
}
```

Implement `enableObserveIntegration()` in your package. The callback does not run when the integration is omitted or set to `false`.

Call the initialization function from your package entry point:

```ts
import { initObserveIntegration } from './observe';

export type { YourPackageIntegrationConfig } from './observe.types';

initObserveIntegration();
```

## Log events

Call `Observe.logEvent()` when your package detects an actionable issue. Use a lowercase event name with the package name as the first segment. Separate segments with periods:

```ts
export function logExpensiveOperation(durationMs: number, thresholdMs: number) {
  if (!observeModule || !enabled) {
    return;
  }

  const { Observe } = observeModule;

  Observe.logEvent('your-package.expensive-operation', {
    severity: 'warn',
    body: 'Reduce the work performed by this operation or increase the configured threshold.',
    attributes: {
      durationMs,
      thresholdMs,
    },
  });
}
```

For more information about naming events and adding details, see [User-defined events](/eas/observe/events.md).
