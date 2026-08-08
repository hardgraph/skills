---
modificationDate: July 29, 2026
title: 'Distribution: Overview'
description: An overview of submitting an app to the app stores or with the internal distribution.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/distribution/introduction/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/distribution/introduction/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > Distribution
Pages in this section:
- [Overview](https://docs.expo.dev/distribution/introduction.md) (this page)
- [App stores best practices](https://docs.expo.dev/distribution/app-stores.md)
- [App transfers](https://docs.expo.dev/distribution/app-transfers.md)
- [Understanding app size](https://docs.expo.dev/distribution/app-size.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Distribution: Overview

An overview of submitting an app to the app stores or with the internal distribution.

Get your app into the hands of users by submitting it to the app stores or with [Internal distribution](/build/internal-distribution.md).

```sh
# npm
npm install --global eas-cli
eas build --auto-submit
eas submit

# yarn
yarn global add eas-cli
eas build --auto-submit
eas submit

# pnpm
pnpm add --global eas-cli
eas build --auto-submit
eas submit

# bun
bun add --global eas-cli
eas build --auto-submit
eas submit
```

You can run `eas build --auto-submit` with [EAS CLI](/eas.md) to build your app and automatically upload the binary for distribution on the Google Play Store and Apple App Store.

This automatically manages **all native code signing** for Android and iOS for any React Native app. Advanced features such as payments, notifications, universal links, and iCloud can be automatically enabled based on your [config plugins](/config-plugins/introduction.md) or native entitlements, meaning no more wrestling with slow portals to get libraries set up correctly.

### Get started

[Submit to the Google Play Store](/submit/android.md) — Learn how to submit an Android app to the Google Play Store.

[Submit to the Apple App Store](/submit/ios.md) — Learn how to submit an iOS or an iPadOS app to the Apple App Store from any operating system.

[Internal distribution](/build/internal-distribution.md) — Share your mobile app internally with testers using AdHoc builds.

[Publish websites](/guides/publishing-websites.md) — Export your website and upload to any web host.

[OTA updates](/eas-update/introduction.md) — Send over-the-air updates to your users instantly.
