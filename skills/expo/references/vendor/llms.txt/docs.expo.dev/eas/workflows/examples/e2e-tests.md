---
modificationDate: July 22, 2026
title: Run E2E tests on EAS Workflows with Maestro
description: Learn how to set up and run E2E tests on EAS Workflows with Maestro.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/eas/workflows/examples/e2e-tests/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/eas/workflows/examples/e2e-tests/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Workflows > Examples
Pages in this section:
- [Introduction](https://docs.expo.dev/eas/workflows/examples/introduction.md)
- [Create development builds](https://docs.expo.dev/eas/workflows/examples/create-development-builds.md)
- [Publish preview updates](https://docs.expo.dev/eas/workflows/examples/publish-preview-update.md)
- [Clean up update branches](https://docs.expo.dev/eas/workflows/examples/branch-cleanup.md)
- [Deploy to production](https://docs.expo.dev/eas/workflows/examples/deploy-to-production.md)
- [Run E2E tests](https://docs.expo.dev/eas/workflows/examples/e2e-tests.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Run E2E tests on EAS Workflows with Maestro

Learn how to set up and run E2E tests on EAS Workflows with Maestro.

In this guide, you'll learn how to run end-to-end (E2E) tests on EAS Workflows using [Maestro](https://maestro.dev/). The example demonstrates how to configure your E2E tests workflow using the [default Expo template](/more/create-expo.md#--template). For your own app, you'll need to adjust the flows to match your app's UI.

## Set up your project

If you haven't already, create a new project and sync it with EAS.

Follow the [Get started with EAS Workflows guide](/eas/workflows/get-started.md) to create a new project and sync it with EAS. Then, [configure your project](/eas/workflows/get-started.md) and link your GitHub repository.

## Add example Maestro test cases

This is what the UI of the app created from the default Expo template looks like:

Let's create two simple Maestro flows for the example app. Start by creating a directory called **.maestro** in the root of your project directory. This directory will contain the flows you'll configure and should be at the same level as **eas.json**.

Inside, create a new file called **home.yml**. This flow will launch the app and assert that the text "Welcome!" is visible on the home screen.

```yaml
appId: dev.expo.eastestsexample # This is an example app id. Replace it with your app id.
---
- launchApp
- assertVisible: 'Welcome!'
```

Next, create a new flow called **expand_test.yml**. This flow will open the "Explore" screen in the example app, click on the "File-based routing" collapsible, and assert that the text "This app has two screens." is visible on the screen.

```yaml
appId: dev.expo.eastestsexample # This is an example app id. Replace it with your app id.
---
- launchApp
- tapOn: 'Explore.*'
- tapOn: '.*File-based routing'
- assertVisible: 'This app has two screens.*'
```

## Run Maestro tests locally (optional)

To run Maestro tests locally, install the Maestro CLI by following the instructions in [Installing Maestro](https://docs.maestro.dev/maestro-cli/how-to-install-maestro-cli).

[Install your app on a local Android Emulator or iOS Simulator](/more/expo-cli.md#compiling). Open a terminal, navigate to the Maestro directory, and run the following commands to start the tests with the Maestro CLI:

```sh
maestro test .maestro/expand_test.yml
maestro test .maestro/home.yml
```

The video below shows a successful run of the **.maestro/expand_test.yml** flow:

## Build profile for E2E tests

E2E tests require a built app file: **.apk** for Android or **.app** for iOS — that EAS can install and test on an emulator/simulator.

In your **eas.json** file, create a build profile for E2E tests. If the file doesn't exist, run `eas build:configure` to generate it.

```json
{
  "build": {
    "e2e-test": {
      "withoutCredentials": true,
      "ios": {
        "simulator": true
      },
      "android": {
        "buildType": "apk"
      }
    }
  }
}
```

The above build profile creates an **.apk** for Android and an **.app** for iOS. The workflow uses this profile to build the app on EAS servers.

## Create an E2E test workflow

At the root of your project, create an **.eas/workflows** directory. Then, add a YAML file for your E2E test workflow, such as **.eas/workflows/e2e-test-android.yml**.

```yaml
name: e2e-test-android

on:
  pull_request:
    branches: ['*'] # Run the E2E test workflow on every pull request.
jobs:
  build_android_for_e2e:
    type: build
    params:
      platform: android
      profile: e2e-test # your eas build profile for E2E test

  maestro_test:
    needs: [build_android_for_e2e]
    type: maestro
    params:
      build_id: ${{ needs.build_android_for_e2e.outputs.build_id }}
      flow_path: ['.maestro/home.yml', '.maestro/expand_test.yml']
```

This workflow builds an **.apk** for Android using the `e2e-test` build profile from the previous step. Then it runs the **.maestro/home.yml** flow on the built APK.

Here's an example of the same test workflow for iOS:

```yaml
name: e2e-test-ios

on:
  pull_request:
    branches: ['*']

jobs:
  build_ios_for_e2e:
    type: build
    params:
      platform: ios
      profile: e2e-test # your eas build profile for E2E test

  maestro_test:
    needs: [build_ios_for_e2e]
    type: maestro
    params:
      build_id: ${{ needs.build_ios_for_e2e.outputs.build_id }}
      flow_path: ['.maestro/home.yml', '.maestro/expand_test.yml']
```

Learn more about [Syntax for EAS Workflows](/eas/workflows/syntax.md).

## Run the E2E test workflow

You can run the E2E test workflow in two ways:

1.  **Manually using the EAS CLI**

```sh
eas workflow:run .eas/workflows/e2e-test-android.yml
```

2.  **Automatically when you open a pull request**

The workflow uses a `pull_request` trigger to run automatically when someone opens a pull request to your repository. Learn more about [EAS Workflows triggers](/eas/workflows/syntax.md#on).

After the workflow starts, you can track its progress and view the results in the EAS dashboard. Here's a screenshot of a completed workflow run:

## More

[Syntax for EAS Workflows](/eas/workflows/syntax.md) — Learn more about the syntax for EAS Workflows.

[Example CI/CD workflows](/eas/workflows/examples/introduction.md) — Learn more about example CI/CD workflows for EAS Workflows.

[Maestro insights](/eas-insights/maestro.md) — Track pass, flake, and failure trends for your Maestro tests over time.

[Maestro documentation](https://docs.maestro.dev/) — Learn more about Maestro flows and how to write them.
