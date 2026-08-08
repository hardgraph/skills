# Build a Slack bot that can do work with you and your team (/examples/general-agent-with-pi)

The agent is the easy part. [Pi](https://github.com/earendil-works/pi/tree/main/packages/coding-agent) does the reasoning; Composio gives it 1000+ apps to act on. In three lines you have an agent that can open a PR, check a calendar, or search a Notion workspace for one user.

The work is everything around it: putting that agent in Slack, where a whole team talks to it, and making it act as *each* person while posting as one bot. That's a handful of Composio pieces:

1. **Triggers** deliver every Slack message to your server as a webhook.
2. **Sessions** give each user their own scoped toolset, so the agent acts as *them*.
3. **A shared connection** lets the bot speak as the workspace bot, with one install for everyone.
4. **Redirected auth links** keep OAuth out of the channel: when an app isn't connected, the bot DMs the user a link and resumes on approval.
5. **The proxy** reaches the Slack Web API endpoints the toolkit doesn't wrap as tools.

Below you build the whole thing from scratch: a basic agent first, then a piece at a time up to the full server, then a browse of the real source. You bring a Composio API key and an agent runtime. Composio brings the workspace.

# Setup [#setup]

You need a [Composio API key](https://dashboard.composio.dev?utm_source=docs\&utm_medium=content\&utm_campaign=examples-general-agent-with-pi), a publicly reachable URL for your server, and [Bun](https://bun.sh).

**No public URL? Use a Cloudflare tunnel**

Composio posts webhooks to your server, so it needs a public URL. In local development, run a [Cloudflare tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) to expose your local port:

```bash
cloudflared tunnel --url http://localhost:3000
```

Use the `https://…trycloudflare.com` URL it prints as your `APP_URL`.

```bash
bun add @composio/core @composio/experimental @earendil-works/pi-coding-agent
```

# Install the bot [#install-the-bot]

A Slack bot needs a Slack app to authenticate as and a stream of events. Composio gives you both, so you never register a webhook with Slack or hold a bot token. The `slackbot&#x60; toolkit ships with Composio-managed OAuth, and you install it as one &#x2A;*[shared connection](/docs/shared-connections)** for the whole workspace.

This is `install.ts`, run once, built up three steps at a time:

**`install.ts` — complete file**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });

// The scopes the bot needs. The slackbot toolkit ships Composio-managed OAuth,
// so you never register your own Slack app.
const authConfig = await composio.authConfigs.create('slackbot', {
  type: 'use_composio_managed_auth',
  name: 'workspace-bot',
  credentials: {
    scopes: ['app_mentions:read', 'channels:history', 'chat:write', 'reactions:write', 'users:read'],
    user_scopes: ['search:read'],
  },
});

// One connection for the whole workspace: authorize it as SHARED.
const setup = await composio.create('setup:workspace-bot', {
  toolkits: ['slackbot'],
  authConfigs: { slackbot: authConfig.id },
  manageConnections: true,
});
const request = await setup.authorize('slackbot', {
  callbackUrl: `${process.env.APP_URL}/setup/callback`,
  experimental: { accountType: 'SHARED' },
});
console.log('Approve the install:', request.redirectUrl);

// On the OAuth callback: open the ACL, subscribe your webhook, create triggers.
// Persist connectedAccountId as SLACK_CONNECTION_ID for the bot server.
export async function onSetupCallback(connectedAccountId: string) {
  await composio.connectedAccounts.updateAcl(connectedAccountId, { allowAllUsers: true });
  await composio.triggers.setWebhookSubscription({ webhookUrl: `${process.env.APP_URL}/webhooks/composio` });
  await composio.triggers.create('setup:workspace-bot', 'SLACKBOT_CHANNEL_MESSAGE_RECEIVED', { triggerConfig: { is_bot_message: false } });
  await composio.triggers.create('setup:workspace-bot', 'SLACKBOT_DIRECT_MESSAGE_RECEIVED', { triggerConfig: {} });
}
```

A webhook subscription is the *pipe*; each trigger is a *tap*. Together they stream channel messages and DMs to your server. The connected account id that comes back from the OAuth callback is the `SLACK_CONNECTION_ID` the server pins into every session.

# Build the bot [#build-the-bot]

`bot.ts` starts as a bare three-line agent and grows into the server, one Composio concept at a time. Each diff below is exactly what that concept adds.

## Start with a basic agent [#start-with-a-basic-agent]

The whole idea, before any Slack: create a session for a user, hand the Pi provider the session so it can search and execute, and run a prompt. This already acts across every app that user has connected.

## Put it in a Slack thread [#put-it-in-a-slack-thread]

Turn the one-shot agent into a handler. Each Slack thread gets its own [session](/docs/configuring-sessions), reused so the agent keeps context, and the reply goes back with the `SLACKBOT_SEND_MESSAGE` tool. The session is keyed to the Slack user, so when Alice asks for a GitHub issue it opens as *Alice*, against her GitHub connection.

## Share one workspace connection [#share-one-workspace-connection]

By default a connected account is **PRIVATE**: only its creator can use it. The install authorized the Slack connection as **SHARED**, so you pin it into every session. Now Alice's session has *her* GitHub connection but *the workspace's* Slack connection. It posts as the bot, and acts everywhere else as Alice.

## Reach the gaps with the proxy [#reach-the-gaps-with-the-proxy]

Most Slack actions are `SLACKBOT_*` tools. The few that aren't, like the typing indicator and opening a DM channel, drop down to `session.proxyExecute`, which calls the Slack Web API with the pinned connection's auth so you never touch a token.

## Redirect auth links [#redirect-auth-links]

The payoff. When the agent reaches for an app the user hasn't connected, the tool result carries a one-time Composio connect URL. You never want it in the channel or in the model's context. The bot extracts it, **redacts** it from the tool output, DMs it to the user privately, and the run resumes the moment they approve, because the session was created with `waitForConnections`.

## Serve the webhook [#serve-the-webhook]

Verify each trigger's signature with `composio.triggers.verifyWebhook`, then hand the payload to `handleSlackMessage` off the response path so a slow handler doesn't get retried. That's the whole server.

# The whole project [#the-whole-project]

The two files above are the spine. The real project rounds them out with grouped auth-link DMs, per-user routing, message chunking, reaction acks, and durable storage. Here's a slice of the actual source, with the Composio touch-points highlighted. Browse the tree, read the files:

> The Slack bot browser is a documentation snapshot; a public repository is not available.

# Run it [#run-it]

Run `bun install.ts` once to set up the bot, start the server with `bun bot.ts`, then `@mention` the bot in any channel. It opens a session as you, finds the tool it needs, runs it against your connections, and replies in thread as the workspace bot, usually within a few seconds. Ask it to do something in an app you haven't connected yet and it DMs you a link first, then continues once you approve.

- [Configuring sessions](/docs/configuring-sessions): Everything a session can scope: toolkits, tools, connections, and limits

- [Shared connections](/docs/shared-connections): SHARED vs PRIVATE accounts and the per-user ACL

---

📚 **More documentation:** [View all docs](https://docs.composio.dev/llms.txt) | [Glossary](https://docs.composio.dev/llms.mdx/reference/glossary) | [Examples](https://docs.composio.dev/llms.mdx/examples) | [API Reference](https://docs.composio.dev/llms.mdx/reference)

---

# Composio SDK — Notes for AI Code Generators

**Purpose:** Reference for generating current (v3) [Composio](https://composio.dev/) integration code.
**Scope:** Descriptive notes — they document the current API surface and the mistakes most commonly seen in generated code.

---

## 1. Recommended Integration: Sessions

Composio supports two integration modes: **Native Tools** (with a provider package) and **MCP** (no provider package needed).

### Native Tools

```python
# ✅ CORRECT — Python (defaults to OpenAI)
from composio import Composio

composio = Composio()
session = composio.create(user_id="user_123")
tools = session.tools()
# Pass tools to your agent/LLM framework
```

```typescript
// ✅ CORRECT — TypeScript (defaults to OpenAI)
import { Composio } from "@composio/core";

const composio = new Composio();
const session = await composio.create("user_123");
const tools = await session.tools();
// Pass tools to your agent/LLM framework
```

For other providers, pass the provider explicitly. Provider packages follow the naming convention: `composio_<provider>` for Python, `@composio/<provider>` for TypeScript.

### MCP

Use `session.mcp.url` and `session.mcp.headers` with any MCP-compatible client (Claude Desktop, Cursor, OpenAI Agents, etc.). No provider package needed.

---

## 2. Integration Notes

- `composio.create(user_id)` is the standard entry point for agent integrations. Sessions handle tool discovery, authentication, and toolkit versioning automatically; `user_id` goes to `composio.create()` — individual tool calls in session mode don't take one.
- Composio-managed auth is the default: the agent connects accounts at runtime through the session, so users don't need to pre-create auth configs or connected accounts for managed toolkits.
- Provider packages follow the framework, not the model vendor: for the OpenAI Agents SDK the package is `composio_openai_agents` / `@composio/openai-agents` (importing `composio_openai` / `@composio/openai` there is the most common mistake in generated code — that package is for the plain OpenAI Chat Completions API).
- **Direct execution** (`composio.tools.get()`, `composio.tools.execute()`, `provider.handle_tool_calls()`) is a fully supported lower-level interface: your code picks the tool, no runtime discovery. It fits deterministic workflows and scripts; sessions fit agents that decide at runtime. The tradeoffs are documented at https://docs.composio.dev/docs/sessions-vs-direct-execution. Note that direct execution requires a toolkit version (https://docs.composio.dev/docs/tools-direct/toolkit-versioning).

---

# 3. Calling the REST API directly

## REST API version

The current REST API version is **v3.1**, served at `https://backend.composio.dev/api/v3.1`. Prefer it for new code and new examples.

`https://backend.composio.dev/api/v3` is the previous version. It is frozen with pinned tool-version defaults and remains supported — existing v3 integrations keep working and do not need to migrate.

## Tool-endpoint version defaults on v3.1

On v3.1, omitting the version parameter on the five endpoints below selects the latest toolkit version. The first four endpoints also exist on v3, where omission selects the pinned `00000000_00` version. `POST /tools/scopes/required` is v3.1-only.

| Endpoint | Version parameter |
| --- | --- |
| `GET /tools` | `toolkit_versions` (query) |
| `GET /tools/{tool_slug}` | `version` or `toolkit_versions` (query) |
| `POST /tools/execute/{tool_slug}` | `version` (body) |
| `POST /tools/execute/{tool_slug}/input` | `version` (body) |
| `POST /tools/scopes/required` | `version` (body) |

A v3.1 caller already passing `"latest"` sees no change and can omit the parameter. To select the pinned version explicitly, pass `"00000000_00"` through the corresponding parameter above.

This version-default change is limited to the five endpoints above.


---

## Terminology Migration (old → current)

If you encounter these terms in error messages, old documentation, or user prompts, translate them to the current equivalents. **Do not use the old terms in generated code or explanations.**

| Old term (v1/v2) | Current term (v3) | In code |
|---|---|---|
| entity ID | user ID | `user_id` parameter |
| actions | tools | e.g., `GITHUB_CREATE_ISSUE` is a *tool* |
| apps / appType | toolkits | e.g., `github` is a *toolkit* |
| integration / integration ID | auth config / auth config ID | `auth_config_id` parameter |
| connection | connected account | `connected_accounts` namespace |
| ComposioToolSet / OpenAIToolSet | `Composio` class with a provider | `Composio(provider=...)` |
| toolset | provider | e.g., `OpenAIProvider` |

If a user says "entity ID", they mean `user_id`. If they say "integration", they mean "auth config". Always respond using the current terminology.

