---
modificationDate: July 17, 2026
title: Expo Skills for AI agents
description: A list of official AI agent skills provided by Expo for building, deploying, and debugging Expo and React Native apps.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/skills/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/skills/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > AI
Pages in this section:
- [Overview](https://docs.expo.dev/agents.md)
- [Expo Skills](https://docs.expo.dev/skills.md) (this page)
- [MCP Server](https://docs.expo.dev/mcp.md)
- [LLMs](https://docs.expo.dev/llms.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Skills for AI agents

A list of official AI agent skills provided by Expo for building, deploying, and debugging Expo and React Native apps.

Expo Skills are structured instruction files that teach AI agents how to build, deploy, and debug Expo and React Native apps accurately and efficiently. They work with Claude Code, Cursor, Codex, and other AI agents.

[The 3 tools you need to build mobile apps with AI](https://www.youtube.com/watch?v=WLGAuwagI8o&t=61) — Learn how Expo Skills teach an AI agent to build a habit tracker app from scratch.

## Install Expo Skills

#### Claude Code

Run the following command to install the official Expo plugin from the `claude-plugins-official` marketplace:

```sh
/plugin install expo@claude-plugins-official
```

#### Codex

From the command line, run the following command to install the official Expo plugin:

```sh
codex plugin add expo@openai-curated
```

You can also open `/plugins` in Codex and install `expo` from the `openai-curated` marketplace.

#### Cursor

If you have already installed Expo Skills for Claude Code, Codex, or another agent, recent versions of Cursor import them automatically. Open **Settings** > **Rules, Skills, Subagents**, make sure **Include third-party Plugins, Skills, and other configs** is enabled (it is on by default), and the Expo Skills appear in the **Skills** list.

If you have not installed Expo Skills yet, run one of the following with the [skills CLI](https://skills.sh/docs/cli):

```sh
# npm
npx skills add expo/skills

# yarn
yarn dlx skills add expo/skills

# pnpm
pnpm dlx skills add expo/skills

# bun
bunx skills add expo/skills
```

Then reopen Cursor and verify the skills appear under **Settings** > **Rules, Skills, Subagents** > **Skills**.

> Skills in Cursor are not shown in the slash command (`/`) menu. They work via auto-discovery when you ask the agent Expo-related questions.

#### Other agents

Use the [skills CLI](https://skills.sh/docs/cli) to add Expo Skills to any compatible agent:

```sh
# npm
npx skills add expo/skills

# yarn
yarn dlx skills add expo/skills

# pnpm
pnpm dlx skills add expo/skills

# bun
bunx skills add expo/skills
```

## Available Expo Skills

The following skills are available in the `expo` plugin. Skills name prefixed with`expo-*` skills are used for the Expo open source framework and `eas-*` skills for the EAS services.

### Expo SDK and framework

| Skill | Description |
| --- | --- |
| [`expo-app-clip`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-app-clip/SKILL.md) | Add an iOS App Clip target to an Expo app. Use when the user mentions App Clip, AASA, apple-app-site-association, appclips, smart app banner, or wants to ship a lightweight iOS Clip invoked from a URL alongside their parent app. |
| [`expo-brownfield`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-brownfield/SKILL.md) | Integrate Expo and React Native into an existing native iOS or Android app. Use when the user mentions brownfield, embedding React Native in a native app, AAR/XCFramework, or adding Expo to an existing Kotlin/Swift project. Covers both the isolated approach and the integrated approach. |
| [`expo-data-fetching`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-data-fetching/SKILL.md) | Use when implementing or debugging ANY network request, API call, or data fetching. Covers fetch API, React Query, SWR, error handling, caching, offline support, and Expo Router data loaders (`useLoaderData`). |
| [`expo-dev-client`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-dev-client/SKILL.md) | Build and distribute Expo development clients locally or via TestFlight for internal testing. For production TestFlight releases and store submission, use the eas-app-stores skill. |
| [`expo-dom`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-dom/SKILL.md) | Use Expo DOM components to run web code in a webview on native and as-is on web. Migrate web code to native incrementally. For the end-to-end migration of a whole web app, use the expo-web-to-native skill. |
| [`expo-examples`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-examples/SKILL.md) | Expo's official example projects - the expo/examples repo of ~70 `with-*` integrations (Stripe, Clerk, Supabase, OpenAI, maps, Reanimated, SQLite, Skia, NativeWind, and more). Use when integrating a third-party library or service into an existing Expo app and you want the canonical, version-matched pattern to adapt, or when scaffolding a new project from one with `npx create-expo --example`. |
| [`expo-module`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-module/SKILL.md) | Guide for creating and writing Expo native modules and views using the Expo Modules API (Swift, Kotlin, TypeScript). Covers module definition DSL, native views, shared objects, config plugins, lifecycle hooks, autolinking, and type system. Use when building or modifying native modules for Expo. Not for migrating an existing Swift module from the definition DSL to the Expo Modules API 2.0 macros; use expo-migrate-module (from the expo-experiments plugin) for that. |
| [`expo-native-ui`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-native-ui/SKILL.md) | Build beautiful, native-feeling Expo screens. Covers Apple HIG styling, semantic colors, native controls, SF Symbols, media, animations, visual effects, gradients, storage, and responsive layout. For routing and navigation, use the expo-router skill. |
| [`expo-project-structure`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-project-structure/SKILL.md) | Folder structure for a new Expo app. Use when scaffolding or laying out a new Expo project with Expo Router, or deciding where a file should live in one. For new projects only — never restructure an existing app to match. |
| [`expo-router`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-router/SKILL.md) | Navigation and routing for Expo Router. Covers file-based routes, groups and dynamic routes, folder organization, Link with previews and context menus, native Stack, page titles, modals and form sheets, NativeTabs, headers and toolbars, and header search bars. |
| [`expo-skill-feedback`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-skill-feedback/SKILL.md) | Submit feedback on an Expo skill—or Expo itself—and control bundled anonymous usage telemetry (off by default / opt-in). Submit feedback with: npx --yes submit-expo-feedback@latest "ACTIONABLE_FEEDBACK". Optionally add either or both: --category "CATEGORY" and --subject "SUBJECT". Replace the uppercase placeholders before running. Use when a skill was useful, confusing, broken, missing context, or worth improving; when Expo, Expo CLI, EAS CLI, docs, or MCP worked well or fell short; when an AI agent repeatedly failed, got stuck, or needed the user to take over an Expo task (report it as an eval candidate); or when the user explicitly asks to enable or disable telemetry (tracking), check its status, or understand what it collects. |
| [`expo-tailwind-setup`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-tailwind-setup/SKILL.md) | Set up Tailwind CSS v4 in Expo with react-native-css and NativeWind v5 for universal styling. |
| [`expo-ui`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-ui/SKILL.md) | Build native UI with the @expo/ui package: real SwiftUI on iOS and Jetpack Compose on Android rendered from React in an Expo or React Native app. Covers universal cross-platform components (Host, Column, Row, Button, Text, List, and more imported from @expo/ui), drop-in replacements for popular React Native community libraries (BottomSheet, DateTimePicker, Slider, Menu, etc.), and platform-specific SwiftUI (@expo/ui/swift-ui, iOS only) and Jetpack Compose (@expo/ui/jetpack-compose, Android only) trees and modifiers. Use when adding or reviewing @expo/ui Host/RNHostView trees, building native-feeling UI where standard React Native components fall short (grouped settings forms with toggles, sections, menus, sheets, pickers, sliders), choosing between universal and platform-specific components, or replacing an RN community UI library with a native @expo/ui equivalent. Not for custom native modules, Expo Router navigation, Reanimated, or data fetching. |
| [`expo-upgrade`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-upgrade/SKILL.md) | Guidelines for upgrading Expo SDK versions and fixing dependency issues. |
| [`expo-web-to-native`](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-web-to-native/SKILL.md) | Migrate an existing web React app to a native iOS/Android app with Expo. Use when the user wants to turn a website into a mobile app, port a Next.js/Vite/CRA React codebase to React Native, reuse web code on native incrementally, or asks how web idioms (the DOM, CSS, React Router, localStorage, window) map to native. This is the end-to-end migration guide; use the `expo-dom` skill for the DOM-component mechanism itself. |

### Expo Application Services (EAS)

| Skill | Description |
| --- | --- |
| [`eas-app-stores`](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-app-stores/SKILL.md) | Deploy Expo apps to the app stores with EAS - build and submit to the iOS App Store, Google Play Store, and TestFlight, configure eas.json build and submit profiles, manage app versions and build numbers, and publish App Store metadata and ASO. Use whenever the user wants to deploy, release, or ship an app to production or the app stores, is preparing a production build, running eas build or eas submit, shipping to TestFlight, bumping version or build numbers, or setting up store listing metadata. For deploying an Expo website or API routes, use the eas-hosting skill. |
| [`eas-hosting`](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-hosting/SKILL.md) | Deploy Expo websites and Expo Router API routes to EAS Hosting - export the web bundle, run eas deploy for production and PR preview URLs, manage environment secrets and custom domains, and work within the Cloudflare Workers runtime. Also covers authoring API routes (+api.ts handlers, HTTP methods, request handling, CORS). Use when deploying an Expo web app or API routes, setting up EAS Hosting, or configuring hosting environments and domains. Not for native builds or store releases - use the eas-app-stores skill for those. |
| [`eas-observe`](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-observe/SKILL.md) | Use for anything related to EAS Observe - adding `expo-observe` to an Expo project (AppMetricsRoot/ObserveRoot HOC, markInteractive, the useObserve hook, the Expo Router / React Navigation integrations for per-route metrics, and user-defined events via `Observe.logEvent`), querying via the EAS CLI (`eas observe:metrics-summary`, `observe:metrics`, `observe:routes`, `observe:events`, `observe:versions`), or interpreting the resulting metrics (cold/warm launch, TTR, TTI, navigation cold/warm TTR, update download, and the TTI frameRate params for triaging slow startups). |
| [`eas-simulator`](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-simulator/SKILL.md) | Run and control a user's app on a remote iOS/Android simulator hosted on EAS cloud. Read before running any `eas simulator:*` commands - it has the current syntax for this experimental API. Use whenever the user needs a simulator they can't run locally - 'run my app on a cloud simulator', 'use eas simulator to run/install/screenshot my app', 'I'm on Linux/Cursor and need an iOS device', 'no sim on this box / headless CI', 'let an agent click through my app and screenshot it', 'test my dev build on a remote sim with live reload', 'stream a sim to my browser' - even when they don't say 'EAS Simulator' or 'cloud'. On a host WITHOUT a local simulator (Linux, CI, cloud sandbox) it's the default; on macOS, do NOT auto-trigger for a plain 'run on the simulator' - use it only for a cloud/remote/shareable sim, an iOS version they lack, or an agent-driven session. NOT for local sims (expo run:ios, Xcode, Android Studio), EAS Build/Update, web preview, or physical devices. |
| [`eas-update-insights`](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-update-insights/SKILL.md) | Check the health of published EAS Update: crash rates, install/launch counts, unique users, payload size, and the split between embedded and OTA users per channel. Use when the user asks how an update is performing, whether a rollout is healthy, how many users are on the embedded build vs OTA, or wants to gate CI on update health. |
| [`eas-workflows`](https://github.com/expo/skills/blob/main/plugins/expo/skills/eas-workflows/SKILL.md) | Helps understand and write EAS workflow YAML files for Expo projects. Use this skill when the user asks about CI/CD or workflows in an Expo or EAS context, mentions .eas/workflows/, or wants help with EAS build pipelines or deployment automation. |

## Example prompts

Try the following prompts after installing Expo Skills. Your AI agent will automatically use the appropriate skill:

| Example prompt | Skill used |
| --- | --- |
| Build a settings screen with native-feeling controls | `expo-native-ui` |
| Add tab navigation and a modal to my app | `expo-router` |
| Set up Tailwind CSS in my Expo project | `expo-tailwind-setup` |
| Embed a recharts chart in my native app using web code | `expo-dom` |
| Add a SwiftUI picker component to my Expo app | `expo-ui` |
| Use Material Design 3 components with Jetpack Compose | `expo-ui` |
| How do I deploy my Expo app to the Apple App Store? | `eas-app-stores` |
| Create a CI/CD workflow that builds on every PR | `eas-workflows` |
| Upgrade my project to the latest Expo SDK | `expo-upgrade` |

## Additional resources

[expo/skills GitHub repository](https://github.com/expo/skills) — expo/skills — Browse the source for all available Expo Skills, or report issues.

[Expo MCP Server](/mcp.md) — Companion AI tooling that gives coding agents direct access to Expo and EAS services.
