---
modificationDate: July 30, 2026
title: Using Model Context Protocol (MCP) with Expo
description: A guide on integrating Model Context Protocol with Expo projects to enhance AI model capabilities.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/mcp/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/mcp/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > AI
Pages in this section:
- [Overview](https://docs.expo.dev/agents.md)
- [Expo Skills](https://docs.expo.dev/skills.md)
- [MCP Server](https://docs.expo.dev/mcp.md) (this page)
- [LLMs](https://docs.expo.dev/llms.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Model Context Protocol (MCP) with Expo

A guide on integrating Model Context Protocol with Expo projects to enhance AI model capabilities.

Model Context Protocol (MCP) is a standard protocol that allows AI models to integrate with external data sources, providing enhanced context for more precise responses. It enables AI-assisted tools like agents to understand your development environment more deeply, allowing them to provide better assistance with your codebase.

Expo MCP Server is a remote MCP server hosted by Expo that integrates with popular AI-assisted tools such as Claude Code, Cursor, VS Code, and others, enabling them to interact directly with your Expo projects.

[Introducing Expo MCP Server: for accurate, context-aware AI responses](https://www.youtube.com/watch?v=dp9dpIgDxZQ) — Enhance your AI-assisted tools for building apps with Expo.

[The 3 tools you need to build mobile apps with AI](https://www.youtube.com/watch?v=WLGAuwagI8o&t=364) — Watch an AI agent use MCP servers while building a mobile app with Expo.

## What does Expo MCP Server do?

Expo MCP Server teaches your AI-assisted tools about the Expo SDK and lets them interact with mobile simulators and the React Native DevTools. These are some examples of the tasks Expo MCP Server enhances:

**Learn about developing with Expo.** Your AI-assisted tools can fetch the latest official Expo documentation on demand and use it to reply to prompts like:

-   "How do I use Expo Router?"
-   "Search the Expo docs for implementing deep linking"
-   "Read the Expo Router docs page"
-   "What is Expo CNG?"

**Manage dependencies.** Expo MCP Server guides you toward installing our recommended packages and uses `npx expo install` to install known, compatible versions.

-   "Add SQLite with basic CRUD operations"
-   "Install `expo-camera` and show me how to take photos"
-   "Add `expo-notifications` for push notifications"

**Manage builds and workflows.** Expo MCP Server can trigger and monitor EAS builds, run workflows, and pull crash data from TestFlight:

-   "Investigate why my most recent iOS build on EAS failed"
-   "Identify any patterns in recent failing workflows"
-   "Create a workflow that runs Maestro tests"
-   "Show me recent TestFlight crashes"
-   "Show TestFlight feedback for my app"

**Automate visual verification and testing.** Multimodal AI-assisted tools can screenshot and interact with your running app in a simulator. Expo MCP Server includes local capabilities enabled by adding the `expo-mcp` package to your project's dependencies.

-   "Add a blue circle view and make sure it renders correctly"
-   "Add a button and tap it to verify the interaction works"
-   "Add a counter button that increments on tap and verify the state updates correctly"

Your AI-assisted tools can autonomously write the code, capture screenshots to verify the UI is correct, test interactions, and fix issues they find.

The complete table of [MCP capabilities](/mcp.md#available-mcp-capabilities) documents the tools and prompts Expo MCP Server provides to AI-assisted tools.

#### Prerequisites

##### Expo account

An Expo account is required to use Expo MCP Server.

##### An Expo project on the latest SDK

Create a project with `npx create-expo-app@latest --template default@sdk-57`, or ensure your existing project has the latest `expo` package installed.

##### An AI-assisted tool with remote MCP support

Claude Code, Cursor, VS Code, or any other tool with remote MCP server support.

## Installation and setup

### Install Expo MCP Server

Expo MCP Server supports integration with various AI-assisted tools. Use the general settings below or expand your specific tool for detailed instructions:

-   **Server type**: Streamable HTTP
-   **URL**: `https://mcp.expo.dev/mcp`
-   **Authentication**: OAuth

#### Claude Code setup

```sh
claude mcp add --transport http expo https://mcp.expo.dev/mcp
```

After installation, run `/mcp` in your Claude Code session to authenticate.

#### Cursor setup

Click the following link to install the MCP server for Cursor:

#### VS Code setup

1.  Open Command Palette (Cmd ⌘ + Shift + P or Ctrl + Shift + P)
2.  Run **MCP: Add Server**
3.  Select **HTTP**
4.  Enter the server details:
    -   **URL**: `https://mcp.expo.dev/mcp`
    -   **Name**: expo

#### Codex setup

```sh
codex mcp add expo --url https://mcp.expo.dev/mcp
```

The above command adds the MCP server to your Codex configuration file and prompts you to authenticate with your Expo account.

### Authenticate with Expo

After installing the MCP server, you'll need to authenticate.

Sign in with your Expo account in the browser when prompted. The server will generate an access token automatically.

### Set up local capabilities (Recommended)

> Local capabilities are only available in **SDK 54 and later**.

For the full MCP experience with advanced features like taking screenshots from your iOS Simulator, opening DevTools, and automation capabilities, set up a local Expo development server:

```sh
# npm
cd /path/to/your-project
npx expo install expo-mcp --dev
npx expo whoami || npx expo login
EXPO_UNSTABLE_MCP_SERVER=1 npx expo start

# yarn
cd /path/to/your-project
yarn expo install expo-mcp --dev
yarn expo whoami || yarn expo login
EXPO_UNSTABLE_MCP_SERVER=1 yarn expo start

# pnpm
cd /path/to/your-project
pnpm expo install expo-mcp --dev
pnpm expo whoami || pnpm expo login
EXPO_UNSTABLE_MCP_SERVER=1 pnpm expo start

# bun
cd /path/to/your-project
bun expo install expo-mcp --dev
bun expo whoami || bun expo login
EXPO_UNSTABLE_MCP_SERVER=1 bun expo start
```

> Whenever you start or stop the development server, you need to **reconnect or restart** your MCP server connection in your AI-assisted tool to ensure the AI-assisted tool gets refreshed capabilities.

## Server capabilities versus local capabilities

Expo MCP Server provides two types of capabilities depending on your setup:

### Server capabilities

Server capabilities are available with just the remote MCP server connection, without needing to set up a local development server.

### Local capabilities

Local capabilities require a local Expo development server to be running and provide advanced features that interact with your local development environment:

-   **Automation tools**: Take screenshots, tap views, find elements by testID
-   **Development tools**: Open React Native DevTools
-   **Project analysis**: Generate `expo-router` sitemap

These capabilities enable more sophisticated workflows like automated testing, visual verification, and deeper project introspection. To use local capabilities, you will need to follow the [Set up local capabilities](/mcp.md#set-up-local-capabilities-recommended) section above.

## Available MCP capabilities

> The MCP capabilities are subject to change from the `expo-mcp` package updates or MCP server changes. The following list is a reference and may not be up to date.

### Tools

| Tool | Description | Example prompt | Availability |
| --- | --- | --- | --- |
| `add_library` | Add an Expo library to the project using expo install and attach usage instructions when available. | "add sqlite and basic CRUD to the app" | Server |
| `read_documentation` | Fetch a single Expo documentation page and return its content as markdown. Returns up to ~5000 tokens per call. Use offset to paginate through long pages. | "read the Expo Router docs page" | Server |
| `search_documentation` | Search the official Expo documentation and return page URLs ranked by relevance for a user query. Use read_documentation to fetch the full content of specific pages, starting from the top. | "search documentation for CNG" | Server (requires an EAS paid plan) |
| `learn` | Learn Expo how-to for a specific topic and remember it for future conversations. Use this to teach the assistant about specific Expo features or workflows. | "learn how to use expo-router" | Server |
| `workflow_create` | Creates a new EAS workflow YAML file for Expo projects or fetches workflow syntax documentation. Use this when users want to create CI/CD workflows in .eas/workflows/ or need to learn EAS workflow syntax. After creating, use workflow_validate to validate the file. | "create a CI/CD workflow for building and deploying" | Server |
| `workflow_info` | Fetches detailed information about a specific EAS workflow run by ID. Use this to check the status, job results, errors, and artifacts of a workflow run. If workflow has multiple jobs, draw them in a diagram to show the dependencies between jobs. | "get the status of the latest workflow run" | Server |
| `workflow_list` | Lists recent EAS workflow runs for a project. Provide either appId (from app.json "extra.eas.projectId") OR appFullName (e.g., "@owner/my-app"). | "list the recent workflow runs" | Server |
| `workflow_logs` | Fetches logs for a specific job in an EAS workflow run. Call without sectionIndex or phase to get a summary of log sections (phase names and line ranges); then call again with sectionIndex or phase to fetch that section. | "show me the logs for the build job in the workflow" | Server |
| `workflow_run` | Triggers an EAS workflow run from a git reference. Provide either appId (from app.json "extra.eas.projectId") OR appFullName (e.g., "@owner/my-app"). The workflow file must exist at the specified git reference. | "run the build-and-deploy workflow" | Server |
| `workflow_cancel` | Cancels a running EAS workflow. Use workflow_info to get the workflow run ID. | "cancel the running workflow" | Server |
| `workflow_validate` | Validates EAS workflow YAML syntax and configuration. Use this after creating a workflow to ensure it is valid. Provide either appId (from app.json "extra.eas.projectId") OR appFullName (e.g., "@owner/my-app"). | "validate my workflow file" | Server |
| `build_list` | Lists EAS builds for a project. Provide either appId (from app.json "extra.eas.projectId") OR appFullName (e.g., "@owner/my-app"). Use this to see recent builds, their status, and available artifacts. | "list the recent builds for this project" | Server |
| `build_info` | Fetches the status and detailed information about a specific EAS build by ID. Use this to check build status, errors, artifacts, and other details. | "get the status of my latest iOS build" | Server |
| `build_logs` | Fetches the logs for a specific EAS build. The build must be completed (finished or errored) to have logs available. | "show me the logs for the failed build" | Server |
| `build_submit` | Submits an EAS build to the app store (Google Play Store for Android, App Store for iOS). The build must be a finished build with the appropriate distribution type. Provide either appId (from app.json "extra.eas.projectId") OR appFullName (e.g., "@owner/my-app"). | "submit the latest build to the App Store" | Server |
| `build_run` | Triggers a new EAS build using a build profile from eas.json. Requires a GitHub repository to be connected to the project. Provide either appId (from app.json "extra.eas.projectId") OR appFullName (e.g., "@owner/my-app"). | "run a production build for iOS" | Server |
| `build_cancel` | Cancels an EAS build that is queued or in progress. Use build_info to check the current status first. | "cancel the build that is currently in progress" | Server |
| `testflight_crashes` | Fetch TestFlight crash data. Without crashId, lists recent crashes. With crashId, returns the full crash log with stack trace. | "show me recent TestFlight crashes" | Server |
| `testflight_feedback` | Fetch screenshot feedback from TestFlight. Returns feedback metadata including device info, user comments, and screenshot URLs. | "show TestFlight feedback for my app" | Server |
| `appstore_reviews` | Fetch public App Store customer reviews for an app (rating, title, body, reviewer, territory). For TestFlight beta feedback use testflight_feedback instead. | "" | Server |
| `appstore_reply_review` | Post or edit the public developer response to an App Store customer review. This is a write action: the response is visible to everyone on the App Store, and Apple publishes it after a short review. App Store Connect allows a single response per review, so any existing response is replaced. | "" | Server |
| `appstore_delete_review_response` | Delete the public developer response on an App Store customer review. This is a write action: it removes the response from the App Store. No-op-safe — if the review has no response, it reports that nothing was deleted. | "" | Server |
| `playstore_crashes` | Fetch crash and ANR data from Google Play (Android Vitals). Without issueId, lists recent crash/ANR issues. With issueId, returns the full error report with stack trace. | "" | Server |
| `playstore_reviews` | Fetch user reviews from Google Play. Returns review metadata including author, star rating, device info, and comment text. Note: Google Play only exposes production reviews with text from approximately the last week. | "" | Server |
| `playstore_reply_review` | Post a public developer reply to a Google Play user review, or edit the existing reply. This is a write action: the reply is visible to users on the store listing. Each review has a single developer reply, so replying again replaces it. Reply text is limited to 350 characters. | "" | Server |
| `expo_router_sitemap` | List all routes (the sitemap) of the current Expo Router project. Use this when you are working with Expo Router and need to know which routes or screens exist in the app. | "check the expo-router-sitemap output" | Local (requires `expo-router` library) |
| `open_devtools` | Open React Native DevTools for the running app to debug JavaScript, inspect the component tree, and view console output. Requires a running dev server (Metro) for the project. | "open devtools" | Local |
| `collect_app_logs` | Collect logs over a short time window from the native device (Android logcat / iOS syslog) and/or the JavaScript console. Use this to debug runtime errors, crashes, or unexpected behavior in a running app. | "collect app logs from the iOS simulator" | Local |
| `automation_tap` | Tap on the running app at the given screen coordinates (x, y) or on the view with the given React Native testID. Provide either both x and y, or testID. Prefer testID when available, as it is resilient to layout changes. | "tap the screen at x=12, y=22" | Local |
| `automation_take_screenshot` | Take a screenshot of the running app — the full screen, or a specific view if a React Native testID is provided. Use this to visually verify the current UI state. | "take a screenshot and verify the blue circle view" | Local |
| `automation_find_view` | Find a view by its React Native testID and return its properties (position, size, and visibility). Use this to verify a view rendered correctly, or to obtain coordinates before calling automation_tap. | "dump properties for testID 'button-123'" | Local |

### Prompts

If your AI-assisted tool supports [MCP prompts](https://modelcontextprotocol.io/specification/2025-06-18/server/prompts), you may see additional menu options, such as [slash commands in Claude Code](https://code.claude.com/docs/en/mcp):

| Prompt | Description | Availability |
| --- | --- | --- |
| `expo_router_sitemap` | Query the all routes of the current expo-router project using `expo-router-sitemap`. | Local |

## Limitations

The current implementation has the following limitations:

-   Only supports a **single development server** connection at a time
-   iOS support for local capabilities is limited to simulators only (physical devices not yet supported)
-   iOS support for local capabilities is only available on macOS hosts.

## Data privacy

Expo does not use data sent to Expo MCP Server to train AI models. Expo MCP Server does not run an AI model itself. It provides MCP tools and prompts to the AI-assisted tool you connect, such as Claude Code, Cursor, or VS Code.

For server capabilities, Expo MCP Server may access Expo account and project data needed to complete the requested tool call, such as build, workflow, documentation, or TestFlight-related data. The result is returned to your AI-assisted tool over the MCP connection.

For local capabilities, data from your development machine is proxied through Expo MCP Server and returned to your AI-assisted tool. For example, when an AI-assisted tool asks to take a simulator screenshot, the data flow is:

1.  The local Expo development server captures the screenshot from your simulator.
2.  The screenshot is sent to Expo MCP Server.
3.  Expo MCP Server returns the screenshot to your local MCP client or AI-assisted tool.

Expo MCP Server returns data to the MCP client you connect, such as Claude Code, Cursor, VS Code, or Codex. From there, the client and its model provider may apply their own retention, zero data retention (ZDR), and training policies. Review those policies before enabling MCP access for projects that handle sensitive data, including HIPAA, SOC 2, or other regulated workloads.

## Additional resources

[Model Context Protocol Documentation](https://modelcontextprotocol.io/) — Learn more about the MCP specification and protocol details.
