---
modificationDate: June 30, 2026
title: Claude Code and Expo
description: Use Claude Code to build, upgrade, debug, and deploy your Expo and React Native projects.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/agents/claude/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/agents/claude/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > AI > AI agents
Pages in this section:
- [Claude Code](https://docs.expo.dev/agents/claude.md) (this page)
- [Codex](https://docs.expo.dev/agents/codex.md)
- [Cursor](https://docs.expo.dev/agents/cursor.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Claude Code and Expo

Use Claude Code to build, upgrade, debug, and deploy your Expo and React Native projects.

Claude Code is Anthropic's terminal-based AI coding agent. It can understand your entire codebase, propose edits, run terminal commands, and manage git operations. Expo projects created with `create-expo-app` are scaffolded with files for Claude Code through **CLAUDE.md** and **.claude/settings.json**. It can also check EAS and Expo CLI logs, fetch documentation from the Expo Model Context Protocol (MCP) Server, use Expo Skills for best practices, manage your EAS deployment workflow, and more.

## Quick start

### Install Claude Code

Install Claude Code globally, then start it from any project. For other install methods, see the [Claude Code documentation](https://code.claude.com/docs).

```sh
curl -fsSL https://claude.ai/install.sh | bash
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

Install Expo Skills and connect the Expo MCP Server so Claude Code knows Expo conventions and can reach your EAS.

[Expo Skills](/skills.md#install-expo-skills) — Install the plugin that teaches agents known-good Expo patterns, and browse the full list of available skills.

[Expo MCP Server](/mcp.md#installation-and-setup) — Connect the remote Expo MCP Server to give agents live access to Expo documentation and EAS.

### Open your project and start prompting

Run Claude Code from your project root, then describe what you want to do.

```sh
cd my-app
claude
```

### Verify setup

Paste the following prompt in your Claude Code session to confirm it can read your project:

```text
Open package.json and tell me which Expo SDK version this project targets.
```

If the agent replies with the SDK version from **package.json**, the agent is reading your project correctly.

## How Claude Code reads your Expo project

When you start Claude Code in your project, it reads two of the scaffolded files:

-   **CLAUDE.md** contains a single line: `@AGENTS.md`. This line imports **AGENTS.md** into Claude Code's context. **AGENTS.md** points Claude Code to the documentation for your project's Expo SDK version. Add project-level instructions to **AGENTS.md**, not **CLAUDE.md**, so they stay in one place.
-   **.claude/settings.json** enables the official Expo plugin ([`expo@claude-plugins-official`](https://claude.com/plugins/expo)) from the Claude Code plugin marketplace.

Both files are committed to your project, so a developer's Claude Code session starts from the same context.

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

## Troubleshooting

#### Plugin 'expo' is enabled in project settings but isn't installed here.

If you see this error in your Claude Code session, then you need to install the official [Expo plugin](https://claude.com/plugins/expo) on your developer machine. To install it, run the following command in the terminal window and then restart your Claude Code session:

```sh
claude plugin install expo@claude-plugins-official
```

The above command installs the Expo plugin globally. If you want to install it just for this project, add `--scope project` to the command:

```sh
claude plugin install expo@claude-plugins-official --scope project
```
