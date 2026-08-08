---
modificationDate: July 08, 2026
title: Share previews with your team
description: Learn how to use EAS Update to send OTA updates and share previews with a team.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/eas/team-development/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/eas/team-development/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Share previews](https://docs.expo.dev/tutorial/eas/team-development.md) (this page)
- [Builds from GitHub](https://docs.expo.dev/tutorial/eas/using-github.md)
- [Next steps](https://docs.expo.dev/tutorial/eas/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Share previews with your team

Learn how to use EAS Update to send OTA updates and share previews with a team.

Updates generally fix small bugs and push small changes in between app store releases. They allow updating the non-native parts of our example app, such as JavaScript code, styling, and images.

In this chapter, we'll use [EAS Update](/eas-update/introduction.md) to share changes with our team. This will help [us and our team quickly share previews](/review/overview.md) of the change.

[Watch: How to share previews with your team](https://www.youtube.com/watch?v=vPKh-tNm-yI) — Set up EAS Update to share preview versions of your app with your team between app store releases.

## Install expo-updates library

To initialize our project and send an update, we need to use the [`expo-updates`](/versions/latest/sdk/updates.md) library. Run the following command to install it:

```sh
# npm
npx expo install expo-updates

# yarn
yarn expo install expo-updates

# pnpm
pnpm expo install expo-updates

# bun
bun expo install expo-updates
```

## Configure EAS Update

To initialize our project with EAS Update, we need to follow these steps:

-   Since we are using dynamic **app.config.js** for our app's configuration, adding [`updates`](/versions/latest/config/app.md#updates) and [`runtimeVersion`](/eas-update/runtime-versions.md#setting-runtimeversion) properties are required to make our project compatible with EAS Update. Run the following command to obtain these properties and their values from EAS and manually copy them to **app.config.js**:

```sh
eas update:configure
```

#### What about non-dynamic (app.json) projects?

If a project doesn't use dynamic app config (uses **app.json** instead of **app.config.js**), the above command will configure our app to be compatible with EAS Update and add the right properties to **app.json** and **eas.json**.

-   Re-run `eas update:configure` to continue with the setup process. A [`channel`](/eas/json.md#channel) should be added to every build profile in **eas.json**:

```json
{
  "build": {
    "development": {
      ... 
      "channel": "development"
    },
    "ios-simulator": {
      ... 
    },
    "preview": {
      ... 
      "channel": "preview"
    },
    "production": {
      ... 
      "channel": "production"
    }
  }
  ... 
}
```

> Notice that the `eas update:configure` command adds the `channel` to every build profile in **eas.json**. However, our `ios-simulator` profile extends the `development` profile and having a separate `channel` doesn't make sense. We can safely remove `ios-simulator.channel` from the above configuration.

#### What is a channel?

[Channels](/eas-update/how-it-works.md#conceptual-overview) are used to group builds together. If we have an Android and iOS build, both on the app store, we can give them both a channel of production. Later, we can tell EAS Update to target the production channel, so our update will affect all builds with a production channel.

## Create a development build

We need to create a new development build since our last build doesn't contain the `expo-updates` library. Run the following command:

```sh
eas build --platform android --profile development
```

> We are using a development build for Android devices to demonstrate updates. However, we can use `--platform all` or `--platform ios` to create a build for both platforms or just for iOS.

After the new version of the development build is created, make sure to install it on a device.

## Modify the JavaScript code of the app

Let's modify our example app's JavaScript code. If you are not using [Sticker Smash app](/tutorial/eas/introduction.md#prerequisites), you can modify any piece of your code to see the changes in the app.

We'll modify the text of the first button in our example app that says **Choose a photo** to **Select a photo**.

```tsx
<Button theme="primary" label="Select a photo" onPress={pickImageAsync} />
```

## Publish an update

Instead of creating a new build to share this change with our team for testing, let's publish an update:

```sh
eas update --channel development --message "Change first button label"
```

In the command above, we used the `development` channel. Every update is associated with a [channel name](/eas-update/how-it-works.md#publishing-an-update). It is similar to every commit that we make with git, which is associated with a git branch.

So, by using the channel `development` in our build profile and then publishing an, we're asking EAS to deliver this update to builds with the `development` channel. When we make an EAS Update channel it automatically gets mapped to a branch with the same name.

After the update is published, the CLI will prompt us with information about it.

Click on the **Website link** to see the Update on the EAS dashboard under **Over-the-air updates** > **Update groups**:

## Preview the update live in a development build

To preview the live update in a development build:

-   Log in to your Expo account within the development build.
-   Open the **Extensions** tab.
-   Look for **Branch: development** listed under **EAS Update**.
-   Tap on **Open** to access the update.

## Sharing changes with preview or production builds

Updates for non-development builds (preview or production) are automatically downloaded to the device when the app starts up and makes a request for any new updates.

Any team member running the preview or production build will receive the update with the changes we push to those specific branches.

For example, for a `preview` build, we can run:

```sh
eas update --channel preview --message "Change first button label"
```

Here is an example where we've published an update for the `preview` build. To test the update, force close and reopen the app twice to download and view the changes:

## Summary

Chapter 10: Share previews with your team

We successfully configured EAS Update to manage and publish over-the-air updates across platforms, and explored methods to fetch updates to review.

In the next chapter, learn about the process of triggering builds from a GitHub repository.

[Next: Chapter 11: Trigger builds from a GitHub repository](/tutorial/eas/using-github.md)
