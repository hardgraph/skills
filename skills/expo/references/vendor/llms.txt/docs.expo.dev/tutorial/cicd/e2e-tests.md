---
modificationDate: July 08, 2026
title: 'Run E2E tests with Maestro on EAS Workflows'
description: Learn how to automate end-to-end (E2E) tests with Maestro using Android and iOS development builds on EAS Workflows.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/e2e-tests/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/e2e-tests/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > CI/CD tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/cicd/introduction.md)
- [First EAS Workflows job](https://docs.expo.dev/tutorial/cicd/first-workflow.md)
- [Development builds](https://docs.expo.dev/tutorial/cicd/development-builds.md)
- [Preview builds](https://docs.expo.dev/tutorial/cicd/preview-builds.md)
- [E2E tests](https://docs.expo.dev/tutorial/cicd/e2e-tests.md) (this page)
- [Production deployments](https://docs.expo.dev/tutorial/cicd/production.md)
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Run E2E tests with Maestro on EAS Workflows

Learn how to automate end-to-end (E2E) tests with Maestro using Android and iOS development builds on EAS Workflows.

Broken navigation flows and crashing screens often slip past code review because reviewers cannot run the app on every pull request.

## Learning outcomes

-   Write [Maestro](https://maestro.mobile.dev/) flow files for navigation and screen-content tests
-   Run E2E tests against Android and iOS [development builds](/develop/development-builds/introduction.md) in a workflow
-   Trigger E2E tests automatically on pull requests with `on.pull_request` or on demand with `on.pull_request_labeled`

> The Maestro job type in EAS Workflows is currently in [alpha](/more/release-statuses.md#alpha).

## Why are E2E tests useful?

The pipeline from previous chapters builds our project and delivers updates, with Slack notifications. However, a broken navigation flow or a crashing screen can still make it to production silently. E2E tests can help catch these issues before they reach production by simulating real user interactions and verifying that the app behaves as expected.

[Maestro](https://maestro.mobile.dev/) runs automated flows against a built app on an Android Emulator or an iOS Simulator. It can tap buttons, type text, assert screen content, and navigate between screens. EAS Workflows runs these flows in the cloud after building an APK for Android or an iOS Simulator build.

## Set up E2E tests

For Android and iOS, we need to add a special [build profile](/build/eas-json.md) for E2E tests. This profile skips credential setup for Android and allows running our iOS build inside a simulator.

### Add the e2e-test build profile

Add an `e2e-test` profile to **eas.json**. This profile creates an unsigned APK for Android and a simulator build for iOS:

```json
{
  ... 
  "build": {
    "e2e-test": {
      "withoutCredentials": true,
      "android": {
        "buildType": "apk",
        "image": "latest"
      },
      "ios": {
        "simulator": true,
        "image": "latest"
      }
    }
  }
}
```

### Add Maestro flow files

Create a **.maestro/** directory at the root of the Expo project and then add the following test files.

For example, the following code snippet adds a basic home screen test that launches the app and checks that the "Welcome" text is visible. Replace `com.yourname.yourapp` in `appId` with our app's [`android.package`](/versions/latest/config/app.md#package) on Android or [`ios.bundleIdentifier`](/versions/latest/config/app.md#bundleidentifier) on iOS, both defined in the **app.json**:

```yaml
appId: com.yourname.yourapp # Replace with your app's package name/bundle identifier from app config file
---
- launchApp
- assertVisible: 'Welcome'
```

Then, we can add another test for the settings screen:

```yaml
appId: com.yourname.yourapp # Replace with your app's package name/bundle identifier from app config file
---
- launchApp
- tapOn: 'Settings'
- assertVisible: 'Settings'
```

## Create a workflow for E2E tests

### Create an e2e-tests.yml

Inside **.eas/workflows/**, add a new file called **e2e-tests.yml** with the following code:

```yaml
name: E2E Tests

jobs:
  build_android:
    name: Build Android for E2E
    type: build
    params:
      platform: android
      profile: e2e-test
  build_ios:
    name: Build iOS for E2E
    type: build
    params:
      platform: ios
      profile: e2e-test
  test_android:
    name: Run Maestro tests (Android)
    needs: [build_android]
    type: maestro
    params:
      build_id: ${{ needs.build_android.outputs.build_id }}
      flow_path: ['.maestro/home.yml', '.maestro/navigate.yml']
  test_ios:
    name: Run Maestro tests (iOS)
    needs: [build_ios]
    type: maestro
    params:
      build_id: ${{ needs.build_ios.outputs.build_id }}
      flow_path: ['.maestro/home.yml', '.maestro/navigate.yml']
```

In the above workflow, there are two build jobs for Android and iOS: `build_android` and `build_ios`. After each build, a corresponding Maestro job (`test_android` or `test_ios`) runs the tests for that platform.

Each Maestro job references the build via `build_id` and points at the Maestro flow files via `flow_path`.

### Run the workflow manually

Use the `eas workflow:run` command to run the workflow manually:

```sh
eas workflow:run .eas/workflows/e2e-tests.yml
```

On the EAS dashboard, notice that the Android and iOS builds start in parallel. Once each build completes, the corresponding Maestro test job runs the flow files against it.

## Trigger E2E tests automatically

So far we've run the workflow manually. We can trigger it from GitHub events so it runs on every pull request or on demand with a pull request label.

### Run on every pull request

To run E2E tests on every pull request, add an `on.pull_request` trigger to the workflow:

```yaml
on:
  pull_request:
    branches: ['*']
```

### Run on demand with a label

To run E2E tests only when needed, use the [`on.pull_request_labeled`](/eas/workflows/syntax.md#onpull_request_labeled) trigger. It accepts a `labels` parameter, and the workflow only runs when someone adds one of those labels to a pull request. For example, we can use the `test` label to trigger our E2E tests workflow:

```yaml
on:
  pull_request_labeled:
    labels: ['test']
```

## Summary

Chapter 4: E2E tests

We created a Maestro E2E test workflow on development builds for Android and iOS, kept the trigger manual by default, and learned how to use a pull request label trigger for on-demand runs.

In the next chapter, learn how to create a production workflow with fingerprinting and OTA updates.

[Next: Chapter 5: Production deployments](/tutorial/cicd/production.md)
