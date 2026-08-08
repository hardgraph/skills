---
title: AI integrations
url: https://docs.apify.com/integrations/ai.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
children:
  - [Agno](https://docs.apify.com/integrations/agno.md)
  - [Amazon Bedrock](https://docs.apify.com/integrations/aws_bedrock.md)
  - [Claude](https://docs.apify.com/integrations/claude.md)
  - [CrewAI](https://docs.apify.com/integrations/crewai.md)
  - [Cursor](https://docs.apify.com/integrations/cursor.md)
  - [Flowise](https://docs.apify.com/integrations/flowise.md)
  - [GitHub Copilot](https://docs.apify.com/integrations/github-copilot.md)
  - [Google ADK](https://docs.apify.com/integrations/google-adk.md)
  - [Haystack](https://docs.apify.com/integrations/haystack.md)
  - [LangChain](https://docs.apify.com/integrations/langchain.md)
  - [Langflow](https://docs.apify.com/integrations/langflow.md)
  - [LangGraph](https://docs.apify.com/integrations/langgraph.md)
  - [Lindy](https://docs.apify.com/integrations/lindy.md)
  - [LlamaIndex](https://docs.apify.com/integrations/llama-index.md)
  - [Manus](https://docs.apify.com/integrations/manus.md)
  - [Mastra](https://docs.apify.com/integrations/mastra.md)
  - [MCP connectors](https://docs.apify.com/integrations/mcp-connectors.md)
  - [MCP server](https://docs.apify.com/integrations/mcp.md)
  - [Milvus](https://docs.apify.com/integrations/milvus.md)
  - [OpenAI](https://docs.apify.com/integrations/openai.md)
  - [OpenClaw](https://docs.apify.com/integrations/openclaw.md)
  - [Pinecone](https://docs.apify.com/integrations/pinecone.md)
  - [Qdrant](https://docs.apify.com/integrations/qdrant.md)
  - [Skyfire](https://docs.apify.com/integrations/skyfire.md)
  - [Strands Agents SDK](https://docs.apify.com/integrations/strands-agents.md)
  - [Upsonic](https://docs.apify.com/integrations/upsonic.md)
  - [Vercel AI SDK](https://docs.apify.com/integrations/vercel-ai-sdk.md)
  - [x402](https://docs.apify.com/integrations/x402.md)
previous: [Events types for webhooks](https://docs.apify.com/integrations/webhooks/events.md)
next: [Agno](https://docs.apify.com/integrations/agno.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# AI integrations

Plug Apify Actors into the AI stack - chat clients like Claude and ChatGPT via the Apify MCP server, agents built on SDKs and frameworks like OpenAI Agents, LangChain, and Vercel AI SDK, and downstream vector databases for retrieval.

## Featured

https://docs.apify.com/integrations/mcp.md

#### [Apify MCP server](https://docs.apify.com/integrations/mcp.md)

[Expose Apify Actors and storages to any MCP-compatible AI client.](https://docs.apify.com/integrations/mcp.md)

https://docs.apify.com/integrations/mcp-connectors.md

#### [MCP connectors](https://docs.apify.com/integrations/mcp-connectors.md)

[Let Apify Actors call third-party MCP servers (Notion, Slack, GitHub, and more) on a user's behalf.](https://docs.apify.com/integrations/mcp-connectors.md)

https://docs.apify.com/integrations/openai-agents.md

#### [OpenAI Agents SDK](https://docs.apify.com/integrations/openai-agents.md)

[Use Apify Actors as tools in agents built on the OpenAI Agents SDK.](https://docs.apify.com/integrations/openai-agents.md)

https://docs.apify.com/integrations/langchain.md

#### [LangChain](https://docs.apify.com/integrations/langchain.md)

[Wrap Apify Actors as LangChain tools for retrieval and agents.](https://docs.apify.com/integrations/langchain.md)

https://docs.apify.com/integrations/vercel-ai-sdk.md

#### [Vercel AI SDK](https://docs.apify.com/integrations/vercel-ai-sdk.md)

[Call Apify Actors from agents built on the Vercel AI SDK.](https://docs.apify.com/integrations/vercel-ai-sdk.md)

https://docs.apify.com/integrations/google-adk.md

#### [Google ADK](https://docs.apify.com/integrations/google-adk.md)

[Add Apify Actors as tools in Google Agent Development Kit agents.](https://docs.apify.com/integrations/google-adk.md)

https://docs.apify.com/integrations/pinecone.md

#### [Pinecone](https://docs.apify.com/integrations/pinecone.md)

[Embed and index scraped content in Pinecone for RAG.](https://docs.apify.com/integrations/pinecone.md)

https://docs.apify.com/integrations/manus.md

#### [Manus](https://docs.apify.com/integrations/manus.md)

[Apify Actors as tools inside the Manus autonomous agent.](https://docs.apify.com/integrations/manus.md)

https://docs.apify.com/integrations/openclaw.md

#### [OpenClaw](https://docs.apify.com/integrations/openclaw.md)

[Run Apify Actors from the OpenClaw assistant.](https://docs.apify.com/integrations/openclaw.md)

https://docs.apify.com/integrations/x402.md

#### [x402](https://docs.apify.com/integrations/x402.md)

[Use the x402 payments protocol with Apify Actors.](https://docs.apify.com/integrations/x402.md)

## AI assistants

Chat and coding assistants you connect to Apify through the Apify MCP server or a plugin.

https://docs.apify.com/integrations/chatgpt.md

#### [ChatGPT](https://docs.apify.com/integrations/chatgpt.md)

[Use Apify as an MCP server in ChatGPT to run Actors and pull live web data into the conversation.](https://docs.apify.com/integrations/chatgpt.md)

https://docs.apify.com/integrations/claude-desktop.md

#### [Claude Desktop](https://docs.apify.com/integrations/claude-desktop.md)

[Connect Apify to Claude Desktop through the MCP server to run Actors from your chats.](https://docs.apify.com/integrations/claude-desktop.md)

https://docs.apify.com/integrations/claude-code-cli.md

#### [Claude Code CLI](https://docs.apify.com/integrations/claude-code-cli.md)

[Use Apify Actors from the Claude Code CLI through the Apify MCP server.](https://docs.apify.com/integrations/claude-code-cli.md)

https://docs.apify.com/integrations/codex-app.md

#### [Codex (desktop app)](https://docs.apify.com/integrations/codex-app.md)

[Install the Apify plugin for Codex in the ChatGPT desktop app to discover, run, and build Actors.](https://docs.apify.com/integrations/codex-app.md)

https://docs.apify.com/integrations/codex-cli.md

#### [Codex CLI](https://docs.apify.com/integrations/codex-cli.md)

[Install the Apify plugin for Codex in your terminal to discover, run, and build Actors.](https://docs.apify.com/integrations/codex-cli.md)

https://docs.apify.com/integrations/cursor.md

#### [Cursor](https://docs.apify.com/integrations/cursor.md)

[Install the Apify plugin in Cursor to interact with Apify from the IDE.](https://docs.apify.com/integrations/cursor.md)

https://docs.apify.com/integrations/github-copilot.md

#### [GitHub Copilot](https://docs.apify.com/integrations/github-copilot.md)

[Install the Apify plugin in GitHub Copilot to discover, run, and build Actors in VS Code.](https://docs.apify.com/integrations/github-copilot.md)

## By provider

See everything for a provider on one page: the [OpenAI hub](https://docs.apify.com/integrations/openai.md) (ChatGPT, Codex, Agents SDK) and the [Claude hub](https://docs.apify.com/integrations/claude.md) (Claude Desktop, Claude Code CLI).
