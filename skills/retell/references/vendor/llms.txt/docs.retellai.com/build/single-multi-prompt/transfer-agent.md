> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Agent Transfer tool: hand a call to another AI agent

> Add the Agent Transfer (agent swap) tool to a Retell single or multi-prompt agent so it can hand the live call to another AI agent mid-conversation.

**Agent Transfer** (also called **agent swap**) hands a live conversation from one AI agent to another mid-call. The destination agent picks up with the full transcript and context, so you can split a call across specialized agents without a phone-based transfer.

Common uses:

* Route a caller from a front-desk agent to an appointment-booking agent once you know the task.
* Switch from an agent speaking one language to an agent handling another, based on the caller's preference.
* Keep each agent's prompt focused on one job, then chain them (A → B → C) instead of maintaining one large prompt.

## When to use it

Reach for Agent Transfer when you want to move the caller between **AI agents on the same call**. Use it to modularize a long or branching flow, reuse a specialized agent across projects, or switch language.

Use [call transfer](/build/single-multi-prompt/transfer-call) instead when the destination is a **human or an external phone number**. Agent Transfer only swaps between Retell AI agents.

Two targets can't be swapped to: agents backed by a [custom LLM](/integrate-llm/overview) and chat-channel agents. Attempting either fails the transfer.

## Agent Transfer vs. call transfer

Compared with routing to another agent through [call transfer](/build/single-multi-prompt/transfer-call), Agent Transfer offers:

* **Lower latency.** The swap is near-instant, with no new phone call to place.
* **Better reliability.** No new call means no telephony failure to recover from.
* **No handoff message needed.** The destination agent already has the full conversation history, so the caller never repeats themselves.
* **One number for every agent.** Agents that receive transfers don't need their own phone numbers. A single number covers any number of agents.

Agent Transfer also works on **web calls**, not just phone calls.

## What the destination agent inherits

The swap keeps everything on the same call, with the same `call_id` and one entry in [call history](/features/session-history). The destination agent automatically receives:

* **Full conversation history.** The transcript up to the transfer point, so the new agent has the same context the caller does.
* **Call-level `metadata`.** Set on the original call (for example, via [Create Phone Call](/api-references/create-phone-call) or the [Inbound Call Webhook](/features/inbound-call-webhook)) and shared across every agent on the call.
* **Dynamic variables.** Both the `retell_llm_dynamic_variables` passed when the call started and any variables [extracted earlier in the call](/build/single-multi-prompt/extract-dv) stay available to the destination agent's prompt.

There is no "pass variables" parameter on the tool. The shared call context above is the mechanism. To have the destination agent act on a specific value, [extract it into a dynamic variable](/build/single-multi-prompt/extract-dv) on the source agent and reference that variable (`{{variable_name}}`) in the destination agent's prompt.

## What stays fixed and what switches

Some call-level settings are pinned to the **first** agent for the whole call, no matter how many swaps happen:

* **Recording access** (`opt_in_signed_url`): whether recordings use signed or private URLs.
* **Data storage setting** (`data_storage_setting`): what call data Retell stores.
* **PII redaction** (`pii_config`): the post-call PII scrubbing rules.
* **Denoising mode**: the audio denoising applied to the call.

Everything tied to the active agent switches to the destination agent on each swap: **language, voice, LLM/model, prompt, tools, knowledge bases, boosted keywords, interruption sensitivity, and voice speed.** Two exceptions are opt-in: turn on **keep the same voice** or **keep the current language** to carry the current voice or language into the destination agent instead of using its own.

Webhook delivery is configurable per transfer (see [Webhook setting](#configure-the-tool) below); by default only the source agent's webhook fires.

## Configure the tool

<Steps>
  <Step title="Add the Agent Transfer tool">
    In the **Functions** section, click **+ Add** and select **Agent Transfer** from the dropdown.

    <Frame caption="Add Agent Transfer from the Functions dropdown.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/agent-transfer/agent-transfer-add-function.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=c1873e87950f407acd10c9e84224e0b3" alt="Functions section of a single-prompt agent with the + Add dropdown open, listing End Call, Call Transfer, Agent Transfer, calendar and IVR functions, In-Call SMS, Extract Dynamic Variable, Code, and Custom Function, with Agent Transfer outlined in blue." style={{ maxHeight: '560px' }} width="1385" height="1120" data-path="images/agent-transfer/agent-transfer-add-function.png" />
    </Frame>
  </Step>

  <Step title="Configure the transfer">
    Set the following:

    * **Select agent:** the agent to transfer to. Pick a specific version or **Latest** so the transfer always uses the newest published version. The destination agent's own voice is used unless you keep the current one.
    * **Talk While Waiting** (optional): when on, the agent speaks a short filler line if the transfer takes more than \~2 seconds (for example, "Let me get you to the right person"). Provide the message as a prompt for the agent to phrase, or as static text to speak verbatim.
    * **Post-call analysis setting:** which agent's [post-call analysis](/features/post-call-analysis) runs after the transfer. **Only transferred agent** uses the destination agent's full analysis configuration (its fields, model, and prompts). **Both this agent and transferred agent** keeps the analysis fields from both; where field names collide, the destination agent's win. Including both increases usage cost.
    * **Webhook setting:** which agent's [webhook](/features/webhook-overview) receives call events after the transfer: only this (source) agent, only the transferred agent, or both. Defaults to only the source agent.
    * **Keep the same voice / keep the current language** (optional): carry the current voice or language into the destination agent instead of switching to its own.

    <Frame caption="The Agent Transfer configuration window.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/agent-transfer/agent-transfer-config.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=0f855d1b9f954501509e9f56cb137d8f" alt="Agent Transfer window with Function Name set to agent_transfer, an empty Description, a Select Agent picker with Keep the same voice and Keep the current language checkboxes, Post Call Analysis Setting with Only transferred agent selected, Webhook Setting with Only this agent selected, and a Talk While Waiting checkbox." style={{ maxHeight: '560px' }} width="994" height="1120" data-path="images/agent-transfer/agent-transfer-config.png" />
    </Frame>
  </Step>

  <Step title="Prompt the agent to trigger the transfer">
    The agent decides when to call the tool, so tell it why in the global prompt. For example:

    ```
    If the user asks to book an appointment, use the agent_transfer tool to transfer to the Appointment Agent.
    ```
  </Step>

  <Step title="Test and debug">
    Test the transfer in a web call or in the playground before going live.

    <Frame caption="Test the transfer in the playground before going live.">
      <img src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/multi-prompt/playground_test.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=c349f16a44ce7c1f69b0d58a711c7e3e" alt="The Retell playground running a test call, used to verify the agent transfer fires and the destination agent picks up the conversation." style={{ maxHeight: '560px' }} width="838" height="918" data-path="images/multi-prompt/playground_test.png" />
    </Frame>
  </Step>
</Steps>

## FAQ

<AccordionGroup>
  <Accordion title="Can an agent transfer more than once, or transfer back?">
    Yes. Each destination agent gets its own tools, including its own Agent Transfer tool, so you can chain swaps (A → B → C) and transfer back to a previous agent. There's no fixed cap on the number of transfers per call. The only limit is that a new transfer can't start while another is still in progress.
  </Accordion>

  <Accordion title="Does the caller notice the transfer?">
    Not unless you want them to. The swap is near-instant and stays on the same call. Turn on **Speak during execution** if you want the agent to say a short line while it happens.
  </Accordion>

  <Accordion title="Does a transfer create a new call or call ID?">
    No. It's the same call with the same `call_id`, and it appears as a single entry in call history. Transcript, metadata, and dynamic variables carry across the swap.
  </Accordion>

  <Accordion title="How is this different from agentic warm transfer, which also uses a second AI agent?">
    They solve different problems, and the second AI agent talks to a different person in each case.

    **Agent Transfer** swaps which AI is talking to *your caller*. There's no new phone call: it's the same call, the same `call_id`, and the conversation history carries over. Use it to move between AI agents.

    **[Agentic warm transfer](/build/single-multi-prompt/transfer-call)** is a mode of call transfer that places a real outbound call to a phone number. Its transfer agent talks to *the person who answers at the destination*, never to your caller — your caller is on hold the whole time. Use it to screen a human before handing off to them.

    Once that transfer agent bridges the call, your caller and the destination are connected directly and every AI drops off. With Agent Transfer, an AI is always still on the line.
  </Accordion>

  <Accordion title="Which agents can't I transfer to?">
    Agents backed by a [custom LLM](/integrate-llm/overview) and chat-channel agents. Transferring to either fails, and the agent continues on the current call.
  </Accordion>

  <Accordion title="What does the transfer tool look like in the API?">
    Its `type` is `agent_swap`, with an `agent_id` and optional `agent_version` (defaults to the latest). The LLM sees the tool under the **name** you give it, so name it clearly (for example, `transfer_to_appointment_agent`).
  </Accordion>
</AccordionGroup>

<Note>
  Building in [Conversation Flow](/build/conversation-flow/overview) instead of a single or multi-prompt agent? Use the [Agent Transfer node](/build/conversation-flow/transfer-agent-node). It has the same behavior, configured as a node.
</Note>
