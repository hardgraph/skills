---
modificationDate: March 02, 2026
title: Share previews with your team
description: Share previews of your app with your team by publishing updates on branches.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/review/share-previews-with-your-team/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/review/share-previews-with-your-team/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Review
Pages in this section:
- [Distributing apps for review](https://docs.expo.dev/review/overview.md)
- [Share previews with your team](https://docs.expo.dev/review/share-previews-with-your-team.md) (this page)
- [Open updates with Orbit](https://docs.expo.dev/review/with-orbit.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Share previews with your team

Share previews of your app with your team by publishing updates on branches.

Once you've made changes on a branch, you can share them with your team by publishing an update. This allows you to get feedback on your changes during review.

The following steps will outline a basic flow for publishing a preview of your changes, and then sharing it with your team. For a more comprehensive resource, see the [Preview updates](/eas-update/preview.md) guide.

## Publish a preview of your changes

You can publish a preview of your current changes by running the following [EAS CLI](/develop/tools.md#eas-cli) command:

```sh
eas update --auto
```

This command will publish an update under the current branch name.

## Share with your team

Once the preview is published, you'll see output like this in the terminal window:

```sh
✔ Published!
...
EAS Dashboard      https://expo.dev/accounts/your-account/projects/your-project/updates/708b05d8-9bcf-4212-a052-ce40583b04fd
```

Share the **EAS dashboard** link with a reviewer. After opening the link, they can click on the **Preview** button. They will see a QR code that they can scan to open the preview on their device.

## Create previews automatically

You can automatically create previews on every commit with [EAS Workflows](/eas/workflows/introduction.md). First, you'll need to [configure your project](/eas/workflows/get-started.md), add a file named **.eas/workflows/publish-preview-update.yml** at the root of your project, then add the following workflow configuration:

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

The workflow above will publish an update on every commit to every branch. You can also run this workflow manually with the following EAS CLI command:

```sh
eas workflow:run publish-preview-update.yml
```

Learn more about common patterns with the [workflows examples guide](/eas/workflows/examples/introduction.md).

## Learn more

[Preview updates](/eas-update/preview.md) — Learn how to preview updates in development, preview, and production builds.
