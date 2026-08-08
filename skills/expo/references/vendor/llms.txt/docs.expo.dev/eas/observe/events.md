---
modificationDate: July 28, 2026
title: User-defined events
description: Log named events from your app to track custom signals visible in the EAS Observe dashboard.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/observe/events/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/observe/events/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Observe
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/observe/introduction.md)
- [Get started](https://docs.expo.dev/eas/observe/get-started.md)
- [Dashboard](https://docs.expo.dev/eas/observe/dashboard.md)
- [EAS Update](https://docs.expo.dev/eas/observe/eas-update.md)
- [Events](https://docs.expo.dev/eas/observe/events.md) (this page)
- [Configuration](https://docs.expo.dev/eas/observe/configuration.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# User-defined events

Log named events from your app to track custom signals visible in the EAS Observe dashboard.

User-defined events let you record arbitrary, named events from your app. Use them to track any signal specific to your app that the built-in performance metrics do not cover.

Events are persisted on-device, batched, and dispatched on the next flush as OpenTelemetry log records. They appear in the **Events** tab of the EAS Observe dashboard and are queryable from the EAS CLI.

## Log an event

Call `Observe.logEvent` from anywhere in your app:

```tsx
import { Observe } from 'expo-observe';

function handleOnboardingComplete() {
  Observe.logEvent('onboarding.completed');
}
```

The first argument is the event name. Use a stable, dot-separated identifier. The dashboard groups events by exact name.

## Attach attributes

Pass an `attributes` map to record context with the event:

```tsx
Observe.logEvent('report.exported', {
  attributes: {
    format: 'csv',
    rowCount: 1248,
    durationMs: 532,
    filters: ['status:active', 'region:us-west'],
  },
});
```

Supported attribute value types: `string`, `number`, `boolean`, arrays, and nested objects. Other JS values (`Date`, `undefined`, functions) are dropped.

## Severity

Events default to `"info"` severity. Override with the `severity` option for warnings or errors you want to surface separately in the dashboard:

```tsx
Observe.logEvent('sync.failed', {
  severity: 'error',
  attributes: { reason: 'network_timeout' },
});
```

Supported severities, from lowest to highest: `"trace"`, `"debug"`, `"info"`, `"warn"`, `"error"`, `"fatal"`.

## Body

Use `body` for a free-form message that complements the structured attributes:

```tsx
Observe.logEvent('cache.evicted', {
  body: 'Cache evicted because disk pressure exceeded the configured threshold.',
  severity: 'warn',
  attributes: { evictedItemCount: 42, freedBytes: 1048576 },
});
```

## Naming conventions

-   Use lowercase, dot-separated names: `task.completed`, `onboarding.skipped`, `report.exported`.
-   Pick a vocabulary and stick to it. The dashboard groups by exact event name, so `report_exported` and `report.exported` will appear as separate rows.
-   Avoid Personally Identifiable Information (PII) in event names, attribute keys, and attribute values. Everything you pass is visible in the dashboard and is dispatched off-device.

## Display name

Use `displayName` to provide a human-friendly label for the event in the dashboard:

```tsx
Observe.logEvent('onboarding.completed', {
  displayName: 'Onboarding completed',
});
```

> **Note:** The display name that you set will only be used in the session timeline view.

## View events

In the dashboard: open your project and navigate to [**Observe > Events**](https://expo.dev/accounts/%5Baccount%5D/projects/%5Bproject%5D/observe/events). The default view lists distinct event names with their counts in the selected time range. Click an event name to see individual events with their timestamps, attributes, and the session they belong to.

From the CLI:

```sh
eas observe:events
eas observe:events report.exported
eas observe:events --all-events --json
```

Run `eas observe:events --help` for the full list of flags (time range, platform, session ID, and more).
