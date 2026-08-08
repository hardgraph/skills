---
modificationDate: July 16, 2026
title: agent-device and Expo
description: Use agent-device to let your AI coding agent verify, debug, profile, and test a running Expo app across local and remote devices.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/agents/agent-device/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/agents/agent-device/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > AI > Agent toolkits
Pages in this section:
- [agent-device](https://docs.expo.dev/agents/agent-device.md) (this page)
- [Argent](https://docs.expo.dev/agents/argent.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# agent-device and Expo

Use agent-device to let your AI coding agent verify, debug, profile, and test a running Expo app across local and remote devices.

[`agent-device`](https://agent-device.dev) is an open-source, agent-native CLI from Callstack. It lets AI coding agents operate your running Expo app and verify the result with UI state, screenshots, video, logs, network activity, traces, and performance data.

Code inspection and automated tests do not always show what happens in the running app. With `agent-device`, an agent can check that a screen renders, a flow completes, or a request fires, then report what it observed. This adds behavior review to the same workflow the agent uses to implement the change.

`agent-device` runs from the terminal in Codex, Claude Code, Cursor, and other coding agents, with optional [Model Context Protocol (MCP)](/mcp.md) integration. One command model covers Android Emulators, iOS Simulators, physical devices, Android TV, tvOS, macOS, Linux, and web.

Routine responses stay compact. Semantic snapshots include actionable references such as `@e3`, command results explain how to recover when something fails, and larger outputs are saved as artifacts. This keeps more of the agent's context available for working on your app.

> `agent-device` is built and maintained by [Callstack](https://www.callstack.com) and is available under the MIT license. For the complete list of supported platforms and commands, see the [`agent-device` documentation](https://oss.callstack.com/agent-device/).

#### Prerequisites

##### Node.js 22.12 or later

The `agent-device` CLI requires Node.js 22.12 or newer.

##### A device environment for local testing (optional)

To test locally on Android, install the [Android SDK Platform Tools (`adb`)](/workflow/android-studio-emulator.md#set-up-android-studio) and make sure `adb` is on your `PATH`. To test locally on iOS, use macOS with [Xcode installed](/workflow/ios-simulator.md#setup-xcode-and-watchman). You can skip these local toolchains when using a [device cloud](https://oss.callstack.com/agent-device/docs/device-clouds) or a [remote device host](https://oss.callstack.com/agent-device/docs/remote-proxy).

## Quick start

### Install `agent-device`

Install the CLI globally so your coding agent can use a stable `agent-device` command from its terminal.

```sh
# npm
npm install -g agent-device@latest

# yarn
yarn global add agent-device@latest

# pnpm
pnpm add -g agent-device@latest

# bun
bun add -g agent-device@latest
```

Check that the local toolchain and devices are ready, then read the version-matched workflow guide:

```sh
agent-device doctor
agent-device --version
agent-device help workflow
```

### Install the `agent-device` skill (optional)

If your coding agent supports skills, install the official skill. It teaches the agent to select the right workflow and read help that matches your installed CLI version.

```sh
# npm
npx skills add callstack/agent-device

# yarn
yarn dlx skills add callstack/agent-device

# pnpm
pnpm dlx skills add callstack/agent-device

# bun
bunx skills add callstack/agent-device
```

You can also use `agent-device` without a skill. Tell your agent to use `agent-device`; it can learn the current workflow from the CLI's built-in help. See [AI Agent Setup](https://oss.callstack.com/agent-device/docs/agent-setup) for client-specific instructions and optional MCP configuration.

### Run your Expo app

Start the app on an Android Emulator or iOS Simulator. `agent-device` operates the installed app, so you do not need to add an `agent-device` library to your Expo project.

```sh
npx expo run:android
npx expo run:ios
npx expo start
```

### Prompt your agent

Ask your coding agent to use `agent-device` against the running app. It can discover the app, open a session, inspect the current screen, act on elements, and verify the result.

```text
Use `agent-device` to open the settings screen in my Expo app on iOS, verify it loaded correctly,
and capture a screenshot as evidence.
```

If the agent confirms the settings screen loaded and returns the screenshot path, it can observe and control your app.

## How the `agent-device` loop works

The agent opens the app, reads a compact accessibility snapshot, and acts on refs such as `@e2`. Settled interactions can include the resulting UI changes in the same response. The agent then verifies the result with an assertion or the evidence that suits the task, such as a screenshot, focused log window, or performance sample.

```bash
agent-device apps --platform ios
agent-device open MyApp --platform ios
agent-device snapshot -i
# @e1 [heading] "Welcome"
# @e2 [button] "Get Started"
agent-device press @e2 --settle
agent-device screenshot ./artifacts/get-started.png
agent-device close
```

Your agent chooses and runs these commands by following the workflow guidance bundled with the installed CLI.

## What `agent-device` lets your agent do

-   **Control and interact:** Inspect accessibility labels, roles, values, test IDs, and interactive refs. Launch apps, tap, type, scroll, perform gestures, handle alerts, open deep links, and change device state. Use the same commands locally or through [device clouds](https://oss.callstack.com/agent-device/docs/device-clouds) and a [remote proxy](https://oss.callstack.com/agent-device/docs/remote-proxy) across Android, iOS, web, TV, and desktop targets.
-   **Profile and debug:** Inspect React Native components, props, hooks, slow commits, and re-renders; evaluate targeted JavaScript through Metro's Chrome DevTools Protocol (CDP); and collect focused logs and available network requests and responses. Add platform-supported native CPU, memory, FPS and frame-health, trace, crash, screenshot, video, and audio evidence in the same session.
-   **Test and repeat:** Record a working session as a deterministic `.ad` script, replay it or run it with the built-in test command in CI, and keep artifacts for failed runs. Run supported Maestro YAML with `agent-device test --maestro`, or export compatible `.ad` flows to Maestro YAML.

This covers routine implementation checks, exploratory QA, bug reproduction, and performance work. When an investigation needs live native breakpoints, variables, memory inspection, or stepping, `agent-device` can reproduce and document the flow while Xcode or LLDB attaches to the app process. For version-matched diagnostic and profiling guidance, start with `agent-device help debugging`, `agent-device help react-devtools`, and `agent-device help cdp`, or see [Debugging and Profiling](https://oss.callstack.com/agent-device/docs/debugging-profiling).

## Example prompts

After setup, describe the outcome and evidence you want. For example:

| Task | Example prompt |
| --- | --- |
| Verify an implementation | Open the app and test the new checkout flow. Capture the confirmation screen and report anything that blocks you. |
| Reproduce a bug on real device | Reproduce issue #123 on a real Android device using my BrowserStack subscription. |
| Check accessibility | Inspect sign-up and report interactive elements with missing or ambiguous accessibility labels. Provide relevant screenshots. |
| Profile a slow screen | Profile the products list while scrolling. Design repeatable experiments with replay scripts and find React or native performance offenders. |
| Design-to-code loop | Implement this Figma design, verify it on iOS using screenshot diffing, and iterate until the difference is below 2%. |
| Dogfood the app | Explore the app as a first-time user and return a prioritized report with a screenshot for each finding. |

## Use `agent-device` with Expo AI tooling

`agent-device` complements Expo's own agent tooling. Expo Skills teach your agent how to implement the feature, the Expo MCP Server gives it current Expo and EAS context, and `agent-device` lets it verify the result in the running app.

[Expo MCP Server](/mcp.md#installation-and-setup) — Connect the remote Expo MCP Server to give agents live access to Expo documentation and EAS.

[Expo Skills](/skills.md#install-expo-skills) — Install the plugin that teaches agents known-good Expo patterns.

For automated pull request testing, start with Callstack's [`agent-device` EAS Workflow template](https://github.com/callstackincubator/eas-agent-device/blob/main/.eas/workflows/agent-qa-mobile.yml). It shows how to run an AI QA agent against an Expo app and retain reviewable artifacts.

## Optional MCP setup

Most coding agents can use `agent-device` directly through their integrated terminal. If your client supports MCP and you prefer structured tools, configure it to launch the installed CLI:

```json
{
  "mcpServers": {
    "agent-device": {
      "command": "agent-device",
      "args": ["mcp"]
    }
  }
}
```

MCP provides structured tools for the same device workflows. Keep the CLI available so the agent can read version-matched help and use terminal-only setup commands.

## Limitations and tips

-   Accessibility labels, roles, and test IDs make agent interactions substantially more reliable. Use screenshots as evidence or as a visual fallback, but prefer refs and selectors for actions.
-   Keep state-changing commands in one session sequential. Close the session when the task finishes; use `agent-device close --shutdown` in CI when the emulator or simulator should also stop.
-   Device-level UI automation, including snapshots, taps, typing, screenshots, and logs, works on the installed app in a development build or in Expo Go, with no library added to your project.
-   React Native component inspection and React profiling require a development server and a compatible React DevTools connection, so keep it running. In Expo Go, native CPU, memory, and trace profiling targets the Expo Go host process rather than an app-specific native binary, so use a development build to profile your native code.
-   Logging is off by default. Ask the agent to open a focused log window for a reproduction instead of collecting an unbounded device log.
-   Physical device automation requires platform-specific pairing, signing, permissions, and trust setup. Start with a simulator or emulator, then follow the [installation guide](https://oss.callstack.com/agent-device/docs/installation) for physical devices.
-   Run `agent-device help react-native`, `agent-device help debugging`, or `agent-device help dogfood` when a task needs deeper, version-matched guidance.
