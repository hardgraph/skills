---
modificationDate: February 10, 2025
title: expo-module.config.json
description: Learn about different configuration options available in expo-module.config.json.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/module-config/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/module-config/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Modules API > Reference
Pages in this section:
- [Module API](https://docs.expo.dev/modules/module-api.md)
- [Inline modules reference](https://docs.expo.dev/modules/inline-modules-reference.md)
- [Type generation reference](https://docs.expo.dev/modules/type-generation-reference.md)
- [Android lifecycle listeners](https://docs.expo.dev/modules/android-lifecycle-listeners.md)
- [iOS AppDelegate subscribers](https://docs.expo.dev/modules/appdelegate-subscribers.md)
- [Autolinking](https://docs.expo.dev/modules/autolinking.md)
- [Shared objects](https://docs.expo.dev/modules/shared-objects.md)
- [expo-module.config.json](https://docs.expo.dev/modules/module-config.md) (this page)
- [Mocking native calls](https://docs.expo.dev/modules/mocking.md)
- [Design considerations](https://docs.expo.dev/modules/design.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# expo-module.config.json

Learn about different configuration options available in expo-module.config.json.

Expo modules are configured in **expo-module.config.json**. This file currently is capable of configuring autolinking and module registration. The following properties are available:

-   `platforms` — An array of supported platforms. Acceptable values are `android`, `apple` (or use the more granular `ios` / `macos` / `tvos`), `web` and `devtools` (see [Create a dev tools plugin](/debugging/create-devtools-plugins.md)).
-   `apple` — Config with options specific to Apple platforms
    -   `modules` — Names of Swift native modules classes to put to the generated modules provider file.
    -   `appDelegateSubscribers` — Names of Swift classes that hook into `ExpoAppDelegate` to receive AppDelegate lifecycle events.
-   `android` — Config with options specific to Android platform
    -   `modules` — Full names (package + class name) of Kotlin native modules classes to put to the generated package provider file.
