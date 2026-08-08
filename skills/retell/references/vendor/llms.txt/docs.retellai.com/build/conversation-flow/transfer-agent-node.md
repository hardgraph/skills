> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agent Transfer Node

> Agent transfer nodes (agent swap) hand a Retell call from one AI agent to another, useful for specialized agents, language switches, and modular call flows.

In advanced call flows, it's common to switch the handling agent, transferring the conversation from one AI agent to another. **Agent Transfer** (also known as **Agent Swap**) enables you to modularize tasks and re-use specialized agents without relying on [traditional phone-based transfers](/build/single-multi-prompt/transfer-call). Examples include:

* Transferring from a front-desk agent to an appointment-booking agent based on task.
* Transferring from an agent speaking one language to another agent handling a different language, based on user preference.

## Why Use Agent Transfer Instead of Call Transfer?

Compared to transferring to another agent using [transfer call](/build/conversation-flow/call-transfer-node), **Agent Transfer** offers significant advantages:

* **Lower Latency**: The transition between agents is near-instant, much lower than transfer call.
* **Better Reliability**: No need to create a new phone call, avoiding potential telephony failures.
* **No Handoff Message Needed**: The destination agent has access to the full conversation history, eliminating the need for adding hand-off messages or repeated customer questions.
* **No Separate Numbers for Agents**: Agents receiving transfers don’t need their own phone numbers. One number is all you need, no matter how many agents you transfer to.

## What stays fixed and what switches

Some call-level settings are pinned to the **first** agent for the whole call, no matter how many swaps happen:

* **Recording access** (`opt_in_signed_url`)
* **Data storage setting** (`data_storage_setting`)
* **PII redaction** (`pii_config`)
* **Denoising mode**

All other settings (language, voice, voiceModel, LLM/model, prompt, and tools) reflect the currently active agent. Use **keep the same voice** or **keep the current language** to carry the current voice or language into the destination agent instead.

Webhook delivery is configurable per transfer: by default only the source agent's webhook fires, but you can send events to the transferred agent's webhook or both.

For a fuller walkthrough of what the destination agent inherits (transcript, metadata, and dynamic variables) and every configuration option, see [Agent Transfer](/build/single-multi-prompt/transfer-agent).

## Steps

<Steps>
  <Step title="Add Agent Transfer Node">
    Select "Agent Transfer" from the 'Add New Node' menu.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/XoiOOpBkax1duk4y/images/cf/transfer-agent.png?fit=max&auto=format&n=XoiOOpBkax1duk4y&q=85&s=35f5316e53ababe990e132beeb4d2f3d" data-path="images/cf/transfer-agent.png" />
    </Frame>
  </Step>

  <Step title="Configure Details">
    You can configure the following main settings:

    * **Transfer agent**: the ID and version of a specific agent to transfer to. You can select the latest version as well.
    * **Speak during execution and messages**: if the agent should speak something while performing the transfer.
    * **Post call analysis setting**: choose which agent's post-call analysis applies after the transfer. Select **Only transferred agent** to run post-call analysis with the destination agent's full analysis configuration (its analysis fields, analysis model, and analysis prompts), or **Both this agent and transferred agent** to keep the analysis fields from both agents.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/XoiOOpBkax1duk4y/images/cf/transfer-agent-detail.png?fit=max&auto=format&n=XoiOOpBkax1duk4y&q=85&s=512a549c0212f7b7cca3a60c6c64416b" data-path="images/cf/transfer-agent-detail.png" />
    </Frame>
  </Step>

  <Step title="Test and Debug">
    You can test agent transfer both in web call and playground.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/cf/transfer-agent-test.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=bfbf835ca2e3dd2a68266df5d9d2ad60" data-path="images/cf/transfer-agent-test.png" />
    </Frame>
  </Step>
</Steps>
