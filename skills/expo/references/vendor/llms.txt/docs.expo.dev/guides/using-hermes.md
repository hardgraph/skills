---
modificationDate: July 29, 2026
title: Using Hermes engine
description: A guide on configuring Hermes for both Android and iOS in an Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/using-hermes/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/using-hermes/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Assorted
Pages in this section:
- [Authentication with OAuth or OpenID providers](https://docs.expo.dev/guides/authentication.md)
- [Using Hermes](https://docs.expo.dev/guides/using-hermes.md) (this page)
- [iOS Developer Mode](https://docs.expo.dev/guides/ios-developer-mode.md)
- [Expo Vector Icons](https://docs.expo.dev/guides/icons.md)
- [Localization](https://docs.expo.dev/guides/localization.md)
- [Using Bun](https://docs.expo.dev/guides/using-bun.md)
- [Edit rich text](https://docs.expo.dev/guides/editing-richtext.md)
- [App store assets](https://docs.expo.dev/guides/store-assets.md)
- [Local-first](https://docs.expo.dev/guides/local-first.md)
- [Keyboard handling](https://docs.expo.dev/guides/keyboard-handling.md)
- [Controlled components](https://docs.expo.dev/guides/controlled-components.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Hermes engine

A guide on configuring Hermes for both Android and iOS in an Expo project.

[Hermes](https://hermesengine.dev/) is a JavaScript engine optimized for React Native. By compiling JavaScript into bytecode ahead of time, Hermes can improve your app start-up time. The binary size of Hermes is also smaller than other JavaScript engines, such as JavaScriptCore (JSC). It also uses less memory at runtime, which is particularly valuable on lower-end Android devices.

## Support

The Hermes engine is the default JavaScript engine used by Expo and it is fully supported across all Expo tooling.

### Switch JavaScript engine on a specific platform

You may want to use Hermes on one platform and JSC on another. One way to do this is to set the `"jsEngine"` to `"hermes"` at the top level in app config and then override it with `"jsc"` under the `"ios"` key. You may alternatively prefer to explicitly set `"hermes"` on just the `"android"` key in this case.

```json
{
  "expo": {
    "jsEngine": "hermes",
    "ios": {
      "jsEngine": "jsc"
    }
  }
}
```

## Publish updates

Publishing updates with `eas update` and `npx expo export` will generate Hermes bytecode bundles and their source maps.

Note that the Hermes bytecode format may change between different Hermes versions — an update produced for a specific version of Hermes will not run on a different version of Hermes. Starting from Expo SDK 46 (React Native 0.69), [Hermes is bundled within React Native](https://reactnative.dev/architecture/bundled-hermes). Updating React Native version or Hermes version can be thought of in the same way as updating any other native module. So if you update the `react-native` version you should also update the `runtimeVersion` in **app.json**. If you don't do this, your app may crash on launch because the update may be loaded by an existing binary that uses an older Hermes version that is incompatible with the updated bytecode format. See [`runtimeVersion`](/eas-update/runtime-versions.md) for more information.

## JavaScript debugger

To debug JavaScript code running with Hermes, you can start your project with `npx expo start` then press J to open the debugger in Google Chrome or Microsoft Edge. The developer menu of development builds and Expo Go also have the **Open DevTools** (formerly **Open JS Debugger**) option to do the same.

Alternatively, you can use the JavaScript inspector by opening [Google Chrome DevTools manually](https://reactnative.dev/docs/other-debugging-methods#remote-javascript-debugging-deprecated)

### Troubleshooting

> `No compatible apps connected. JavaScript Debugging can only be used with the Hermes engine.` when opening the debugger.

-   Make sure you [set up Hermes in the `jsEngine` field](/guides/using-hermes.md#switch-javascript-engine-on-a-specific-platform).
    
-   If your app is built by `eas build`, `npx expo run:android` or `npx expo run:ios`, make sure it is a debug build.
    
-   Internally, the app will establish a WebSocket connection, make sure your app is connected to the development server.
    
    -   Try to reload the app by pressing R in the Expo CLI Terminal UI.
    -   Test debugging availability by running the command: `curl http://127.0.0.1:8081/json/list` (adjust the `127.0.0.1:8081` to match your dev server URL). The HTTP response should be an array, as shown below. If it is an empty response, add either the `--localhost` or `--tunnel` flag to the `npx expo start` command.
    
    ```json
    [
      {
        "id": "0-2",
        "description": "host.exp.Exponent",
        "title": "Hermes ABI47_0_0React Native",
        "faviconUrl": "https://react.dev/favicon.ico",
        "devtoolsFrontendUrl": "devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&ws=%5B%3A%3A1%5D%3A8081%2Finspector%2Fdebug%3Fdevice%3D0%26page%3D2",
        "type": "node",
        "webSocketDebuggerUrl": "ws://[::1]:8081/inspector/debug?device=0&page=2",
        "vm": "Hermes"
      },
      {
        "id": "0--1",
        "description": "host.exp.Exponent",
        "title": "React Native Experimental (Improved Chrome Reloads)",
        "faviconUrl": "https://react.dev/favicon.ico",
        "devtoolsFrontendUrl": "devtools://devtools/bundled/js_app.html?experiments=true&v8only=true&ws=%5B%3A%3A1%5D%3A8081%2Finspector%2Fdebug%3Fdevice%3D0%26page%3D-1",
        "type": "node",
        "webSocketDebuggerUrl": "ws://[::1]:8081/inspector/debug?device=0&page=-1",
        "vm": "don't use"
      }
    ]
    ```
    

### Can I use Remote Debugging with Hermes?

One of the many limitations of [remote debugging](/more/glossary-of-terms.md#remote-debugging) is that it does not work with modules built on top of [JSI](https://github.com/react-native-community/discussions-and-proposals/issues/91), such as [`react-native-reanimated`](https://github.com/software-mansion/react-native-reanimated) version 2 or higher.

Hermes supports [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/v8/) to debug JavaScript in place by connecting to the engine running on the device, as opposed to remote debugging, which executes JavaScript within a desktop Chrome tab. Hermes apps use this debugging technique automatically when you open the debugger in Expo Go or a development build.
