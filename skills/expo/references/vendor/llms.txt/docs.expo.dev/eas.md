---
modificationDate: July 17, 2026
title: Expo Application Services
description: Learn about Expo Application Services (EAS) for Expo and React Native apps.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS
Pages in this section:
- [Introduction](https://docs.expo.dev/eas.md) (this page)
- [Configuration with eas.json](https://docs.expo.dev/eas/json.md)
- [EAS CLI](https://docs.expo.dev/eas/cli.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Application Services

Learn about Expo Application Services (EAS) for Expo and React Native apps.

Expo Application Services (EAS) are deeply integrated cloud services for Expo and React Native apps, from the team behind Expo.

Read the full pitch at [expo.dev/eas](https://expo.dev/eas), or follow the links below to learn how to get started.

[EAS Workflows](/eas/workflows/introduction.md) — Automate your development and release workflows with CI/CD jobs.

[EAS Build](/build/introduction.md) — Compile and sign Android/iOS apps with custom native code in the cloud.

[EAS Submit](/deploy/submit-to-app-stores.md) — Upload your app to the Google Play Store or Apple App Store from the cloud with one CLI command.

[EAS Hosting](/eas/hosting/introduction.md) — Deploy Expo Router and React Native web apps and API routes.

[EAS Update](/eas-update/introduction.md) — Address small bugs and push quick fixes directly to end users.

[EAS Metadata (in preview)](/eas/metadata.md) — Upload all app store information required to get your app published.

[EAS Insights (in preview)](/eas-insights/introduction.md) — View analytics about a project's performance, usage, and reach.

[EAS Observe (in open beta)](/eas/observe/introduction.md) — Monitor your app's performance in production.
