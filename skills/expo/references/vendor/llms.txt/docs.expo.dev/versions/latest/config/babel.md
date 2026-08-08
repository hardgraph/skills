---
title: babel.config.js
description: A reference for Babel configuration file.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/config/babel/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/config/babel/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Reference (v57.0.0) > Configuration files
Pages in this section:
- [app.json / app.config.js](https://docs.expo.dev/versions/latest/config/app.md)
- [babel.config.js](https://docs.expo.dev/versions/latest/config/babel.md) (this page)
- [metro.config.js](https://docs.expo.dev/versions/latest/config/metro.md)
- [package.json](https://docs.expo.dev/versions/latest/config/package-json.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# babel.config.js

A reference for Babel configuration file.

Babel is used as the JavaScript compiler to transform modern JavaScript (ES6+) into a version compatible with the JavaScript engine on mobile devices.

Each new Expo project created using `npx create-expo-app` configures Babel automatically and uses [`babel-preset-expo`](https://github.com/expo/expo/tree/main/packages/babel-preset-expo) as the default preset. There is no need to create a **babel.config.js** file unless you need to customize the Babel configuration.

## Create babel.config.js

If your project requires a custom Babel configuration, you need to create the **babel.config.js** file in your project by following the steps below:

1.  Navigate to the root of your project and run the following command inside a terminal. This will generate a **babel.config.js** file in the root of your project.

```sh
npx expo customize babel.config.js
```

2.  The **babel.config.js** file contains the following default configuration:

```js
module.exports = function (api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo'],
  };
};
```

3.  If you make a change to the **babel.config.js** file, you need to restart the Metro bundler to apply the changes and use `--clear` option from Expo CLI to clear the Metro bundler cache:

```sh
npx expo start --clear
```

## babel-preset-expo

[`babel-preset-expo`](https://github.com/expo/expo/tree/main/packages/babel-preset-expo) is the default preset used in Expo projects. It extends the default React Native preset (`@react-native/babel-preset`) and adds support for decorators, tree-shaking web libraries, and loading font icons.
