---
modificationDate: June 30, 2026
title: Learning resources
description: Explore a curated list of resources to learn about Expo and React Native.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/follow-up/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/follow-up/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > Expo tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/introduction.md)
- [Create your first app](https://docs.expo.dev/tutorial/create-your-first-app.md)
- [Add navigation](https://docs.expo.dev/tutorial/add-navigation.md)
- [Build a screen](https://docs.expo.dev/tutorial/build-a-screen.md)
- [Use an image picker](https://docs.expo.dev/tutorial/image-picker.md)
- [Create a modal](https://docs.expo.dev/tutorial/create-a-modal.md)
- [Add gestures](https://docs.expo.dev/tutorial/gestures.md)
- [Take a screenshot](https://docs.expo.dev/tutorial/screenshot.md)
- [Handle platform differences](https://docs.expo.dev/tutorial/platform-differences.md)
- [Configure status bar, splash screen and app icon](https://docs.expo.dev/tutorial/configuration.md)
- [Learning resources](https://docs.expo.dev/tutorial/follow-up.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Learning resources

Explore a curated list of resources to learn about Expo and React Native.

Now that the example app is done, let's learn more about the technologies we used to build it.

## Build your project into an app

To start creating a new app on your machine you can use `npx create-expo-app@latest --template default@sdk-57` and [set up your development environment](/get-started/set-up-your-environment.md) sequentially.

> **Note:** During the SDK 57 transition period, `create-expo-app@latest` without the `--template` flag creates an SDK 54 project. If you plan to use Expo Go on a physical device, use an SDK 54 project. Otherwise, use `--template default@sdk-57` to create an SDK 57 project.

### Recommended resources

Once you've created your new project, you can learn more about different tools and concepts that will help you on your app development journey:

-   [Development tools](/develop/tools.md): A reference of Expo tools that will help you during various aspects of your app-building journey.
-   [Development builds](/develop/development-builds/introduction.md): Using a development build allows you to gain full control over your app's build process, and to test your app on a device or simulator.
-   [Development overview](/workflow/overview.md): This is a high-level overview that provides details on key concepts for developing an app with Expo and the flow of core development loop.
-   [Expo Router](/router/introduction.md): We went through basics of Expo Router and implemented a tab navigator. See its documentation to learn more about the library.
-   [App icon](/develop/user-interface/splash-screen-and-app-icon.md#app-icon) and [splash screen](/develop/user-interface/splash-screen-and-app-icon.md#splash-screen): You can learn more about customizing your app icon and splash screen guides. Also, look through the [app config reference](/workflow/configuration.md) for properties you can configure in the **app.json** file.
-   [App distribution](/deploy/build-project.md) and [submission](/deploy/submit-to-app-stores.md) to app stores: Read these resources to learn more about how to release and submit your app to app stores once it's ready to ship.
-   [Debugging](/debugging/runtime-issues.md): Sometimes things go wrong, and when they do, you can use debugging tools to find and fix errors.

## Learning

### React

We used React components and APIs. Having a solid understanding of React is essential to using Expo to build your app. We recommend reading the React documentation's [Quick Start section](https://react.dev/learn) and the [Hooks section](https://react.dev/reference/react/hooks).

### React Native

While developing the tutorial app, we used React Native extensively. You can start from the [React Native basics guide](https://reactnative.dev/docs/getting-started) to learn more. Also, check out the following docs:

-   [View API reference](https://reactnative.dev/docs/view)
-   [Text API reference](https://reactnative.dev/docs/text)
-   [Platform specific code](https://reactnative.dev/docs/platform-specific-code)
-   [Presenting data in a list](https://reactnative.dev/docs/using-a-listview)

We used Flexbox to layout our components. Check out the following recommendations to learn more about it:

-   [Height and Width](https://reactnative.dev/docs/height-and-width)
-   [Layout with Flexbox](https://reactnative.dev/docs/flexbox)

### Gestures and animations

To learn more about implementing different types of gestures and animations, we recommend the following documentation:

-   [React Native Gesture Handler](https://docs.swmansion.com/react-native-gesture-handler/docs/)
-   [React Native Reanimated](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/getting-started)

## Join the community

Join our community on [Discord](https://chat.expo.dev/) to chat with other Expo users or to ask questions.
