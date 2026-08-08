---
id: "tools/paywalls"
title: "Paywalls"
description: "RevenueCat Paywalls let you remotely configure your entire paywall view without any code changes or app updates. Whether you're building a new app, exploring new paywall concepts, or running experiments, you can get started without touching your app code. Pick from a curated template, start from scratch, or describe what you want in the AI Editor and let it generate a paywall for you."
permalink: "/docs/tools/paywalls"
slug: "paywalls"
version: "current"
original_source: "docs/tools/paywalls.mdx"
---

> **AI agents:** This is the Markdown version of a RevenueCat documentation page. For the complete documentation index, see [llms.txt](https://www.revenuecat.com/docs/llms.txt).

RevenueCat Paywalls let you remotely configure your entire paywall view without any code changes or app updates. Whether you're building a new app, exploring new paywall concepts, or running experiments, you can get started without touching your app code. Pick from a curated template, start from scratch, or describe what you want in the [AI Editor](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls#paywalls-ai-editor) and let it generate a paywall for you.

**Video:** [RevenueCat Paywalls](https://www.youtube.com/watch?v=mPzCTxIlMXE)

You can think of a Paywall as an optional feature of your Offering. An Offering is the collection of Products which are organized into Packages to be displayed to your customers as a single "offer" across platforms. Now, with Paywalls, you can control the actual view that is used to display that "offer" in addition to controlling the products that are offered.

Paywalls can be a single screen, or a [multipage flow](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls#multipage-paywalls) that guides customers through a sequence of screens before they reach the offer — useful for onboarding, or progressive value propositions.

Therefore, you can create a unique Paywall for each of your Offerings, and can create an unlimited number of Offerings and Paywalls for each variation you want to test with Experiments.

## Getting started

Our paywalls use native code to deliver smooth, intuitive experiences to your customers when you're ready to deliver them an Offering; and you can use our Dashboard to build your paywall from any of our existing templates, or start from scratch to create your own. Either way, you'll have full control of the components and their properties to modify the paywall to meet your needs.

To use RevenueCat Paywalls:

1. [Install the RevenueCat UI SDK](https://www.revenuecat.com/docs/tools/paywalls/installation)

2. [Create a Paywall](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls) on the Dashboard for the [Offering](https://www.revenuecat.com/docs/getting-started/entitlements) you intend to serve to your customers

3. See [displaying paywalls](https://www.revenuecat.com/docs/tools/paywalls/displaying-paywalls) for how to display it into your app.

4. (Optional) [Connect analytics integrations](https://www.revenuecat.com/docs/tools/paywalls/integrations) to send paywall events to Amplitude, Mixpanel, PostHog, Segment, or your own server

## Limitations

### Required SDK Versions

| RevenueCat SDK           | Minimum recommended version   |
| :----------------------- | :---------------------------- |
| purchases-ios            | 5.27.1 and up                 |
| purchases-android        | 8.19.2 and up                 |
| react-native-purchases   | 8.11.3 and up                 |
| purchases-flutter        | 8.10.1 and up                 |
| purchases-kmp            | 1.8.2+13.35.0 and up          |
| purchases-capacitor      | 10.3.3 and up                 |
| purchases-unity          | RevenueCatUI package required |
| cordova-plugin-purchases | Not supported                 |

:::info\[Paywalls on the web]
Paywalls are also available on the web via RevenueCat's Web Purchase Links. [Learn more](https://www.revenuecat.com/docs/web/paywalls).
:::

Prior SDK versions support our beta release of Paywalls, but we recommend updating to at least the recommended version listed above for each SDK to take advantage of all features and fixes from the beta period.

#### Multipage paywalls

[Multipage paywalls](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls#multipage-paywalls) require a higher minimum SDK version than single-page paywalls. On SDK versions below the minimum, RevenueCat automatically shows **only** the final screen of the flow. If you'd rather serve a different paywall to older versions instead of the final screen, set up [Fallback paywalls by SDK version](https://www.revenuecat.com/docs/tools/targeting/fallback-paywalls-by-sdk-version).

| RevenueCat SDK         | Minimum version for multipage |
| :--------------------- | :---------------------------- |
| purchases-ios          | 5.83.0                        |
| purchases-android      | 10.16.0                       |
| react-native-purchases | 10.6.0                        |
| purchases-flutter      | 10.7.0                        |
| purchases-kmp          | 3.4.0                         |
| purchases-capacitor    | 13.3.0                        |
| purchases-unity        | 9.7.0                         |
| purchases-js           | 1.50.0                        |

### Platforms

- ✅ iOS 15.0 and higher
- ✅ Android 7.0 (API level 24) and higher
- ✅ Mac Catalyst 15.0 and higher
- ✅ macOS 12.0 and higher
- ✅ Web (via Web Purchase Links)
- ❌ watchOS
- ❌ tvOS
- ❌ visionOS

## Next Steps

- Now that you know how our paywalls work, read about [creating paywalls](https://www.revenuecat.com/docs/tools/paywalls/creating-paywalls)
- Once you're ready to see your paywalls in action, you can follow our guides on [displaying paywalls](https://www.revenuecat.com/docs/tools/paywalls/displaying-paywalls)
