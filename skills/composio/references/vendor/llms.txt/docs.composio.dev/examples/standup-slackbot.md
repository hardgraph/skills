# Daily standup bot (/examples/standup-slackbot)

Standup is a crucial part of running an effective engineering team, and also oh so tedious: every morning, everyone digs back through what they did and writes it up. It's worse for teams spread across timezones, where there's no shared standup to anchor the day, so it's easy to just forget.

But the work you did *is* there: the PRs in  GitHub, the docs in  Notion, the decisions in  Slack threads. If it's all recorded somewhere, an agent should be able to at least draft it. [Composio sessions](/docs/configuring-sessions) make this *incredibly easy for agents*: a session hands the agent everything it needs, [search](/docs/how-composio-works#meta-tools) to find the right tool, [parallel execute](/docs/how-composio-works#meta-tools) to run many at once, a [sandbox](/docs/sandbox/remote) and [volumes](/docs/sandbox/remote#files-and-mounts). It uses Composio to extract, parse, and cross-reference data across all those sources and write clean summaries of the real work your team shipped. All you have to do is create a session for your teammate and let it cook.

![The daily standup reminder in Slack](/images/standup-slackbot/slack-reminder.png)
*The daily reminder, with Draft and Connect more tools buttons*

![A generated standup draft in Slack](/images/standup-slackbot/slack-draft.png)
*The draft the agent writes, delivered to a teammate in Slack*

So we built a Slack bot that does exactly that. Once a day, at a set time in each teammate's own timezone, it reminds them to post in the daily standup thread in a central channel. With one button click, they can run a subagent that uses their Composio connections to generate a clean, consolidated draft to review and post. We'll build it step by step.

> **Is this the right example for you?**: This is a deliberately advanced, opinionated build. It's a strong reference for five things:

  * **Background-agent sessions**: the draft agent runs on a schedule, not in a conversation. It works from the tools a member already connected and never pauses to ask for auth.
  * **Manual execution for deterministic workflows**: outside the draft, the bot doesn't let an agent decide. It runs a fixed flow, calling tools directly with [manual execution](/docs/tools-direct/executing-tools), so a button always triggers the same exact steps.
  * **Manual, pre-connected auth**: members connect their tools ahead of time using [manual connections](/docs/manually-authenticating), and the agent just uses whatever is there.
  * **White-labelling** (advanced): your own Slack app and bot identity, via [white-labelling](/docs/white-labeling-authentication). This is *not* the easy path. We'd recommend Composio's managed apps, which require no additional configuration. Only do this if you specifically want your own branding.
  * **The proxy** (advanced): using [`proxyExecute`](/docs/extending-sessions/proxy-execute) to call Slack API endpoints Composio doesn't wrap as tools.

It is **not** an example of [in-chat or dynamic auth](/docs/authentication) (asking a user to connect a tool mid-run), and it's more setup than many bots need. If you'd rather have a Slack bot with zero setup (Composio's managed app) or in-chat auth, start with the [general Slack bot](/examples/general-agent-with-pi) instead.

The Slack bot itself follows a deterministic flow: the same menu every day. When a member taps a button, it launches a subagent with a Composio session to produce the draft. Here's the shape of it:

```mermaid
`flowchart TD
cron([Vercel cron]) --> thread[Find or create today's thread]
thread --> dm
dm@{ img: "/images/standup-slackbot/slack-reminder.png", label: "DM each due member: Draft or Connect", pos: "b", w: 300, h: 69, constraint: "on" }
dm -->|Draft| agent[Create a Composio session for the user and launch a sub-agent to generate summary]
agent --> review
review@{ img: "/images/standup-slackbot/slack-draft.png", label: "Member reviews: Confirm or Edit", pos: "b", w: 300, h: 143, constraint: "on" }
review -->|Confirm| post[Post into the thread as the member]
dm -->|Connect| oauth
oauth@{ img: "/images/standup-slackbot/slack-connect.png", label: "Creates buttons for the user to link their accounts to Composio via OAuth", pos: "b", w: 340, h: 58, constraint: "on" }
click agent "/docs/how-composio-works#meta-tools" "Composio metatools"
`
```

# Setup [#setup]

You need a [Composio API key](https://dashboard.composio.dev/~/project/settings/api-keys?utm_source=docs\&utm_medium=content\&utm_campaign=examples-standup-slackbot), a Slack workspace you can install an app into, and Node with [tsx](https://nodejs.org). The finished bot deploys to [Vercel](https://vercel.com) as two serverless functions, a cron and an interactivity handler, so there's no long-running server.

# Make your custom Slack bot [#make-your-custom-slack-bot]

This bot doesn't post as "Composio". It posts as *my* app, with its own name, icon, and (frankly ridiculous) face:

![The Daily Standup Bot avatar](/images/standup-slackbot/bot-avatar.png)
*Create the app from scratch and name it*

**Add the Bot Token Scopes.** Under **OAuth & Permissions**, add: `chat:write`, `im:write`, `channels:history`, `channels:read`, `users:read`, `users:read.email`, `team:read`. Then turn on **Interactivity** and point its Request URL at your deployment's `/api/interactivity`.

![Adding bot token scopes under OAuth & Permissions](/images/standup-slackbot/bot-scopes.png)
*Add the bot token scopes*

**Grab the app credentials.** On **Basic Information**, copy the **Client ID** and **Client Secret**. Composio drives the OAuth as your app with these.

![The app's Client ID and Secret under Basic Information](/images/standup-slackbot/app-credentials.png)
*Copy the Client ID and Secret*

# Auth the bot [#auth-the-bot]

The Slack app exists; now connect it through Composio so your code can act as it. You create one `slackbot` auth config from your credentials, then a setup script does the OAuth once with Composio's [manual authentication](/docs/manually-authenticating) flow.

> **`slackbot` vs `slack`**: Composio has two Slack toolkits, and this bot uses both:

  * **`slackbot`** authenticates a Slack *app* and acts as the **bot** (a bot token). It posts the reminders and drafts as "Daily Standup Bot," and it's the one you white-label here.
  * **`slack`** authenticates an individual **user** and acts as *them* (a user token). Each teammate connects this so the bot can post their standup under their own name and read their activity for context.

Rule of thumb: posting *as the bot* uses `slackbot`; doing something *as a person* uses `slack`.

**Create an auth config and pick the `Slackbot` toolkit.** In the [Composio dashboard](https://dashboard.composio.dev/~/project/auth-configs?utm_source=docs\&utm_medium=content\&utm_campaign=examples-standup-slackbot), click **Create Auth Config** and search `slackbot`. Choose **Slackbot**, *not* `Slack`: `Slackbot` posts as the bot identity, while `Slack` acts as an individual user.

![Choosing the Slackbot toolkit, not Slack](/images/standup-slackbot/auth-config-slackbot.png)
*Pick Slackbot, not Slack*

**Use your own credentials.** Pick **OAuth 2.0**, then **Your Own Credentials**, and paste the Client ID and Secret from before. Add `team:read` to the user token scopes. This is the white-label step: your app, your name, your face.

![Selecting Your Own Credentials and entering the Client ID and Secret](/images/standup-slackbot/auth-config-credentials.png)
*Use your own credentials*

**Save the auth config id.** Once created, copy its `ac_...` id into `COMPOSIO_SLACKBOT_AUTH_CONFIG_ID`. This is the one auth config your app uses to take actions on behalf of your bot.

![The created slackbot auth config with its ac_ id](/images/standup-slackbot/auth-config-created.png)
*The created auth config and its ac_ id*

**Run the setup script to connect the bot.** For this bot, we first need to connect the bot itself to Composio, which only needs to be done once. The script creates an OAuth link for you to connect your *Slack bot* to Composio, which lets you use Composio to send messages on behalf of your bot.

**`scripts/setup.ts` — complete file**

```typescript
import { Composio } from '@composio/core';

const composio = new Composio({ apiKey: process.env.COMPOSIO_API_KEY });
const AUTH_CONFIG = process.env.COMPOSIO_SLACKBOT_AUTH_CONFIG_ID!;

// Connect the bot's own Slack app once, so it can post and DM as the bot.
async function main() {
  const session = await composio.create('default', {
    authConfigs: { slackbot: AUTH_CONFIG },
  });

  const toolkits = await session.toolkits({ toolkits: ['slackbot'] });
  const active = toolkits.items.find((t) => t.slug === 'slackbot')?.connection?.isActive;
  if (active) {
    console.log('Bot already connected.');
    return;
  }

  // Not connected: print the Connect Link, then wait for the user to finish.
  const connectionRequest = await session.authorize('slackbot');
  console.log('Authorize the bot:', connectionRequest.redirectUrl);

  const account = await connectionRequest.waitForConnection();
  console.log('Bot connected:', account.id);
}

main();
```

The first run prints a link and waits:

```text
╭─────────────────────────────────────────────────────────╮
│  Daily Standup Bot: One-Time Setup                       │
╰─────────────────────────────────────────────────────────╯
  ✅ Auth config has the required user scopes.
  ·  Bot is not connected yet. Generating an authorization link…

  Open this URL in your browser to authorize the bot:

    https://backend.composio.dev/s/AbC123xy

  Waiting for you to complete the OAuth flow (Ctrl+C to abort)…
  ✅ Bot connected to Slack.

──────────────────────────────────────────────────────────────────────
  🎉 Setup complete. Invite the bot to your standup channel and
     point your Slack app's Interactivity Request URL at
     https://<your-deployment>/api/interactivity
──────────────────────────────────────────────────────────────────────
```

Open that link to approve the bot, and the connection goes live:

![Approving the bot's OAuth connection](/images/standup-slackbot/oauth-approve.png)
*Approve the bot in Slack*

![Composio successfully connected to Slackbot](/images/standup-slackbot/connected.png)
*Connected*

The script is idempotent and repeatable. Forgot a scope, or hit an issue? No stress, just re-run it with `--reconnect`.

# Talk to Slack [#talk-to-slack]

To send and update messages in our deterministic bot workflow, we use Composio's `SLACKBOT_SEND_MESSAGE` and `SLACKBOT_UPDATES_A_MESSAGE` tools via [manual tool execution](/docs/tools-direct/executing-tools). `SLACKBOT_SEND_MESSAGE` takes Block Kit `blocks`, so a message with interactive buttons can go through a tool too.

When a Slack action has no tool, like opening a modal (`views.open`), it drops to [`proxyExecute`](/docs/extending-sessions/proxy-execute): the escape hatch for anything the named tools don't cover, hitting any Slack Web API endpoint as a connected account with no token in your code.

# Make the buttons work [#make-the-buttons-work]

Our StandUp bot gives the user two options every morning: **Draft** or **Connect more tools**. Each message uses [Block Kit](https://api.slack.com/block-kit) to create those buttons. For each button we define an `action_id` that lets us recognise which button was clicked.

```ts
declare const memberEmail: string, dmChannel: string, dmTs: string;
// the reminder's Draft button
const draftButton = {
  type: 'button',
  style: 'primary',
  text: { type: 'plain_text', text: '📝 Draft' },
  action_id: 'draft',
  value: JSON.stringify({ memberEmail, dmChannel, dmTs }),
};
```

![The daily standup reminder in Slack](/images/standup-slackbot/slack-reminder.png)
*The daily reminder, with Draft and Connect more tools buttons*

When it's clicked, Slack POSTs to your `/api/interactivity` handler. Verify the request, ack within Slack's 3-second window, then route on the `action_id`:

**`api/interactivity.ts` — complete file**

```typescript
import { verifySlackSignature, readRawBody, updateMessage, postAsMember } from './_utils/slack';
import { generateDraft } from './_utils/agent';
import { draftMessage, connectMenu } from './_utils/blocks';

type SlackInteractionPayload = {
  actions?: Array<{
    action_id?: string;
    value?: string;
  }>;
};

// Slack POSTs here every time someone clicks a button. Verify it really came
// from Slack, then ack within 3 seconds (Slack retries if you're slow).
export default async function handler(req: Request, res: Response) {
  const body = await readRawBody(req);
  if (!verifySlackSignature(body, req.headers)) return res.status(401).end();

  const payload = JSON.parse(new URLSearchParams(body).get('payload') ?? '{}');
  res.status(200).end();        // ack immediately
  await handleClick(payload);   // then do the slow work
}

// Each button carried its context in `value`, so the handler knows exactly what
// to do. No model decides anything here: the flow is fixed.
async function handleClick(payload: SlackInteractionPayload) {
  const action = payload.actions?.[0];
  const ctx = JSON.parse(action?.value ?? '{}');

  if (action?.action_id === 'draft') {
    const draft = await generateDraft(ctx.memberEmail);   // launch the subagent
    await updateMessage(ctx.dmChannel, ctx.dmTs, draftMessage(draft, ctx));
  } else if (action?.action_id === 'connect') {
    await updateMessage(ctx.dmChannel, ctx.dmTs, connectMenu(ctx));
  } else if (action?.action_id === 'confirm') {
    await postAsMember(ctx.memberEmail, ctx.channel, ctx.draft, ctx.threadTs);
  }
}
```

**Connect more tools** generates a per-member OAuth link for each toolkit the member hasn't connected, so they can add a source without leaving Slack:

![The connect-more-tools menu in Slack](/images/standup-slackbot/slack-connect.png)
*Connect more tools, each button a per-member OAuth link*

**Edit** opens a modal (`views.open` through the proxy), and **Confirm** posts the draft into the day's thread as the member.

# Draft the standup [#draft-the-standup]

Now this is the cool and magical part, and the easy part: all the background agent needs is a tool-router session and a prompt. When a member taps **Draft**, you spin up a session scoped to the toolkit catalogue and let the agent research and write.

## A session writes the draft [#a-session-writes-the-draft]

A [tool-router session](/docs/configuring-sessions) gives the agent its tools. Pass the member's email and your full list of toolkits, hand the tools to the model, and let it investigate and write. You don't have to check which ones the member set up: the session only exposes tools for the accounts they've actually connected, and ignores the rest.

## Use what's connected, nothing more [#use-whats-connected-nothing-more]

The router can also *manage* connections, asking the user to authorize any toolkits they haven't connected yet. During a draft you don't want that: if the agent reaches for a tool the member hasn't connected, it should skip it, not prompt them to log in. `manageConnections: false` removes those meta-tools, so the agent drafts from exactly what's already connected.

The bot posts the result back as a draft the member can confirm or edit:

![A generated standup draft in Slack with Confirm and Edit buttons](/images/standup-slackbot/slack-draft.png)
*The draft the agent writes, delivered to a teammate in Slack*

# The whole project [#the-whole-project]

> The standup bot browser is a documentation snapshot; a public repository is not available.

# Run it [#run-it]

Edit `standup.config.ts` with your team (each member's Slack email and timezone, plus your channel and GitHub org), set your four environment variables, run `npx tsx scripts/setup.ts` once to connect your bot, then `vercel deploy`.

- [Configuring sessions](/docs/configuring-sessions): What a session can scope: toolkits, tools, connections, and connection management

- [White-labeling authentication](/docs/white-labeling-authentication): Ship a bot under your own app's name, icon, and credentials

- [Custom vs managed auth](/docs/custom-app-vs-managed-app): Bring-your-own Slack app versus a Composio-managed connection

- [Triggers](/docs/triggers): Run agents in response to events: schedules, webhooks, and app activity

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

