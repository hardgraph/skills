---
modificationDate: July 29, 2026
title: Configure EAS Observe
description: Control how EAS Observe collects and dispatches metrics, including environment settings, development mode, and custom endpoints.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/observe/configuration/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/observe/configuration/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Observe
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/observe/introduction.md)
- [Get started](https://docs.expo.dev/eas/observe/get-started.md)
- [Dashboard](https://docs.expo.dev/eas/observe/dashboard.md)
- [EAS Update](https://docs.expo.dev/eas/observe/eas-update.md)
- [Events](https://docs.expo.dev/eas/observe/events.md)
- [Configuration](https://docs.expo.dev/eas/observe/configuration.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Configure EAS Observe

Control how EAS Observe collects and dispatches metrics, including environment settings, development mode, and custom endpoints.

Configure EAS Observe at runtime to fit your app's build setup, environment, and data routing. This page covers the `configure()` and `dispatchEvents()`, enabling metrics in development, using a custom endpoint, and separating data by environment.

## `configure()`

Use the `configure()` method to control how EAS Observe behaves at runtime:

```tsx
import { Observe } from 'expo-observe';

Observe.configure({
  environment: 'production',
  dispatchingEnabled: true,
});
```

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `environment` | `string` | `process.env.NODE_ENV` | The environment label for observability events |
| `dispatchingEnabled` | `boolean` | `true` | Whether to send collected events to the server |
| `dispatchInDebug` | `boolean` | `false` | Whether to dispatch metrics that were collected in a debug build. Has no effect on release builds. |
| `sampleRate` | `number` | `undefined` | Fraction of installations that dispatch metrics, in `[0, 1]`. See [Sampling](/eas/observe/configuration.md#sampling). |
| `integrations` | `object` | `undefined` | Opt-in settings for EAS Observe integrations. See [Expo Router](/eas/observe/integrations/expo-router.md) and [React Navigation](/eas/observe/integrations/react-navigation.md), or [integrate your own package](/eas/observe/integrations/third-party.md). |

## `dispatchEvents()`

Events are automatically dispatched when the app moves to the background. On Android, a background worker dispatches events once network connectivity is available. On iOS, this happens when the app resigns active state or is about to terminate.

To flush events manually (for example, during testing or to ensure events are sent before a specific point), call `dispatchEvents()`:

```tsx
import { Observe } from 'expo-observe';

await Observe.dispatchEvents();
```

## Sampling

By default, every installation dispatches its metrics. For high-volume apps, you can sample a fraction of installations by setting `sampleRate` to a value between `0` and `1`:

```tsx
import { Observe } from 'expo-observe';

// Dispatch metrics from ~25% of installations.
Observe.configure({
  sampleRate: 0.25,
});
```

The sampling decision is **deterministic per installation**. Each installation is either permanently in-sample or out-of-sample for a given rate, so the choice is stable across app launches and you get a consistent slice of installations rather than a random subset of sessions.

A few details worth knowing:

-   Values outside the `[0, 1]` range are clamped to the nearest edge. `0` always drops; `1` always dispatches.
-   Out-of-sample devices drop pending metrics rather than accumulating them. Lowering the rate later does not retroactively send earlier sessions.
-   Sampling depends on [`dispatchingEnabled`](/eas/observe/configuration.md#configure). If `dispatchingEnabled` is `false`, nothing is dispatched regardless of `sampleRate`.

## Enable metrics in development

By default, metrics collected from debug builds are not dispatched. To dispatch them anyway (for example, while testing your EAS Observe integration), set `dispatchInDebug` to `true` when calling `configure()`:

```tsx
import { Observe } from 'expo-observe';

Observe.configure({
  dispatchInDebug: true,
});
```

A build is treated as a debug build if either the native app is a debug build or the JS bundle is a development bundle (`__DEV__` is `true`). This detection is independent of the `environment` value (see [Environments](/eas/observe/configuration.md#environments)).

`dispatchInDebug` has no effect on release builds, which always dispatch (subject to [`dispatchingEnabled`](/eas/observe/configuration.md#configure) and [`sampleRate`](/eas/observe/configuration.md#sampling)). If `dispatchingEnabled` is `false` or this installation is out-of-sample, nothing dispatches regardless of `dispatchInDebug`.

> Enable this only when testing your EAS Observe integration. Development/debug performance differs significantly from production, so collecting development/debug metrics may distort the results shown in your dashboard.

## Custom endpoint

EAS Observe sends data using the OpenTelemetry Protocol (OTLP) over HTTP with a JSON payload. It posts metrics to `<endpointUrl>/<project-id>/v1/metrics` and logs to `<endpointUrl>/<project-id>/v1/logs`, where `<project-id>` is your EAS project ID. This means you can set `endpointUrl` to an OpenTelemetry-compatible backend or an OpenTelemetry Collector to route your observability data there.

To change the endpoint, set the `endpointUrl` value in your **app config**:

```json
{
  "expo": {
    "extra": {
      "eas": {
        "observe": {
          "endpointUrl": "https://your-custom-endpoint.com"
        }
      }
    }
  }
}
```

The endpoint URL is baked into the native layer of the app at build time, so changing it requires regenerating native code. After updating your app config, run `npx expo prebuild` and create a new build to apply the change.

The path includes your project ID before the standard `/v1/metrics` and `/v1/logs` OTLP paths. If your backend expects the standard OTLP path without that prefix, send the data to an OpenTelemetry Collector that receives it and re-exports it to your backend.

## Environments

All metrics are grouped by environment. The environment value is derived from `process.env.NODE_ENV` by default (falling back to `'production'` if unset). To override it, use [`configure({ environment })`](/eas/observe/configuration.md#configure).

The environment is a metadata tag attached to each metric and is independent of how the bundle was built. To control whether debug-build metrics are dispatched, see [Enable metrics in development](/eas/observe/configuration.md#enable-metrics-in-development). To disable all dispatching globally, use `configure({ dispatchingEnabled: false })`.
