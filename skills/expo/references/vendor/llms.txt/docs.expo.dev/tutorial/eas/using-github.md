---
modificationDate: July 08, 2026
title: Trigger builds from a GitHub repository
description: Learn about the process of triggering builds from a GitHub repository.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/using-github/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/using-github/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > EAS tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/eas/introduction.md)
- [Configure development build](https://docs.expo.dev/tutorial/eas/configure-development-build.md)
- [Android development build](https://docs.expo.dev/tutorial/eas/android-development-build.md)
- [iOS development build for simulators](https://docs.expo.dev/tutorial/eas/ios-development-build-for-simulators.md)
- [iOS development build for devices](https://docs.expo.dev/tutorial/eas/ios-development-build-for-devices.md)
- [Multiple app variants](https://docs.expo.dev/tutorial/eas/multiple-app-variants.md)
- [Internal distribution build](https://docs.expo.dev/tutorial/eas/internal-distribution-builds.md)
- [Manage app versions](https://docs.expo.dev/tutorial/eas/manage-app-versions.md)
- [Android production build](https://docs.expo.dev/tutorial/eas/android-production-build.md)
- [iOS production build](https://docs.expo.dev/tutorial/eas/ios-production-build.md)
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md) (this page)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Trigger builds from a GitHub repository

Learn about the process of triggering builds from a GitHub repository.

[Expo GitHub App](/build/building-from-github.md) automatically triggers builds from our GitHub projects with EAS. We can trigger builds for any build profile based on our development team's preferences. It also allows triggering builds for `git` push committed directly to a repository or a pull request.

In this chapter, we'll configure this functionality. We already have a GitHub repository for our example app to demonstrate this.

[Watch: How to trigger builds from a GitHub repository](https://www.youtube.com/watch?v=fBLFEFC0ip0) — Connect the Expo GitHub App to your repository and configure it to trigger EAS builds on push or pull request.

## Configure Expo GitHub app

To use this functionality, we need to connect our GitHub account:

-   In the EAS dashboard, go to [expo.dev/settings](https://expo.dev/settings#connections), and under **Connections** > **GitHub**, click **Connect**. This opens the **Connect GitHub** accounts page.
-   Click the **Get started** button which opens a popup to authorize the Expo GitHub app. Click **Install and Authorize**.
-   Once the app is installed on our GitHub account, we need to link it to our Expo account. In the next popup, click **Link installation**.
-   Once the account is linked, it will show under **GitHub**.

## Connect the GitHub repository

To enable triggering builds from a GitHub repository, we need to connect it to our project in the EAS dashboard:

-   In EAS dashboard, go to **Projects** > Select your project > **Project settings** > **GitHub**.
-   Under **Connect a GitHub repository**, we'll see a list of our GitHub repos. We need to connect the right one. In the example, we're searching for our repo **sticker-smash.**
-   Click **Connect** for the project repository.

## Use default repository settings

The Expo GitHub app needs to know where to find the source code of our project. By default, it selects the root directory using `/`. In our example project, the source code is also available in the root repository. We can leave this to default in the EAS dashboard.

## Trigger a build using a GitHub PR label

The Expo GitHub app provides us [multiple options](/build/building-from-github.md#trigger-a-build-from-github) to trigger a build, such as:

-   Manually from the Builds page for a specific platform
-   Automatically when new code is pushed to the repository
-   Automatically using GitHub PR labels

To automatically trigger a build using a GitHub PR label, we're going to utilize the third option from the list above:

-   We need to specify the build image that we will be using. Open **eas.json**, and under the `development` profile, add [`android.image`](/eas/json.md#image) and [`ios.image`](/eas/json.md#image-1) properties and set their value to [`latest`](/build-reference/infrastructure.md#configuring-build-environment).
    
    ```json
    {
      "build": {
        "development": {
          ... 
          "android": {
            "image": "latest"
          },
          "ios": {
            "image": "latest"
          }
        }
      }
      ... 
    }
    ```
    
-   Next, let's create a new branch called `dev`, and make a change in our app's JavaScript code. Then, commit the change, push the branch, and create a PR from that branch.
    
-   In the PR link, under **Labels**, create a label called `eas-build-all:development`.
    

-   Click **Create pull request** button to create the PR. The Expo GitHub app will start the process of creating a development build.
    
-   In the EAS dashboard, on the **Builds** page, we can verify that the builds for both Android and iOS are triggered.
    

-   If we check the details of an individual build, we can see under **Created by** that the build is created by the GitHub app.

## Summary

Chapter 11: Trigger builds from a GitHub repository

We successfully linked our GitHub account with Expo, connected our repository to our EAS project, and learned about automated development build creation using GitHub PR labels.

Learn about the next steps to use EAS.

[Next: Next steps in your journey with EAS](/tutorial/eas/next-steps.md)
