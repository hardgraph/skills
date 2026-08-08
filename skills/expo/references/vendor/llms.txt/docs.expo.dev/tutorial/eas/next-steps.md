---
modificationDate: July 17, 2026
title: Next steps
description: Learn about the next steps in your journey with EAS.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/next-steps/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/next-steps/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md)
- [iOS development build for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices.md)
- [Multiple app variants](https://docs.expo.dev/tutorial/eas/multiple-app-variants.md)
- [Internal distribution build](https://docs.expo.dev/tutorial/eas/internal-distribution-builds.md)
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Next steps

Learn about the next steps in your journey with EAS.

Congratulations! You've completed the EAS tutorial and learned about the main features. Now, you have a working [EAS project](https://expo.dev/eas).

## EAS

But this is just the beginning. Here are some next steps to continue your journey with EAS:

[EAS Workflows](/eas/workflows/introduction.md) — See EAS Workflows documentation to learn more about automating your development and release workflows.

[EAS Build](/build/introduction.md) — See EAS Build documentation to learn more about compiling and signing Android and iOS apps.

[EAS Hosting](/eas/hosting/introduction.md) — See EAS Hosting documentation to learn more about deploying Expo Router and React Native web apps, and API routes.

[EAS Submit](/deploy/submit-to-app-stores.md) — See EAS Submit documentation to learn more about uploading app to Google Play Store and Apple App Store with one CLI command.

[EAS Update](/eas-update/introduction.md) — See EAS Update documentation to learn more about publishing updates and applying customizable strategies.

[EAS Metadata (in preview)](/eas/metadata.md) — See EAS Metadata documentation to automate and maintain your app store presence from the command line.

[EAS Insights (in preview)](/eas-insights/introduction.md) — See EAS Insights documentation to learn more about how to use expo-insights library and get precise usage metrics. — expo-insights

## eas.json reference

[eas.json schema](/eas/json.md) — See the complete schema reference to learn more about available properties for EAS Build and EAS Submit, and to configure and override their default behavior from within your project.

## Custom builds

[Custom builds](/custom-builds/get-started.md) — See custom builds documentation to extend EAS Build and use your own configuration to create build workflows.

## Relevant guides

[Automate submissions](/build/automate-submissions.md) — See automate submissions guide on how to enable automatic submissions with EAS Build on app stores.

[GitHub Actions with EAS Update](/eas-update/github-actions.md) — See Use GitHub Actions guide on how to automate publishing updates with EAS Update.

[App credentials](/app-signing/app-credentials.md) — See App credentials guide to learn more about credentials requirement for Android and iOS.

[Develop an app with Expo](/workflow/overview.md) — An overview of the development process of building an Expo app to help build a mental model of the core development loop.

We hope you enjoyed this course. If you have any questions or feedback, don't hesitate to reach out to us on [Discord](https://chat.expo.dev/), or share your experience on [X](https://x.com/expo).
