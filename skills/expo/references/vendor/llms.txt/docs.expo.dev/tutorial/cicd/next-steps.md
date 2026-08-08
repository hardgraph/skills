---
modificationDate: July 28, 2026
title: Next steps
description: Learn about the next steps in your journey with EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/next-steps/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/next-steps/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > CI/CD tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/cicd/introduction.md)
- [First EAS Workflows job](https://docs.expo.dev/tutorial/cicd/first-workflow.md)
- [Development builds](https://docs.expo.dev/tutorial/cicd/development-builds.md)
- [Preview builds](https://docs.expo.dev/tutorial/cicd/preview-builds.md)
- [E2E tests](https://docs.expo.dev/tutorial/cicd/e2e-tests.md)
- [Production deployments](https://docs.expo.dev/tutorial/cicd/production.md)
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Next steps

Learn about the next steps in your journey with EAS Workflows.

Congratulations! You've completed the CI/CD tutorial and built a full pipeline for an Expo project with [EAS Workflows](/eas/workflows/introduction.md). Every push, pull request, and version tag now runs the right automation, from development builds through production releases and web deploys.

## EAS Workflows references

[EAS Workflows syntax reference](/eas/workflows/syntax.md) — See the complete syntax reference for workflow files, including triggers, jobs, expressions, and conditional execution.

[Pre-packaged jobs](/eas/workflows/pre-packaged-jobs.md) — See the catalog of pre-packaged job types for builds, updates, fingerprinting, submissions, deploys, Slack notifications, and more.

[EAS environment variables](/eas/environment-variables.md) — Learn how to define and use environment variables in EAS Workflows for secrets, config, and per-environment values.

## Related EAS services

[EAS Build](/build/introduction.md) — See EAS Build documentation to learn more about compiling and signing Android and iOS apps.

[EAS Update](/eas-update/introduction.md) — See EAS Update documentation to learn more about publishing over-the-air updates and customizable rollout strategies.

[EAS Submit](/deploy/submit-to-app-stores.md) — See EAS Submit documentation to learn more about uploading app binaries to the Google Play Store and Apple App Store from one CLI command.

[EAS Hosting](/eas/hosting/introduction.md) — See EAS Hosting documentation to learn more about deploying Expo Router and React Native web apps.

## Relevant guides

[Automate submissions](/build/automate-submissions.md) — See the automate submissions guide to enable automatic submissions to app stores after every EAS Build.

[App credentials](/app-signing/app-credentials.md) — See the app credentials guide to learn more about credentials requirements for Android and iOS production builds and submissions.

[Custom builds](/custom-builds/get-started.md) — See the custom builds documentation to extend EAS Build with your own configuration and build steps.

[GitHub Actions with EAS Update](/eas-update/github-actions.md) — See the GitHub Actions guide on how to publish updates with EAS Update from a GitHub Actions workflow.

We hope you enjoyed this tutorial. If you have any questions or feedback, don't hesitate to reach out to us on [Discord](https://chat.expo.dev/), or share your experience on [X](https://x.com/expo).
