---
modificationDate: July 13, 2026
title: Automating EAS CLI commands
description: Learn how to automate sequences of EAS CLI commands with EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/workflows/automating-eas-cli/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/workflows/automating-eas-cli/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Workflows
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/workflows/introduction.md)
- [Get started](https://docs.expo.dev/eas/workflows/get-started.md)
- [Pre-packaged jobs](https://docs.expo.dev/eas/workflows/pre-packaged-jobs.md)
- [Syntax](https://docs.expo.dev/eas/workflows/syntax.md)
- [Environment variables](https://docs.expo.dev/eas/workflows/environment.md)
- [Automating EAS CLI commands](https://docs.expo.dev/eas/workflows/automating-eas-cli.md) (this page)
- [REST API](https://docs.expo.dev/eas/workflows/rest-api.md)
- [Troubleshooting](https://docs.expo.dev/eas/workflows/troubleshooting.md)
- [Limitations](https://docs.expo.dev/eas/workflows/limitations.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Automating EAS CLI commands

Learn how to automate sequences of EAS CLI commands with EAS Workflows.

If you're using EAS CLI to build, submit, and update your app, you can automate sequences of commands with EAS Workflows. EAS Workflows can build, submit, and update your app, while also running other jobs like Maestro tests, unit tests, custom scripts, and more.

Below you'll find how to set up your project to use EAS Workflows, followed by common examples of EAS CLI commands and how you can run them using EAS Workflows.

## Configure your project

EAS Workflows optionally supports a GitHub repository that's linked to your EAS project to run. This guide assumes a GitHub repository is linked, and shows how to trigger workflows when pushing to specific branches on GitHub. You can link a GitHub repo to your EAS project with the following steps:

-   Navigate to your project's [GitHub settings](https://expo.dev/accounts/%5Baccount%5D/projects/%5BprojectName%5D/github).
-   Follow the UI to install the GitHub app.
-   Select the GitHub repository that matches the Expo project and connect it.

## Creating builds

You can make a build of your project using EAS CLI with the `eas build` command. To make an iOS build with the `production` build profile, you could run the following EAS CLI command:

```sh
eas build --platform ios --profile production
```

To write this command as a workflow, create a workflow file named **.eas/workflows/build-ios-production.yml** at the root of your project.

Inside **build-ios-production.yml**, you can use the following workflow to kick off a job that creates an iOS build with the `production` build profile.

```yaml
name: iOS production build

on:
  push:
    branches: ['main']

jobs:
  build_ios:
    name: Build iOS
    type: build
    params:
      platform: ios
      profile: production
```

Once you have this workflow file, you can kick it off by pushing a commit to the `main` branch, or by running the following EAS CLI command:

```sh
eas workflow:run .eas/workflows/build-ios-production.yml
```

You can provide parameters to make Android builds or use other build profiles. Learn more about build job parameters with the [build job documentation](/eas/workflows/syntax.md#build).

## Submitting builds

You can submit your app to the app stores using EAS CLI with the `eas submit` command. To submit an iOS app, you could run the following EAS CLI command:

```sh
eas submit --platform ios
```

To write this command as a workflow, create a workflow file named **.eas/workflows/submit-ios.yml** at the root of your project.

The submit job requires the ID of the build to submit. Inside **submit-ios.yml**, you can use the following workflow to create an iOS build and then submit it:

```yaml
name: Submit iOS app

on:
  push:
    branches: ['main']

jobs:
  build_ios:
    name: Build iOS
    type: build
    params:
      platform: ios
      profile: production
  submit_ios:
    name: Submit iOS
    needs: [build_ios]
    type: submit
    params:
      build_id: ${{ needs.build_ios.outputs.build_id }}
```

To submit an existing build instead of creating a new one, pass its `build_id` directly to the submit job, or use the [`get-build`](/eas/workflows/pre-packaged-jobs.md#get-build) job to look one up dynamically and pass its `build_id` output.

Once you have this workflow file, you can kick it off by pushing a commit to the `main` branch, or by running the following EAS CLI command:

```sh
eas workflow:run .eas/workflows/submit-ios.yml
```

You can provide parameters to use other submit profiles. Learn more about submit job parameters with the [submit job documentation](/eas/workflows/syntax.md#submit).

## Publishing updates

You can update your app using EAS CLI with the `eas update` command. To update your app, you could run the following EAS CLI command:

```sh
eas update --auto
```

To write this command as a workflow, create a workflow file named **.eas/workflows/publish-update.yml** at the root of your project.

Inside **publish-update.yml**, you can use the following workflow to kick off a job that sends and over-the-air update.

```yaml
name: Publish update

on:
  push:
    branches: ['*']

jobs:
  update:
    name: Update
    type: update
    params:
      branch: ${{ github.ref_name || 'test'}}
```

Once you have this workflow file, you can kick it off by pushing a commit to any branch, or by running the following EAS CLI command:

```sh
eas workflow:run .eas/workflows/publish-update.yml
```

You can provide parameters to update specific branches or channels, and configure the update's message. Learn more about update job parameters with the [update job documentation](/eas/workflows/syntax.md#update).

## Next step

Workflows are a powerful way to automate your development and release processes. Learn how to create development builds, publish preview updates, and create production builds with the workflows examples guide:

[Workflow examples](/eas/workflows/examples/introduction.md) — Learn how to use workflows to create development builds, publish preview updates, and create production builds.
