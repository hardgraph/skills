---
modificationDate: June 30, 2026
title: Codex and Expo
description: Use Codex to build, upgrade, debug, and deploy your Expo and React Native projects.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/agents/codex/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/agents/codex/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > AI > AI agents
Pages in this section:
- [Claude Code](https://docs.expo.dev/agents/claude.md)
- [Codex](https://docs.expo.dev/agents/codex.md) (this page)
- [Cursor](https://docs.expo.dev/agents/cursor.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Codex and Expo

Use Codex to build, upgrade, debug, and deploy your Expo and React Native projects.

Codex is OpenAI's terminal-based AI coding agent. It can read and write files across your project, run terminal commands, and browse the web. Expo projects created with `create-expo-app` are scaffolded with an **AGENTS.md** file that Codex reads directly. It can also check EAS and Expo CLI logs, fetch documentation from the Expo Model Context Protocol (MCP) Server, use Expo Skills for best practices, manage your EAS deployment workflow, and more.

## Quick start

### Install Codex

Install Codex, then start it from any project. For other install methods, see the [Codex CLI documentation](https://developers.openai.com/codex/cli).

```sh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

### Create a new Expo project

Create a project with the following command or ensure your existing project has the latest expo package installed.

```sh
# npm
npx create-expo-app@latest --template default@sdk-57

# yarn
yarn create expo-app --template default@sdk-57

# pnpm
pnpm create expo-app --template default@sdk-57

# bun
bun create expo --template default@sdk-57
```

### Set up Expo Skills and the Expo MCP Server

Install Expo Skills and connect the Expo MCP Server so Codex knows Expo conventions and can reach your EAS.

[Expo Skills](/skills.md#install-expo-skills) — Install the plugin that teaches agents known-good Expo patterns, and browse the full list of available skills.

[Expo MCP Server](/mcp.md#installation-and-setup) — Connect the remote Expo MCP Server to give agents live access to Expo documentation and EAS.

### Open your project and start prompting

Run Codex from your project root, then describe what you want to do.

```sh
cd my-app
codex
```

### Verify setup

Paste the following prompt in your Codex session to confirm it can read your project:

```text
Open package.json and tell me which Expo SDK version this project targets.
```

If the agent replies with the SDK version from **package.json**, the agent is reading your project correctly.

## How Codex reads your Expo project

When you start Codex in your project, it reads the scaffolded **AGENTS.md** file. This file points Codex to the documentation for your project's Expo SDK version and holds any project-level instructions you add.

**AGENTS.md** is committed to your project, so a developer's Codex session starts from the same context.

## Example prompts

After setup, describe Expo tasks in plain language. For example:

| Task | Example prompt |
| --- | --- |
| Upgrade the SDK | Upgrade this project to the latest Expo SDK and fix any breaking changes. |
| Add navigation | Add a tab navigator with Expo Router and a new settings screen. |
| Automate builds | Create an EAS Workflow that builds the app on every pull request. |
| Debug a build | My latest iOS build failed. Read the EAS Build logs and tell me what went wrong. |
| Add notifications | Configure expo-notifications and show a local notification when the app launches. |
| Set up CI/CD | Create a CI/CD workflow that builds on every PR. |
| Add native UI | Add a SwiftUI picker component to my Expo app. |
| Check feedback | Show TestFlight feedback for my app. |
| Verify the UI | Take a screenshot and verify the blue circle view. |

For more examples, see the Example prompts sections of [Expo Skills](/skills.md#example-prompts) and [Expo MCP Server](/mcp.md#what-does-expo-mcp-server-do).
