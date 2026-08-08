---
modificationDate: June 03, 2026
title: Estimate bandwidth usage
description: Learn how to estimate bandwidth usage for EAS Update.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas-update/estimate-bandwidth/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas-update/estimate-bandwidth/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Update > Reference
Pages in this section:
- [Code signing](https://docs.expo.dev/eas-update/code-signing.md)
- [Asset selection and exclusion](https://docs.expo.dev/eas-update/asset-selection.md)
- [Using without other EAS services](https://docs.expo.dev/eas-update/standalone-service.md)
- [Request proxying](https://docs.expo.dev/eas-update/request-proxying.md)
- [Migrate from CodePush](https://docs.expo.dev/eas-update/codepush.md)
- [Migrate from Classic Updates](https://docs.expo.dev/eas-update/migrate-from-classic-updates.md)
- [Trace update ID back to the EAS dashboard](https://docs.expo.dev/eas-update/trace-update-id-expo-dashboard.md)
- [Estimate bandwidth usage](https://docs.expo.dev/eas-update/estimate-bandwidth.md) (this page)
- [Integrate in existing native apps](https://docs.expo.dev/eas-update/integration-in-existing-native-apps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Estimate bandwidth usage

Learn how to estimate bandwidth usage for EAS Update.

## Understanding update bandwidth usage

EAS Update enables an app to update its own non-native pieces (such as JavaScript, styling, and images) over-the-air. This guide explains how bandwidth is consumed and how consumption can be optimized.

## Bandwidth calculation breakdown

Each subscription plan includes a predefined bandwidth allocation per monthly billing period in addition to its monthly active user (MAU) allocation ([learn more about how MAU are calculated](/eas-update/introduction.md#how-are-monthly-active-users-counted-for)). MAU's beyond the standard allocation are billed at [usage-based pricing rates](https://expo.dev/pricing#update), and each of those additional MAU add an extra 40 MiB to your standard bandwidth allocation. This bandwidth determines the number of updates your users can download before being charged for additional bandwidth usage.

Here's how to estimate bandwidth usage per update:

-   **Update size:** The key factor in bandwidth consumption is the size of update assets that are not already on the device. If an update only modifies the JavaScript portion of your app, users will only download the new JavaScript. To begin an example, let's say the uncompressed JavaScript portion generated during export is **10 MB**. Compression will further reduce its size.
    
-   **Compression ratio:** The level of compression depends on the file type. JavaScript and Hermes bytecode (commonly used in React Native apps) can be compressed, while images and icons are not automatically compressed. In the example above, a Hermes bytecode bundle achieves an estimated **2.6× compression ratio**, reducing the actual download size to:
    
    ```text
    10 MB / 2.6 ≈ 3.85 MB update bandwidth size
    ```
    

Given a bandwidth allocation, we estimate how many updates can be downloaded in a monthly billing period before additional bandwidth charges apply. For example, if you have 60,000 MAUs on a production plan, it includes 50,0000 MAU and **1 TiB (1,024 GiB) of bandwidth per month**. An additional 10,000 MAUs purchased through usage-based pricing receives an additional **40 MiB of bandwidth per MAU**. The total number of updates that can be downloaded is:

```text
(1,024 GiB × 1,024 MiB/GiB) + (10,000 MAU × 40 MiB/MAU) = 1,448,576 MiB per month
1,448,576 MiB / 3.85 MiB ≈ 376,254 updates
```

## Measuring your actual update size

Start by generating your uncompressed production bundle with the following command:

```sh
# npm
npx expo export

# yarn
yarn expo export

# pnpm
pnpm expo export

# bun
bun expo export
```

Then, look inside the **dist/_expo/static/js** directory. There will be **android** and **ios** directories with a **.hbc** file in each. The size of the **.hbc** file is the uncompressed size of your Hermes bundle. Note that Android and iOS bundles may differ in size, especially if you make use of platform-specific code files.

These files have long names that include a hash. For this example, rename the file you would like to analyze to **bundle.hbc**.

To determine the actual compressed size of your Hermes bundle, which is how large it will be when downloaded by app users, run the following commands:

```sh
brotli -5 -k bundle.hbc
gzip -9 -k bundle.hbc
ls -lh bundle.hbc.br bundle.hbc.gz
```

This will generate **Brotli- and Gzip-compressed** versions of your Hermes bundle (**bundle.hbc.br** and **bundle.hbc.gz**) and display their sizes. You can use this to refine bandwidth calculations based on your app's real-world update sizes.

## Factors that affect bandwidth consumption

Actual bandwidth usage varies due to:

-   **User behavior:** Theoretical calculations assume every user downloads every update. However, many users only get updates when they reopen the app, often skipping intermediate updates. As a result, actual bandwidth usage is usually much lower than the theoretical maximum.
-   **Missing assets:** If an update includes assets such as fonts and images that are not already on the device from the build or previously downloaded updates, they will need to be downloaded as well.

## Optimizing bandwidth usage

1.  **Monitor usage first:** The easiest way to manage bandwidth is to track your [usage metrics](https://expo.dev/accounts/%5Baccount%5D/settings/usage) and identify any unusual spikes or inefficiencies.
2.  **Optimize asset size:** Reduce the size of your assets with [this guide](/eas-update/optimize-assets.md).
3.  **Exclude assets when needed:** Use [asset selection](/eas-update/asset-selection.md) to reduce the number of assets included with each update. This is an advanced optimization and other approaches should be tried first.
