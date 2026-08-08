---
modificationDate: July 08, 2026
title: 'Automate production deployments with EAS Workflows'
description: Learn how to automate production builds and over-the-air updates with EAS Workflows from a release branch for Android and iOS.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/cicd/production/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/cicd/production/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Production deployments](https://docs.expo.dev/tutorial/cicd/production.md) (this page)
- [Tag-based releases](https://docs.expo.dev/tutorial/cicd/tag-based-releases.md)
- [Web deployments](https://docs.expo.dev/tutorial/cicd/web-deployments.md)
- [Next steps](https://docs.expo.dev/tutorial/cicd/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Automate production deployments with EAS Workflows

Learn how to automate production builds and over-the-air updates with EAS Workflows from a release branch for Android and iOS.

Rebuilding the app for every production change is wasteful when most changes are TypeScript/JavaScript only.

## Learning outcomes

-   Trigger production deployments from a `release/*` branch instead of every push to `main`
-   Branch between a native build and an [OTA update](/deploy/send-over-the-air-updates.md) based on the project fingerprint
-   Automate app store submissions as a follow-up step in the production workflow

#### Prerequisites

##### EAS Build configured for production

[EAS CLI](/eas/cli.md) prompts to generate credentials the first time we trigger a build for a profile. Trigger the production build manually for both Android and iOS so the credentials exist before the workflow runs:

```sh
eas build --profile production --platform all
```

## Using a release branch

Previously, with development builds, we have seen that using the `on.push.branches` trigger runs our workflow on every push to the specified `main` branch. For production deployments, triggering a workflow on every push to the `main` branch means every change merged is a new release. Teams usually want a deliberate release process, so they use a release branch or tag instead.

In a release branch strategy, we usually specify the branch with a pattern such as `release/*`, where `*` is the feature or version name. This is what separates Continuous Integration (CI) from Continuous Delivery (CD). CI runs on every push to the `main` branch, while CD runs on a release branch when the team is ready to deploy to production.

The table below shows how each workflow's trigger defines its purpose:

| Workflow file | Trigger | Build profile | Purpose |
| --- | --- | --- | --- |
| **.eas/workflows/build.yml** | `on.push.branches: ['main']` | `development` | CI workflow that runs on every push to the `main` branch. It can run unit tests and generate development builds. |
| **.eas/workflows/preview.yml** | `on.push.branches: ['main']` | `preview` | Preview workflow that runs on every push to the `main` branch. It can generate preview builds for internal testing and sharing with stakeholders. |
| **.eas/workflows/production.yml** | `on.push.branches: ['release/*']` | `production` | CD workflow that runs on every push to a release branch. |

The above files coexist inside a project's GitHub repository without any conflict. Each uses a different build profile and a trigger.

```
Production workflow decision: a push to a release branch fires the workflow, fingerprint hashes the project, get-build looks up an existing build with the same hash, then the workflow publishes an OTA update if a match is found, or runs a new native build if not.
```

## The `build` job type for production builds

Let's start by creating a production workflow. It uses the [pre-packaged](/tutorial/cicd/first-workflow.md#types-of-jobs-in-eas-workflows) `build` job type from EAS Workflows and runs for both Android and iOS.

### Add a production.yml

Inside **.eas/workflows/**, create a new file called **production.yml** that uses the `production` build profile from **eas.json**:

```yaml
name: Deploy to production

on:
  push:
    branches: ['release/*']

jobs:
  build_android:
    name: Build Android
    type: build
    params:
      platform: android
      profile: production
  build_ios:
    name: Build iOS
    type: build
    params:
      platform: ios
      profile: production
```

This creates a **.aab** artifact for Android and an **.ipa** artifact for iOS on a push to any branch that matches the pattern `release/*`.

### Add fingerprints to the workflow

Let's add a [`fingerprint`](/eas/workflows/pre-packaged-jobs.md#fingerprint) job to the workflow to check if there are changes to native code and update existing `build_android` and `build_ios` jobs based on that. We are also adding [`get-build`](/eas/workflows/pre-packaged-jobs.md#get-build) jobs to look up existing builds for Android and iOS.

Update the **production.yml** file with the following code:

```yaml
name: Deploy to production

on:
  push:
    branches: ['release/*']

jobs:
  fingerprint:
    name: Fingerprint
    type: fingerprint
    environment: production
  get_android_build:
    name: Check for existing Android build
    needs: [fingerprint]
    type: get-build
    params:
      fingerprint_hash: ${{ needs.fingerprint.outputs.android_fingerprint_hash }}
      profile: production
  get_ios_build:
    name: Check for existing iOS build
    needs: [fingerprint]
    type: get-build
    params:
      fingerprint_hash: ${{ needs.fingerprint.outputs.ios_fingerprint_hash }}
      profile: production
  build_android:
    name: Build Android
    needs: [get_android_build]
    if: ${{ !needs.get_android_build.outputs.build_id }}
    type: build
    params:
      platform: android
      profile: production
  build_ios:
    name: Build iOS
    needs: [get_ios_build]
    if: ${{ !needs.get_ios_build.outputs.build_id }}
    type: build
    params:
      platform: ios
      profile: production
```

In the above workflow, the `fingerprint` job runs first and generates a hash based on the changes in the codebase. The `get-build` jobs then check if there are existing builds for Android and iOS with the same fingerprint hash. If there are no existing builds, the `build` jobs run to create new builds for production.

The [`environment`](/eas/workflows/syntax.md#jobsjob_idenvironment) field on the `fingerprint` job tells EAS which environment variables to load when computing the fingerprint. Our production environment may have different values for variables such as `API_URL` or feature flags. Those values can affect the native layer when EAS bakes them into the build. Setting `environment: production` ensures the fingerprint reflects what ships.

The [`if`](/eas/workflows/syntax.md#jobsjob_idif) field used on the `build_android` and `build_ios` jobs is a boolean expression. When it evaluates to `false`, the build jobs are skipped. The `!` operator means the build job runs only when `get-build` did not find a matching build.

### Add a job for updates

Currently, our workflow only creates production builds when there are changes to native code. If there are only changes to TypeScript/JavaScript files, we want to trigger an OTA update instead of a native build.

Let's add two jobs `update_android` and `update_ios` that run when there are no changes to native code and use `update` job type to trigger an OTA update:

```yaml
name: Deploy to production

on:
  push:
    branches: ['release/*']

jobs:
  fingerprint:
    # ...
  get_android_build:
    # ...
  get_ios_build:
    # ...
  build_android:
    # ...
  build_ios:
    # ...
  update_android:
    name: Publish Android update
    needs: [get_android_build]
    if: ${{ needs.get_android_build.outputs.build_id }}
    type: update
    params:
      branch: production
      platform: android
  update_ios:
    name: Publish iOS update
    needs: [get_ios_build]
    if: ${{ needs.get_ios_build.outputs.build_id }}
    type: update
    params:
      branch: production
      platform: ios
```

Our workflow only builds production-ready app binaries or publishes OTA updates. Automating store submission is a common next step discussed below in the [Automate app store submissions](/tutorial/cicd/production.md#automate-app-store-submissions) section.

### Create a release branch and push changes

> **Note:** Ensure that the workflow file is already part of our GitHub repository and available on the `main` branch at this point.

To test our workflow, let's create a release branch and push it to our GitHub repository from a terminal window:

```sh
git checkout -b release/1.0.0
git push origin release/1.0.0
```

Open the EAS dashboard and find the workflow run. Since no build exists yet for this fingerprint, the `build` jobs should run, and the `update` jobs should be grayed out.

Both Android and iOS builds are running in parallel. Once each job completes, we receive a new artifact for Android and iOS.

Now, let's make a TypeScript/JavaScript only change in our example project on the same release branch and push again using the commands below:

```sh
git add .
git commit -m "Update welcome text"
git push origin release/1.0.0
```

After the workflow runs, notice in the EAS dashboard that the fingerprint matches the build from the last push. The `build` jobs are skipped entirely, and the `update` jobs publish an OTA update.

## Automate app store submissions

The workflow we have built in this chapter only builds production-ready app binaries or publishes OTA updates to an existing production app. [Automating store submission](/build/automate-submissions.md) is a common next step in a production release workflow.

EAS Workflows provides a `submit` pre-packaged job for automating app store submissions. It requires us to manage our app store credentials with EAS CLI. It also requires us to fulfill app store requirements, such as manually uploading the first Android release (**.aab**) to the Google Play Store. iOS has its own Apple Developer Program enrollment and signing credentials to set up.

Once the app store requirements are in place, we can add two new jobs `submit_android` and `submit_ios` to our workflow that run after the `build` jobs:

```yaml
# rest of the workflow file

submit_android:
  name: Submit Android
  needs: [build_android]
  type: submit
  params:
    build_id: ${{ needs.build_android.outputs.build_id }}
submit_ios:
  name: Submit iOS
  needs: [build_ios]
  type: submit
  params:
    build_id: ${{ needs.build_ios.outputs.build_id }}
```

The above `submit_android` and `submit_ios` jobs require a `build_id` parameter to know which build to submit to the app stores. They run only when the `build_android` and `build_ios` jobs run, which means a new production build is created. If there are only changes to TypeScript/JavaScript files, the `submit` jobs are skipped as well since no new build is created.

## Summary

Chapter 5: Production deployments

We created a production workflow for Android and iOS triggered by pushes to a release branch, used fingerprinting to choose between a native build and an OTA update, and saw how EAS Submit automates app store submissions.

In the next chapter, learn how to switch production deployments from release branches to version tags.

[Next: Chapter 6: Tag-based releases](/tutorial/cicd/tag-based-releases.md)
