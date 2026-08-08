---
modificationDate: June 18, 2026
title: Argent and Expo
description: Use Argent to let your AI agent control, debug, and profile your Expo project on Android Emulators and iOS Simulators.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/agents/argent/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/agents/argent/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > AI > Agent toolkits
Pages in this section:
- [agent-device](https://docs.expo.dev/agents/agent-device.md)
- [Argent](https://docs.expo.dev/agents/argent.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Argent and Expo

Use Argent to let your AI agent control, debug, and profile your Expo project on Android Emulators and iOS Simulators.

[Argent](https://argent.swmansion.com) is an agentic toolkit from Software Mansion. Where an AI agent like Claude Code or Codex reads your code and documentation, Argent gives that same agent direct access to your running app: it can **control, debug, and profile** your Expo app on an Android Emulator or iOS Simulator. You set it up once and connect it to your editor through the [Model Context Protocol (MCP)](/mcp.md).

> Argent is built and maintained by [Software Mansion](https://swmansion.com) and is free to use. For the complete list of its capabilities, see the [Argent website](https://argent.swmansion.com).

#### Prerequisites

##### Node.js 18 or later

Argent's CLI requires Node.js version 18 or newer.

##### A running Android Emulator or iOS Simulator

For Android, install the [Android SDK Platform Tools (`adb`)](/workflow/android-studio-emulator.md#set-up-android-studio), make sure `adb` is on your `PATH`, and have an emulator available. For iOS, use macOS with [Xcode installed](/workflow/ios-simulator.md#setup-xcode-and-watchman).

## Quick start

### Set up Argent

From your Expo project's root directory, run the init command. The wizard detects your editor, registers the Argent MCP server, and copies its skills and agent definitions into your workspace.

```sh
# npm
npx @swmansion/argent init

# yarn
yarn dlx @swmansion/argent init

# pnpm
pnpm dlx @swmansion/argent init

# bun
bunx @swmansion/argent init
```

Your editor launches the Argent MCP server with the `argent` command, so install Argent globally and make sure that command is on your `PATH`. Then restart your editor so it picks up the new configuration.

```sh
# npm
npm install -g @swmansion/argent

# yarn
yarn global add @swmansion/argent

# pnpm
pnpm add -g @swmansion/argent

# bun
bun add -g @swmansion/argent
```

### Run your app on an emulator or simulator

Argent acts on a running app, so start your Expo app on an Android Emulator or iOS Simulator. Use a [development build](/develop/development-builds/introduction.md) or test in Expo Go, with the development server running.

```sh
npx expo run:android
npx expo run:ios
npx expo start
```

### Prompt your agent

Open your project in your editor, then open its agent panel and describe what you want to do with the running app. For example, ask it to launch the app, tap a button, and read the logs.

### Verify setup

In your agent panel, enter the following prompt to confirm Argent can reach the running app:

```text
Take a screenshot of the running app and describe what's on screen.
```

If the agent returns a screenshot and describes your app's current screen, Argent is connected correctly.

## What Argent lets your agent do

Once connected, your agent can work against the live app on the emulator or simulator:

-   **Control**: launch the app, tap, swipe, type into fields, open deep links, and navigate through accessibility trees to run multi-step flows.
-   **Debug**: read console logs, explore the view hierarchy, inspect the React component tree, and examine network requests and their payloads at both the JavaScript and native layers.
-   **Profile**: record React and native profiles together, trace slow React commits to the native stack frames behind them, and surface UI hangs, render cascades, and memory leaks.

## Example prompts

After setup, describe tasks in plain language. For example:

| Task | Example prompt |
| --- | --- |
| Smoke-test a flow | Launch the app, tap through onboarding, and tell me where it breaks from the logs. |
| Verify the UI | Take a screenshot of the home screen and confirm the product list rendered. |
| Debug a network call | Open the cart screen and show me the failing request and its response payload. |
| Inspect React state | Open the settings screen and show me the React component tree for the toggle row. |
| Profile a slowdown | Profile scrolling the products list and point me to the slowest commit. |
| Test a deep link | Open the app through its deep link and confirm the right screen loads. |

## Use Argent with the rest of the Expo AI tooling

Argent connects through MCP, so it works alongside Expo's own agent tooling. Pair it with Expo Skills and the Expo MCP Server so your agent knows Expo conventions while Argent drives the app.

[Expo MCP Server](/mcp.md#installation-and-setup) — Connect the remote Expo MCP Server to give agents live access to Expo documentation and EAS.

[Expo Skills](/skills.md#install-expo-skills) — Install the plugin that teaches agents known-good Expo patterns.

## Manage Argent

```sh
argent update
argent flags
argent remove
```

## Limitations and tips

-   Argent supports Android Emulators and iOS Simulators.
-   Let Argent launch or relaunch the app on the device when it can. If you boot the device yourself, Argent may not be able to see system dialogs or native modals.
-   In Expo Go, Argent can control the app, read the React component tree, and run the React profiler. Native profiling needs a development build, because in Expo Go it profiles the Expo Go host app instead of your code.
-   The React component tree and the React profiler rely on the JavaScript debugging connection, so keep your development server running.
-   For the complete and current list of capabilities, see the [Argent documentation](https://argent.swmansion.com).
