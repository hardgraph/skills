---
modificationDate: March 02, 2026
title: Send over-the-air updates
description: Learn how to send over-the-air updates to push critical bug fixes and improvements to your users.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/deploy/send-over-the-air-updates/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/deploy/send-over-the-air-updates/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Deploy
Pages in this section:
- [Build project for app stores](https://docs.expo.dev/deploy/build-project.md)
- [Submit to app stores](https://docs.expo.dev/deploy/submit-to-app-stores.md)
- [App stores metadata](https://docs.expo.dev/deploy/app-stores-metadata.md)
- [Send over-the-air updates](https://docs.expo.dev/deploy/send-over-the-air-updates.md) (this page)
- [Deploy web apps](https://docs.expo.dev/deploy/web.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Send over-the-air updates

Learn how to send over-the-air updates to push critical bug fixes and improvements to your users.

You can send over-the-air updates containing critical bug fixes and improvements to your users.

## Get started

> If you've published [previews](/review/share-previews-with-your-team.md) or created a [build](/deploy/build-project.md) before, you may have already set up updates and can skip this section.

To set up updates, run the following [EAS CLI](/develop/tools.md#eas-cli) command:

```sh
eas update:configure
```

After the command completes, you'll need to make new builds before continuing to the next section.

## Send an update

To send an update, run the following [EAS CLI](/develop/tools.md#eas-cli) command:

```sh
eas update --channel production
```

This command will create an update and make it available to builds of your app that are configured to receive updates on the `production` channel. This channel is defined in [**eas.json**](/eas/json.md#channel).

You can verify the update works by force closing the app and reopening it two times. The update should be applied on the second launch.

## Send updates automatically

You can automatically send updates with [EAS Workflows](/eas/workflows/introduction.md). First, you'll need to [configure your project](/eas/workflows/get-started.md), add a file named **.eas/workflows/send-updates.yml** at the root of your project, then add the following workflow configuration:

```yaml
name: Send updates

on:
  push:
    branches: ['main']

jobs:
  send_updates:
    name: Send updates
    type: update
    params:
      channel: production
```

The workflow above will send an over-the-air update for the `production` update channel on every commit to your project's `main` branch. You can also run this workflow manually with the following EAS CLI command:

```sh
eas workflow:run send-updates.yml
```

Learn more about common patterns with the [workflows examples guide](/eas/workflows/examples/introduction.md).

## Learn more

You can learn how to [rollout an update](/eas-update/rollouts.md), [optimize assets](/eas-update/optimize-assets.md), and more with our [update guides](/eas-update/introduction.md).
