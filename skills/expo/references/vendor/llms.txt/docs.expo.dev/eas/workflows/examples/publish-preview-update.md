---
modificationDate: July 28, 2026
title: Publish preview updates with EAS Workflows
description: Learn how to publish preview updates with EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/workflows/examples/publish-preview-update/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/workflows/examples/publish-preview-update/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Workflows > Examples
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/workflows/examples/introduction.md)
- [Create development builds](https://docs.expo.dev/eas/workflows/examples/create-development-builds.md)
- [Publish preview updates](https://docs.expo.dev/eas/workflows/examples/publish-preview-update.md) (this page)
- [Clean up update branches](https://docs.expo.dev/eas/workflows/examples/branch-cleanup.md)
- [Deploy to production](https://docs.expo.dev/eas/workflows/examples/deploy-to-production.md)
- [Run E2E tests](https://docs.expo.dev/eas/workflows/examples/e2e-tests.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Publish preview updates with EAS Workflows

Learn how to publish preview updates with EAS Workflows.

Once you've made changes to your project, you can share a preview of your changes with your team by publishing a [preview update](/review/share-previews-with-your-team.md). This is useful when you want to review changes with your team without pulling the latest changes and running them locally.

You can access preview updates in the development build UI and through scannable QR codes on the EAS dashboard. When publishing a preview on every commit, your team can review changes without pulling the latest changes and running them locally.

[Expo Golden Workflow: Share preview updates with your team](https://www.youtube.com/watch?v=v_rzRcVSQYQ) — Publish preview updates on every commit with EAS Workflows so your team can review changes without pulling code locally.

## Get started

#### Prerequisites

##### Set up EAS Update

Your project needs to have [EAS Update](/eas-update/introduction.md) setup to publish preview updates. You can set up your project with:

```sh
eas update:configure
```

##### Create new development builds

After you've configured your project, create new [development builds](/develop/development-builds/introduction.md?buildenv=build-with-eas#how-would-you-like-to-build-your-development-build) for each platform.

The following workflow publishes a preview update for every commit on every branch.

```yaml
name: Publish preview update

on:
  push:
    branches: ['*']

jobs:
  publish_preview_update:
    name: Publish preview update
    type: update
    params:
      branch: ${{ github.ref_name || 'test' }}
```
