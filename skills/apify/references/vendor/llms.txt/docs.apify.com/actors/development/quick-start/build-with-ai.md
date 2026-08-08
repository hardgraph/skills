---
title: Build Actors with AI
url: https://docs.apify.com/actors/development/quick-start/build-with-ai.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Quick start](https://docs.apify.com/actors/development/quick-start.md)
previous: [Web IDE](https://docs.apify.com/actors/development/quick-start/web-ide.md)
next: [Develop AI agents](https://docs.apify.com/actors/development/quick-start/develop-ai-agents.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build Actors with AI

This guide provides best practices for building new Actors or improving existing ones using AI code generation tools by providing the AI agents with the right instructions and context.

Different goal?

* *Building and deploying AI agents as Actors on Apify?* See [Develop AI agents on Apify](https://docs.apify.com/actors/development/quick-start/develop-ai-agents.md) for the full stack - templates, sandboxes, LLM access, and monetization.
* *Connecting an external AI agent to Apify?* See [Apify for AI agents](https://docs.apify.com/get-started/agent-onboarding.md) for MCP, Agent Skills, client libraries, and the REST API.

The methods on this page are complementary. Start with the AI coding assistant instructions or Actor templates with AGENTS.md to get going, then add Agent Skills and the Apify MCP server to give your assistant more context and better results.

## Quick start

**Start with a prompt**

1. Create a directory: `mkdir my-new-actor`.
2. Open the directory in *Cursor*, *Claude Code*, *VS Code with GitHub Copilot*, etc. Using Claude Code on the web? See the setup note below first.
3. Copy the AI coding assistant prompt and paste it into your AI coding assistant.
4. Run it, and develop your first Actor with the help of AI.

**Start with a template**

1. [Install the Apify CLI](https://docs.apify.com/cli/docs/installation) if you haven't already.
2. Run `apify create` to initialize an Actor from a [template](https://apify.com/templates) (includes AGENTS.md).
3. Open the project in *Cursor*, *Claude Code*, *VS Code with GitHub Copilot*, etc. Using Claude Code on the web? See the setup note below first.
4. Start developing - your AI coding assistant automatically picks up context from AGENTS.md.

Allow Apify in Claude Code on the web

If you're using Claude Code on the web, your cloud sessions run inside a network-restricted sandbox. By default, the sandbox blocks outbound requests to `*.apify.com`, making documentation, the API, and the MCP server unreachable.

To grant access, add `*.apify.com` to a cloud environment's allowed domains:

1. Open the environment selector and select **Add environment**.
2. Set **Network access** to **Custom**.
3. Check **Also include default list of common package managers** so existing functionality stays intact.
4. Add `*.apify.com` to the **Allowed domains** list.

Alternatively, set **Network access** to **Full** to allow any domain.

For more details, see [Claude Code on the web: Network access](https://code.claude.com/docs/en/claude-code-on-the-web#network-access).

Codex Cloud uses a similar default-deny sandbox. See [Codex Cloud internet access](https://developers.openai.com/codex/cloud/internet-access) for the equivalent configuration.

## AI coding assistant instructions

Use the following prompt in your AI coding assistant such as [Cursor](https://cursor.com/), [Claude Code](https://claude.com/product/claude-code), or [GitHub Copilot](https://github.com/features/copilot):

Use pre-built prompt for your AI coding assistant

Show promptCopy prompt

The prompt guides your AI coding assistant to create and deploy an Apify Actor step by step. It walks through setting up the Actor structure, configuring all required files, installing dependencies, running it locally, logging in, and pushing it to the Apify platform.

## Use Actor templates with AGENTS.md

All [Actor Templates](https://apify.com/templates) have AGENTS.md that will help you with AI coding. You can use the [Apify CLI](https://docs.apify.com/cli/docs) to create Actors from Actor Templates.


```bash
apify create
```


If you do not have the Apify CLI installed, see the [installation guide](https://docs.apify.com/cli/docs/installation).

The command above will guide you through Apify Actor initialization, where you select an Actor Template that works for you. The result is an initialized Actor (with AGENTS.md) ready for development.

## Use Agent Skills

[Agent Skills](https://github.com/apify/agent-skills) are official Apify skills for Actor development, web scraping, data extraction, automation, etc. They work with Claude Code, Cursor, Codex, Gemini CLI, and other AI coding assistants.

Install Agent Skills in your project directory:


```bash
npx skills add apify/agent-skills
```


This adds skill files to your project that AI coding assistants automatically discover and use for context. No additional configuration is needed.

## Use Apify MCP server

The Apify MCP server has tools to search and fetch documentation. If you set it up in your AI editor, it will help you improve the generated code by providing additional context to the AI.

Use Apify MCP server configuration

We have prepared the [Apify MCP server configuration](https://mcp.apify.com/), which you can configure for your needs.

**Cursor**

To add Apify MCP server to Cursor manually:

1. Create or open the `.cursor/mcp.json` file.

2. Add the following to the configuration file:


   ```json
   {

     "mcpServers": {

       "apify": {

         "url": "https://mcp.apify.com/?tools=docs"

       }

     }

   }
   ```


**VS Code**

VS Code supports MCP through MCP-compatible extensions like *GitHub Copilot*, *Cline*, or *Roo Code*.

1. Install an MCP-compatible extension (e.g., GitHub Copilot, Cline).

2. Locate the extension's MCP settings or configuration file (often `mcp.json`).

   * For *GitHub Copilot*: Run the **MCP: Open User Configuration** command.
   * For *Cline* or *Roo Code*: Go to the **MCP Servers** tab in the extension interface.

3. Add the Apify server configuration:


   ```json
   {

     "mcpServers": {

       "apify": {

         "url": "https://mcp.apify.com/?tools=docs"

       }

     }

   }
   ```


**Claude Code**

Run the following command to add the Apify MCP server:


```bash
claude mcp add apify "https://mcp.apify.com/?tools=docs" -t http
```


## Provide context to assistants

Every page in the Apify documentation has a **** button. Use it to add more context to your AI assistant, or even open the page in ChatGPT, Claude, or Perplexity and ask additional questions.

![Page from the Apify documentation with the Copy for LLM button highlighted](/assets/images/copy-for-llm-button-11c4dad9364daa608ea09135afbba4c1.svg)

## Use `/llms.txt` files

The entire Apify documentation is available in Markdown format for use with LLMs and AI coding tools. Two consolidated files are available:

* `https://docs.apify.com/llms.txt`: A Markdown file with an index of all documentation pages in Markdown format, based on the [llmstxt.org](https://llmstxt.org/) standard.
* `https://docs.apify.com/llms-full.txt`: All Apify documentation consolidated in a single Markdown file.

Access Markdown source

Add `.md` to any documentation page URL to view its Markdown source.

Example: `https://docs.apify.com/actors` > `https://docs.apify.com/actors.md`

Provide link to AI assistants

LLMs don't automatically discover `llms.txt` files, you need to add the link manually to improve the quality of answers.

## Best practices

* *Small tasks*: Don't ask AI for many tasks at once. Break complex problems into smaller pieces. Solve them step by step.

* *Iterative approach*: Work iteratively with clear steps. Start with a basic implementation and gradually add complexity.

* *Versioning*: Version your changes often using git. This lets you track changes, roll back if needed, and maintain a clear history.

* *Security*: Don't expose API keys, secrets, or sensitive information in your code or conversations with LLM assistants.
