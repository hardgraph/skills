---
modificationDate: July 13, 2026
title: Troubleshooting overview
description: An overview of troubleshooting guides for app development with Expo and EAS.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/troubleshooting/overview/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/troubleshooting/overview/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Troubleshooting
Pages in this section:
- [Overview](https://docs.expo.dev/troubleshooting/overview.md) (this page)
- ["Application has not been registered" error](https://docs.expo.dev/troubleshooting/application-has-not-been-registered.md)
- [Clear bundler caches on macOS and Linux](https://docs.expo.dev/troubleshooting/clear-cache-macos-linux.md)
- [Clear bundler caches on Windows](https://docs.expo.dev/troubleshooting/clear-cache-windows.md)
- ["React Native version mismatch" errors](https://docs.expo.dev/troubleshooting/react-native-version-mismatch.md)
- [Proxies](https://docs.expo.dev/troubleshooting/proxies.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Troubleshooting overview

An overview of troubleshooting guides for app development with Expo and EAS.

This page lists a collection of various troubleshooting guides for Expo and EAS.

## Error and warnings

[View logs](/workflow/logging.md) — Learn how to view logs when using Expo CLI to encounter errors and warnings while developing your app.

[Errors and warnings](/debugging/errors-and-warnings.md) — Learn about Redbox errors and stack traces to help you debug your Expo project.

[Common development errors](/workflow/common-development-errors.md) — A list of common development errors that are encountered by developers using Expo.

["Application has not been registered" error](/troubleshooting/application-has-not-been-registered.md) — Learn about what the Application has not been registered error means and how to resolve it.

[Clear bundler caches on macOS and Linux](/troubleshooting/clear-cache-macos-linux.md) — Learn how to clear the bundler cache when using Yarn or npm with Expo CLI or React Native CLI on macOS and Linux.

[Clear bundler caches on Windows](/troubleshooting/clear-cache-windows.md) — Learn how to clear the bundler cache when using Yarn or npm with Expo CLI or React Native CLI on Windows.

["React Native version mismatch" errors](/troubleshooting/react-native-version-mismatch.md) — Learn about what React Native version mismatch means and how to resolve it in an Expo or React Native app.

[Proxies](/troubleshooting/proxies.md) — Learn about troubleshooting proxies with a set of recommended tools.

## Runtime issues in development and production

[Debugging runtime issues](/debugging/runtime-issues.md) — Learn about different techniques to debug your native runtime issues in development and production.

[Debugging and profiling tools](/debugging/tools.md) — Learn about different tools available to inspect your Expo project at runtime.

[Dev tools plugins](/debugging/devtools-plugins.md) — Learn about using dev tools plugins to inspect and debug your Expo project.

## Expo Router

[Expo Router: Troubleshooting](/router/reference/troubleshooting.md) — A list of common issues with Expo Router setup and how to fix them.

[Expo Router FAQ](/router/introduction.md) — A list of common questions about Expo Router.

## Push notifications

[Push notifications: Troubleshooting and FAQ](/push-notifications/faq.md) — A collection of common questions about Expo's push notification service.

## EAS

[EAS Build: Troubleshooting errors and crashes](/build-reference/troubleshooting.md) — A reference for troubleshooting build errors and crashes when using EAS Build.

[EAS Update: Basic debugging](/eas-update/debug.md) — Learn how to use basic debugging techniques to fix an update issue.

[EAS Update: Advanced debugging](/eas-update/debug.md) — Learn advanced strategies on how to debug EAS Update.

[EAS Update: Error recovery](/eas-update/error-recovery.md) — Learn how to take advantage of using built-in error recovery when using expo-updates library. — expo-updates

[EAS Observe: Troubleshooting](/eas/observe/reference/troubleshooting.md) — Solutions for common issues when setting up and using EAS Observe.

[EAS Workflows: Troubleshooting](/eas/workflows/troubleshooting.md) — Diagnose and fix common issues when running EAS Workflows.
