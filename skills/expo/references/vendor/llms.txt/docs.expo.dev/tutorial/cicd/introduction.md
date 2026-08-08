---
modificationDate: July 09, 2026
title: 'CI/CD Tutorial: Introduction'
description: An introduction to EAS Workflows tutorial and the core concepts for setting up a CI/CD pipeline for Expo and React Native apps.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/introduction/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/introduction/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > CI/CD tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/cicd/introduction.md) (this page)
- [First EAS Workflows job](https://docs.expo.dev/tutorial/cicd/first-workflow.md)
- [Development builds](https://docs.expo.dev/tutorial/cicd/development-builds.md)
- [Preview builds](https://docs.expo.dev/tutorial/cicd/preview-builds.md)
- [E2E tests](https://docs.expo.dev/tutorial/cicd/e2e-tests.md)
- [Production deployments](https://docs.expo.dev/tutorial/cicd/production.md)
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# CI/CD Tutorial: Introduction

An introduction to EAS Workflows tutorial and the core concepts for setting up a CI/CD pipeline for Expo and React Native apps.

Building, testing, and delivering an Expo and React Native app is rarely a one-step process. Each code change goes through a similar flow of building for Android and iOS, running unit and end-to-end (E2E) tests, and handing it off to teammates, QA, or the app stores. Doing this on every commit gets tedious fast, and it's exactly the kind of work a CI/CD pipeline should handle for us.

By the end of this tutorial, every push to our Expo project builds, tests, or ships automatically. The service that makes this possible is [EAS Workflows](/eas/workflows/introduction.md), a Continuous Integration (CI)/Continuous Delivery (CD) service from Expo Application Services (EAS) for Expo and React Native apps.

We define a workflow in a [YAML](https://en.wikipedia.org/wiki/YAML) file that lives in **.eas/workflows/** at the root of our Expo project. Here is a complete example that builds an Android development build on every push to the `main` branch in a GitHub repository:

```yaml
name: Build Android

on:
  push:
    branches: ['main']

jobs:
  build:
    type: build
    params:
      platform: android
      profile: development
```

That handful of lines is the entire workflow, and EAS handles the rest. We build on this pattern throughout the tutorial.

## Why use EAS Workflows?

EAS Workflows runs Android, iOS, and web builds, publishes [OTA updates](/deploy/send-over-the-air-updates.md), submits to app stores, and runs E2E tests with Maestro, all defined in YAML files like the example above. Because jobs run in a managed cloud environment, there is no build server for us to set up or maintain.

We can trigger workflows from GitHub events (push, pull request, tags, or labels), on a schedule (cron), or manually with [EAS CLI](/eas/cli.md).

## Topics covered

The tutorial has three parts:

-   **Development.** Create custom jobs, automate [development builds](/develop/development-builds/introduction.md) with [fingerprinting](/tutorial/cicd/development-builds.md), and ship [pull request (PR) preview updates](/tutorial/cicd/preview-builds.md) for stakeholders.
-   **Testing and releasing.** Run E2E tests with Maestro in a workflow, then create a production workflow that uses fingerprinting to choose between a native build and an OTA update.
-   **Extensions.** Switch production triggers from branches to version tags and deploy web builds with [EAS Hosting](/eas/hosting/introduction.md).

## Concepts worth knowing first

> **Tip:** Already familiar with EAS Build and EAS Update? Skip to the next section.

Before we create our first workflow, here are a few concepts worth knowing:

#### Where does an Expo app ship?

An Expo app can ship to three targets. Which one, and which EAS service delivers it, depends on what changed in the code:

| Target | EAS service | When to use |
| --- | --- | --- |
| App stores | EAS Build and EAS Submit | New releases, native code changes |
| Installed devices | EAS Update | TypeScript/JavaScript-only fixes, delivered over the air in seconds |
| Web | EAS Hosting | Web app alongside native releases |

#### Build profiles

[EAS Build](/build/introduction.md) is a service for building app binaries for our Expo projects. It supports three build profiles by default: `development`, `preview`, and `production`. Each profile corresponds to a different stage of development. We use each of these three profiles in this tutorial.

#### Builds vs. updates

Like EAS Build, [EAS Update](/eas-update/introduction.md) is a service that serves OTA updates for Expo projects using the [`expo-updates`](/versions/latest/sdk/updates.md) library.

```
Decision flow: Native changes require EAS Build (minutes). JS-only changes use EAS Update (seconds).
```

-   EAS Build compiles native code into a full app binary. Builds take longer to run. We need a new build when native code in our project changes. For example, adding a new native library, changing permissions, or upgrading the Expo SDK.
-   EAS Update delivers TypeScript/JavaScript changes over-the-air to devices that already have a compatible native build installed. If a change requires native code, we create and install that build before publishing an update. That way, each update remains compatible with the build it targets. Publishing an update itself takes seconds. For example, we can publish an update to fix a UI bug or change the copy of a button.

Depending on the change, we use one service or the other. EAS Workflows wires them together so the pipeline can decide when to build and when to publish an update.

## Prerequisites

> **Tip:** Already have an Expo project set up with EAS Build and EAS Update? Skip to the next chapter.

This tutorial is a hands-on experience. To make progress, we need an existing Expo project that uses [Continuous Native Generation](/workflow/continuous-native-generation.md). Set it up on our machine and EAS with the following requirements:

#### Prerequisites

##### An Expo account

We need to [sign up](https://expo.dev/signup) for an Expo account.

##### Install EAS CLI and log in

To install EAS CLI, run the following command:

```sh
# npm
npm install --global eas-cli

# yarn
yarn global add eas-cli

# pnpm
pnpm add -g eas-cli

# bun
bun add -g eas-cli
```

Then, run the following login command to authenticate EAS CLI with the Expo account:

```sh
eas login
```

##### Create an Expo project

To create a new Expo project, run the following command:

```sh
# npm
npx create-expo-app@latest --template default@sdk-57

# yarn
yarn create expo-app --template default@sdk-57

# pnpm
pnpm create expo-app --template default@sdk-57

# bun
bun create expo --template default@sdk-57
```

Creating a new project with `create-expo-app` sets up a project without native **android** and **ios** directories committed. This pattern is called Continuous Native Generation (CNG) and EAS Build creates those native project directories for us automatically.

##### Initialize EAS in the project

To initialize EAS in our project, run the following command:

```sh
eas build:configure
```

This generates an **eas.json** file in the root of our Expo project and creates an EAS project linked to our local project.

##### GitHub repository linked with EAS dashboard

We need a GitHub repository for the project. We can create a new repository or use an existing one. Then, connect the GitHub repository to EAS:

1.  Navigate to the project's [GitHub settings](https://expo.dev/accounts/%5Baccount%5D/projects/%5BprojectName%5D/github) on the EAS dashboard.
2.  Follow the UI in the dashboard to install the Expo GitHub app.
3.  Select the GitHub repository that matches the Expo project and connect it.

##### Store submission credentials for EAS Submit (optional)

Only needed if we add automated app store submission as an adaptation after Chapter 5. See the [Google Play Store](/submit/android.md) and [Apple App Store](/submit/ios.md) guides for instructions on how to configure credentials for EAS Submit.

## Coming from GitHub Actions?

EAS Workflows files live in the **.eas/workflows/** directory at the root of an Expo project, similar to how GitHub Actions workflows live in **.github/workflows/**. The trigger syntax (`on.push`, `on.pull_request`, and so on) also looks much the same.

The main difference is pre-packaged jobs: setting `type: build` lets EAS handle the environment and build artifacts, with no `runs-on` or setup step to write. We cover these as we go.

> **Note:** EAS Workflows can run alongside GitHub Actions and a GitHub Actions job can trigger a workflow with `eas workflow:run`. See [How to integrate EAS Workflows with GitHub Actions](https://expo.dev/blog/how-to-integrate-eas-workflows-with-github-actions) for more information.

## Next step

With our project set up, we can move to the first chapter and write our first workflow.

[Start](/tutorial/cicd/first-workflow.md) — Create and run our first workflow with EAS Workflows.
