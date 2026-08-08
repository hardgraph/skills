> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create chat agent

> Build a Retell chat agent in the dashboard or API for text conversations, convert a voice agent to chat, and deploy via web widget, SMS, or API.

Chat agents hold text conversations with your users. They use the same prompt types, [functions](/build/add-function-calling), and [knowledge bases](/build/knowledge-base) as voice agents, so you can offer the experience you built for calls over text as well — on your website, over SMS, or inside your own app.

<Note>
  Retell has no built-in integrations with third-party chat platforms such as WhatsApp or Messenger. Beyond the [website widget](/deploy/chat-widget) and [SMS](/deploy/enable-sms), you connect your own channel through the [chat API](/api-references/create-chat).
</Note>

## When to use a chat agent

Use a chat agent when your users type instead of talk:

* **Website support** — answer product, billing, or order questions in a chat widget on your site.
* **SMS conversations** — run two-way text threads on a Twilio number, such as appointment reminders with reschedule handling.
* **In-app assistant** — power a chat UI you build into your own web or mobile app.

For example, an e-commerce store embeds a chat agent in its help center to handle order-status questions: the agent looks up the order with a custom function and replies with the tracking link, so support staff only see the conversations the agent escalates.

## Create a chat agent

<Steps>
  <Step title="Start a new agent">
    In the dashboard, open **Agents**, click **Create an Agent**, and select **Chat Agent**.
  </Step>

  <Step title="Pick an agent type">
    Choose **Single prompt** for simple free-form conversations or **Conversational flow** for production-ready, deterministic flows. **Multi-Prompt (Legacy)** is available under **Other options**. Custom LLM is voice-only, so it can't power a chat agent.
  </Step>

  <Step title="Configure the agent">
    Write the [prompt](/build/prompt-engineering-guide), attach functions and a knowledge base, and adjust the chat settings described below.
  </Step>
</Steps>

<Frame caption="The Create an Agent menu on the Agents page, with the Chat Agent option highlighted.">
  <img src="https://mintcdn.com/retellai/yfI-f20iptEBfJzl/images/create-chat-agent.png?fit=max&auto=format&n=yfI-f20iptEBfJzl&q=85&s=2a7c5049e6841edf7dfb9d3836c2d699" alt="Retell dashboard Agents page with the Create an Agent dropdown open, showing Voice Agent and Chat Agent options with Chat Agent highlighted" width="1600" height="900" data-path="images/create-chat-agent.png" />
</Frame>

To create chat agents programmatically, call [Create Chat Agent](/api-references/create-chat-agent) with a Retell LLM or conversation flow response engine.

## Convert an existing voice agent

Converting creates a **new chat agent** based on your voice agent's setup. The original voice agent is not modified and keeps working as before.

<Steps>
  <Step title="Open the voice agent">
    In the agent builder, click the **More options** menu in the top-right corner and select **Convert to Chat Agent**.
  </Step>

  <Step title="Confirm the conversion">
    A dialog confirms that a new chat agent will be created and your original agent stays unchanged. Click **Convert**.
  </Step>

  <Step title="Review the new chat agent">
    Retell opens the new chat agent. Voice-only features are removed during conversion, so review the prompt and flow and adjust anything that referenced them.
  </Step>
</Steps>

Conversion removes what doesn't apply to text:

* **Nodes and functions**: call transfer (including bridge and cancel transfer), press digit (IVR navigation), and mid-call SMS are deleted, and any dangling flow connections are cleaned up.
* **Settings**: voice, speech, transcription, and call settings (voicemail detection, DTMF input, and similar) don't exist on chat agents.

<Frame caption="Converting a voice agent: open More options, select Convert to Chat Agent, and confirm in the dialog.">
  <img src="https://mintcdn.com/retellai/yfI-f20iptEBfJzl/images/convert-chat-agent.gif?s=400c3f3cfb24e497f7d4699d7fb95b33" alt="Three-step sequence in the agent builder: the More options button in the top-right corner is highlighted, then the open menu with Convert to Chat Agent highlighted, then the confirmation dialog explaining a new chat agent will be created while the original stays unchanged, with the Convert button highlighted" width="1280" height="720" data-path="images/convert-chat-agent.gif" />
</Frame>

Conversion works in both directions: a chat agent's **More options** menu offers **Convert to Voice Agent**.

## Chat settings

Chat agents share most configuration with voice agents — prompt, functions, knowledge base, [webhooks](/features/webhook-overview), security, and post-chat analysis — and add a **Chat Settings** section:

* **Auto-close inactive chats** — end the chat automatically when the user stops responding. Set the timeout anywhere from 2 minutes to 72 hours (`end_chat_after_silence_ms`; the API default is 1 hour).
* **Auto-close message** — an optional message sent when a chat is closed automatically (`auto_close_message`).

For chat events, webhooks fire `chat_started`, `chat_ended`, and `chat_analyzed` by default; add `transcript_updated` through the agent's `webhook_events` if you need per-message updates.

## Deploy your chat agent

Chat agents are headless — Retell does not host a chat UI for end users, so you choose how to surface the agent:

* **Website widget** — embed a single `<script>` tag on your site for text (and optional voice) chat with no backend required. See [Retell Website Widget](/deploy/chat-widget).
* **SMS** — connect the agent to a Twilio phone number so users can text it directly. See [Enable SMS](/deploy/enable-sms).
* **Custom integration** — build your own UI on the chat API: [Create chat](/api-references/create-chat) starts a session, [Create chat completion](/deploy/create-chat-completion) exchanges messages, and the session stays open until you call [End chat](/api-references/end-chat) or the inactivity timeout closes it.

Once live, set up [alert rules](/features/alerting-overview) on chat volume, success rate, negative sentiment, or cost to hear about problems without watching the dashboard.

## FAQ

<AccordionGroup>
  <Accordion title="Can a chat agent use my custom LLM?">
    No. Chat agents support Retell LLM (single and multi prompt) and conversation flow response engines. Custom LLM agents are voice-only.
  </Accordion>

  <Accordion title="Does converting a voice agent change or delete the original?">
    No. Conversion creates a separate chat agent; the original voice agent is untouched and any phone numbers attached to it keep working.
  </Accordion>

  <Accordion title="How does a chat session end?">
    Three ways: the agent ends it (an end-chat node or function), you end it with the [End chat API](/api-references/end-chat), or the user goes silent past the inactivity timeout and Retell closes it automatically.
  </Accordion>

  <Accordion title="How are chat agents billed?">
    Chat is billed per agent message, with add-ons (such as guardrails) also charged per message. See [pricing](https://www.retellai.com/pricing) for current rates.
  </Accordion>
</AccordionGroup>
