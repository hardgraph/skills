> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retell MCP server for AI assistants

> Use Retell's MCP server to build and manage voice agents from MCP-capable clients like Cursor, Claude Desktop, and Claude Code via Retell APIs.

## Overview

Retell supports the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) so you can build Retell AI voice agents directly from MCP-capable clients (Cursor, Claude Desktop, Claude Code, Codex-style tools, and more).

If you're already using Retell via REST or SDKs, MCP is simply a different interface to the same core functionality — optimized for agentic workflows in IDEs and assistants.

<Note>
  This page covers Retell's own MCP server, which lets your assistant manage your Retell account. To let one of your agents call an external MCP server's tools during a call, see [MCP tools for single and multi-prompt agents](/build/single-multi-prompt/mcp) or the [MCP node](/build/conversation-flow/mcp-node) for conversation flows.
</Note>

## What can Retell's MCP server do?

Retell's MCP server gives your AI assistant native access to the Retell platform through a single integration. Depending on your API key permissions and what you enable in your MCP client, an assistant can:

* **Agents**: create, update, publish, list, and fetch agent versions.
* **Calls**: create phone/web calls, fetch call details, list calls, delete calls, update call metadata, run QA and review.
* **Phone numbers**: import, provision, list, and fetch phone numbers.
* **Knowledge base**: create KBs, attach sources, remove sources, list KBs.
* **Voices**: list voices, clone voices, search community voices.
* **Chats**: create chats, create chat agents, end chats, update chat metadata.
* **Testing & QA**: create test cases, run tests, list/rerun QA, submit scores.
* **Alerts & webhooks**: create/list alert rules, list incidents, test webhooks.

## How it works

* **Runtime**: MCP clients connect to a hosted (remote) MCP endpoint over Streamable HTTP, authenticate with your Retell API key, then call tools.
* **Tool discovery**: your MCP client can list available tools (via `tools/list`), including each tool's name, description, and JSON input schema.

## Prerequisites

* A Retell API key ([get one here](/accounts/manage-api-keys))
* An MCP client (Cursor, Claude Desktop, Claude Code, etc.)
* Retell MCP server URL: `https://mcp.retellai.com`

**Authentication header format:**

```
Authorization: Bearer <RETELL_API_KEY>
```

## Setup

Most clients support remote MCP over HTTP. Configure:

* **URL**: `https://mcp.retellai.com`
* **Headers**: `Authorization: Bearer <RETELL_API_KEY>`

## Client setup

The exact UI differs by client version, but the configuration concepts are the same: server name, URL/transport, and authentication.

<Tabs>
  <Tab title="Cursor">
    Open the command palette and choose **Cursor Settings** → **MCP** → **Add new global MCP server**, then add:

    ```json theme={"dark"}
    {
      "mcpServers": {
        "retell": {
          "url": "https://mcp.retellai.com",
          "headers": {
            "Authorization": "Bearer <RETELL_API_KEY>"
          }
        }
      }
    }
    ```
  </Tab>

  <Tab title="Claude Desktop">
    Open Claude Desktop settings → **Developer** → **Edit Config**, then add:

    ```json theme={"dark"}
    {
      "mcpServers": {
        "retell": {
          "url": "https://mcp.retellai.com",
          "headers": {
            "Authorization": "Bearer <RETELL_API_KEY>"
          }
        }
      }
    }
    ```
  </Tab>

  <Tab title="Claude Code">
    Run the following command in your terminal:

    ```bash theme={"dark"}
    claude mcp add --transport http retell https://mcp.retellai.com \
      --header "Authorization: Bearer <RETELL_API_KEY>"
    ```
  </Tab>

  <Tab title="Codex">
    Add the following to your `~/.codex/config.toml`:

    ```toml theme={"dark"}
    [mcp_servers.retell]
    url = "https://mcp.retellai.com"
    bearer_token_env_var = "RETELL_API_KEY"
    ```

    Then set the environment variable:

    ```bash theme={"dark"}
    export RETELL_API_KEY="<RETELL_API_KEY>"
    ```
  </Tab>

  <Tab title="Other MCP clients">
    If your client supports remote MCP URLs + headers, configure:

    * **URL**: `https://mcp.retellai.com`
    * **Header**: `Authorization: Bearer <RETELL_API_KEY>`
  </Tab>
</Tabs>

## Prompt examples

Try these prompts in your MCP client to get started:

* "List my agents and summarize what each one does."
* "Create a new agent for inbound sales qualification and publish it."
* "Show me the last 20 calls and flag any with low QA scores."
* "Create a knowledge base and attach these sources, then update my agent to use it."
* "Import this phone number and assign it to my agent."
* "Rerun QA on this call and summarize the failure reasons."

## Security considerations

Connecting an LLM to operational tools introduces new risks. The primary risk class unique to LLM workflows is **prompt injection**: untrusted content (like call transcripts, user messages, or knowledge base documents) can include instructions that try to trick the model into taking unintended actions.

Most MCP clients support **"confirm before running tools"**. Keep that enabled and review tool calls carefully.

### Recommendations

MCP makes it easy to connect powerful tools to an assistant. Treat MCP like giving a programmatic operator access.

* **Use least privilege**: create keys with the minimum permissions needed.
* **Keep keys out of prompts**: store API keys in client secrets/settings, never paste them into chat.
* **Prefer read-first workflows**: fetch the current resource before you update or delete it.
* **Review destructive actions**: tools like delete and publish should be gated by explicit user intent.
* **Be careful with PII**: call logs and transcripts may contain sensitive data — avoid sending full transcripts back to the model if you don't need them.
* **Use non-production data when possible**: if you're exploring agent behavior or testing workflows, prefer development/sandbox environments.

## Troubleshooting

| Issue                | Solution                                                                                                           |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Tools not showing up | Verify the server URL, auth header, and that the client can reach the endpoint.                                    |
| 401 Unauthorized     | Confirm `Authorization: Bearer <RETELL_API_KEY>` and that the key is active.                                       |
| Tool call errors     | Re-run with smaller inputs, then inspect the returned error payload (many clients surface structured tool errors). |

## Feedback

If you run into issues or have requests for additional MCP capabilities, reach out to our support team at [support@retellai.com](mailto:support@retellai.com).
