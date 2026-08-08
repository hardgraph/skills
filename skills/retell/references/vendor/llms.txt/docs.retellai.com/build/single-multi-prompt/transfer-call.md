> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Transfer call tool (single & multi-prompt agents)

> Add the Transfer Call tool to a Retell single or multi-prompt agent to route calls to a human, with cold, warm, and agentic warm transfer modes.

Add the Transfer Call tool so your agent can hand a live phone call off to a human, a department, or another number when the conversation calls for it.

For example, a support agent can stay on the line to answer routine questions and transfer to a human the moment a caller asks for a refund or sounds frustrated.

<Warning>
  Transfer Call works on phone calls only, not web calls. It's available for Retell numbers and imported numbers.
</Warning>

To hand the conversation to another *AI* agent instead of a phone number, use [Agent Transfer](/build/single-multi-prompt/transfer-agent). It's lower-latency and passes the full conversation history to the next agent.

Building a conversation flow agent instead? Use the [call transfer node](/build/conversation-flow/call-transfer-node), which supports the same transfer types.

<Steps>
  <Step title="Add the Call Transfer function">
    In the **Functions** section, click **+ Add** and select **Call Transfer** from the dropdown.

    <Frame caption="Add Call Transfer from the Functions dropdown.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-add-function.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=9bd8bd82fe4982ab99f9aa490c5e34b7" alt="Functions section of a single-prompt agent with the + Add dropdown open, listing End Call, Call Transfer, Agent Transfer, calendar and IVR functions, In-Call SMS, Extract Dynamic Variable, Code, and Custom Function, with Call Transfer outlined in blue." style={{ maxHeight: '560px' }} width="1385" height="1120" data-path="images/transfer-call/transfer-call-add-function.png" />
    </Frame>
  </Step>

  <Step title="Set the transfer target">
    Set the transfer number to either:

    * a number in E.164 format, or a SIP URI in the form `sip:username@domain` (for example `sip:user@retellai.com`), or
    * a [dynamic variable](/build/dynamic-variables) that gets substituted at runtime.

    (Optional) If your transfer destination isn't in E.164 format, change **Format to E.164** to **Keep raw input** to keep the input as is. This applies only when you use custom telephony, not Retell Telephony.

    Set the transfer number extension if needed. The extension can contain `0-9`, `*`, and `#` (for example `123#`).

    <Frame caption="Enter the transfer number in E.164 format, or use a dynamic variable.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-target.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=95d09033387e1249c6470b36e88a19c4" alt="Transfer Call window with Name set to transfer_call, a description, and the Transfer to group outlined in blue showing the Static Destination and Dynamic Routing tabs, a Format to E.164 selector, an Extension Number checkbox, and the destination number +14155551234." style={{ maxHeight: '560px' }} width="1460" height="900" data-path="images/transfer-call/transfer-call-target.png" />
    </Frame>
  </Step>

  <Step title="Update the prompt">
    Tell the agent when to transfer by adding the condition to its prompt. For example:

    > If the user is angry or frustrated, use the transfer\_call tool to transfer the conversation to a human agent.
  </Step>

  <Step title="Choose the transfer type">
    Choose how the call is handed off:

    * **Cold Transfer**: The call goes straight to the destination and your agent drops off right away, like transferring a call and hanging up.
    * **Warm Transfer**: Your agent stays on the line after dialing the destination. It can wait for a real person to speak, play a private message only the destination hears, or do a quick three-way introduction before connecting your caller.
    * **Agentic Warm Transfer**: A second AI agent, the **transfer agent**, picks up the handoff, has a short back-and-forth with the destination, then decides whether to connect (bridge) your caller or cancel. Use this when you want an AI agent to screen or brief the destination first.

    Each type has its own settings, covered after these steps.

    <Frame caption="The three transfer types.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-types.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=4807cbd6ee982966aeef95e30bd4bee6" alt="Transfer Call window with the How should the AI handle the transfer group outlined in blue, offering Cold Transfer (AI transfers immediately) selected, Warm Transfer (AI gives a one-way brief to the agent), and Agentic Warm Transfer (AI has a 2-way conversation with agent, then bridges)." style={{ maxHeight: '560px' }} width="1460" height="716" data-path="images/transfer-call/transfer-call-types.png" />
    </Frame>
  </Step>

  <Step title="Configure caller ID (optional)">
    Choose which caller ID the transfer destination sees:

    1. **Retell Agent's Number**: The destination sees the Retell agent's number.
    2. **User's Number**: The destination sees the caller's number. Your telephony provider must support caller ID override for this to work.
       * Warm transfer uses SIP DIAL, setting the `from` and `P-Asserted-Identity` headers to the user's number.
       * Cold transfer uses SIP REFER; supporting caller ID override is up to the telephony provider.
       * Retell Twilio numbers support showing the user's number on both warm and cold transfer. Retell Telnyx numbers support it only via cold transfer (SIP REFER).
       * If caller ID override isn't supported, the transfer fails.

    <Frame caption="Choose whether the destination sees the Retell agent's number or the caller's number.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-caller-id.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=204d55ac5f4b5d63609d46003c1a89cc" alt="Transfer Call window with the Displayed Caller ID group outlined in blue, offering Retell Agent's Number selected and User's Number, between the SIP Transfer Method row and the Transfer Ring Duration slider." style={{ maxHeight: '560px' }} width="1460" height="620" data-path="images/transfer-call/transfer-call-caller-id.png" />
    </Frame>
  </Step>

  <Step title="Set the transfer ring duration (optional)">
    Set how long the destination rings before the attempt counts as unanswered. This applies to cold, warm, and agentic warm transfers.

    The value you set here applies only to this transfer. If you leave it unset, the transfer uses your agent-level ring duration.

    <Frame caption="Set how long the destination rings before the transfer counts as unanswered.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-ring-duration.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=b8a08c82f6ca4e5887ce948ae5d2dac6" alt="Transfer Call window with the Transfer Ring Duration group outlined in blue, its slider set to 30s, below Displayed Caller ID and above Custom SIP Headers." style={{ maxHeight: '560px' }} width="1460" height="480" data-path="images/transfer-call/transfer-call-ring-duration.png" />
    </Frame>
  </Step>

  <Step title="Add custom SIP headers (optional)">
    Add custom SIP headers for outbound calls. Retell forwards these headers to your SIP provider on the SIP INVITE, so you can use them for custom routing, tagging, or metadata. See [SIP headers](/build/telephony/sip-headers) for the full reference.

    <Warning>Custom SIP headers are preserved only when transferring directly to a SIP endpoint. They may be stripped when transferring to a PSTN number.</Warning>

    All header names must start with `X-`, or be `User-To-User` (case insensitive).

    <Frame style={{ marginTop: '1rem' }} caption="Add custom SIP headers forwarded on the SIP INVITE.">
      <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-sip-headers.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=cfb483fb4ed14f8239345a19f52ba12a" alt="Transfer Call window with the Custom SIP Headers group outlined in blue, holding one row with the header name X-Department and the value billing, a delete icon, and a + Add button below." style={{ maxHeight: '560px' }} width="1460" height="394" data-path="images/transfer-call/transfer-call-sip-headers.png" />
    </Frame>
  </Step>
</Steps>

## Cold transfer settings

Cold transfers have one setting of their own: the SIP method used to hand off the call. SIP (Session Initiation Protocol) is the signaling protocol that sets up and routes VoIP calls.

* **SIP INVITE**: The default. Establishes or updates the active call path, then bridges the transfer. You choose which caller ID the destination sees.
* **SIP REFER**: Asks the endpoint to start a separate call to the destination for the handoff. Use this only if your telephony provider supports SIP REFER; caller ID behavior then depends on the provider.

The **User's Number** caller ID option applies only when the method is **SIP INVITE**.

<Frame caption="Choose SIP INVITE or SIP REFER for the cold transfer handoff.">
  <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-sip-method.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=8aba579d08f712c3be955b7fdfad229f" alt="Transfer Call window scrolled to the SIP Transfer Method row, outlined in blue, with SIP INVITE selected next to SIP REFER, below the Warm Transfer and Agentic Warm Transfer options and above Displayed Caller ID." style={{ maxHeight: '560px' }} width="1460" height="528" data-path="images/transfer-call/transfer-call-sip-method.png" />
</Frame>

## Warm transfer settings

**During Transfer Call**

* **On-hold Music**: What the caller hears while on hold. Defaults to a standard ringtone.
* **Transfer Ring Duration**: How long the destination rings before the attempt counts as unanswered.
* **Navigate IVR**: Turn this on if the destination is an IVR system the agent has to navigate.

**During Agent Connection**

* **Internal queue or hold system**: Set this to **Yes** if the destination queues calls. The agent then waits until a real person starts speaking before it debriefs, within the **Wait Time for Agent Answer** you set. Defaults to 30 seconds.
* **Whisper Debrief Message (optional)**: A message spoken privately to the destination. The caller cannot hear it.

**After Transfer Connects**

* **Three-Way Ring Tone (optional)**: A short sound played when the call connects.
* **Three-Way Debrief Message (optional)**: A message spoken to both the destination and the caller once connected.

<Frame caption="Warm transfer settings, including the queue question and whisper message.">
  <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-warm-settings.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=8db2b4dec7531f7cae7d75f5935bcfc9" alt="Warm transfer Transfer Settings card with During Transfer Call fields for On-hold Music set to Ringtone, Transfer Ring Duration at 30s, and a Navigate IVR toggle; During Agent Connection asking whether there is an internal queue or hold system with Yes selected, a note that the Retell agent waits until a real person starts speaking before debriefing, Wait Time for Agent Answer at 30s, and a Whisper Debrief Message toggle; and After Transfer Connects with Three-Way Ring Tone and Three-Way Debrief Message." style={{ maxHeight: '560px' }} width="1045" height="1120" data-path="images/transfer-call/transfer-call-warm-settings.png" />
</Frame>

## Agentic warm transfer settings

**During Transfer Call**

* **On-hold Music**: What the caller hears while the transfer agent is working. Defaults to a standard ringtone.
* **Transfer Ring Duration**: How long the destination rings before the attempt counts as unanswered.

**During Agent Connection**

* **Two-way Conversation Agent**: The transfer agent that talks to the destination and decides whether to connect the caller. See [Transfer agents](#transfer-agents) below.
* **Wait time for agent answer**: How long to give the transfer agent to reach a decision (connect or cancel).
* **Action on Timeout**: What happens when that wait time runs out: **Cancel Transfer** (treat it as failed) or **Bridge the transfer** (connect the caller anyway).

**After Transfer Connects**

* **Three-Way Ring Tone (optional)**: A short sound played when the call connects.
* **Three-Way Message (optional)**: A short handoff message shared with both sides once connected. Write it as a **Prompt** (instructions the agent turns into a sentence) or a **Static Sentence** (the exact words to say).

<Frame caption="Agentic warm transfer settings, including the two-way conversation agent and timeout behavior.">
  <img src="https://mintcdn.com/retellai/h8x4O1GW5f8KQTjZ/images/transfer-call/transfer-call-agentic-settings.png?fit=max&auto=format&n=h8x4O1GW5f8KQTjZ&q=85&s=333560ea3b6a4235bb1ad88a30578bc7" alt="Agentic warm transfer Transfer Settings card with During Transfer Call fields for On-hold Music set to Ringtone and Transfer Ring Duration at 30s; During Agent Connection with an empty Two-way Conversation Agent selector reading Transfer screening agent, Wait time for agent answer at 30s, and Action on Timeout set to Cancel Transfer; and After Transfer Connects with Three-Way Ring Tone and Three-Way Message." style={{ maxHeight: '560px' }} width="1367" height="1120" data-path="images/transfer-call/transfer-call-agentic-settings.png" />
</Frame>

### Transfer agents

A **transfer agent** is a dedicated agent that screens the handoff. While your caller waits on hold, it calls the destination, talks to whoever picks up, and decides whether the transfer should happen at all.

To make that decision it gets two tools no other agent has:

* **Bridge Transfer**: the recipient is ready, so connect the caller.
* **Cancel Transfer**: the recipient isn't available, so send the caller back to your main agent, which picks the conversation back up.

It also has no **End Call**, **Call Transfer**, or **Agent Transfer**, so bridging, cancelling, or hitting the wait-time limit are the only ways its call ends. Write its prompt around that one decision: ask whether the recipient can take the call, then bridge or cancel.

Click the **Two-way Conversation Agent** selector to pick a transfer agent, or click **Create new** to build one as a **Single Prompt** or **Conversation Flow** agent.

<Frame caption="Search for a transfer agent, preview it, then confirm.">
  <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/transfer-agents/select-transfer-agent-preview.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=d24e40314911030208991cc266a45842" alt="Select transfer agent window with the search box, a transfer agent selected, and its preview shown on the right next to the Confirm selection button." width="2146" height="1418" data-path="images/transfer-agents/select-transfer-agent-preview.png" />
</Frame>

<Note>
  Transfer agents are kept in their own list, separate from your normal agents, and can't be used for inbound, outbound, or batch calls. Whether an agent is a transfer agent is fixed when you create it, so you can't convert an existing agent into one.
</Note>

The transfer agent is given the original conversation automatically, so you don't pass it anything. It knows who called and what they wanted, so it can explain the situation to whoever picks up. Dynamic variables carry over too, including anything you [extracted earlier in the call](/build/single-multi-prompt/extract-dv), plus `original_call_id` if you need to tie the two calls together.

What it doesn't get is anything said after the transfer starts, and your main agent never sees the screening conversation. The two calls are recorded and transcribed separately.

## FAQ

<AccordionGroup>
  <Accordion title="What's the difference between Transfer Call and Agent Transfer?">
    **Transfer Call** places a real phone call to another number (a human, a department, or an external line). **[Agent Transfer](/build/single-multi-prompt/transfer-agent)** hands the conversation to another *AI* agent within the same call: no new phone call, lower latency, and the destination agent inherits the full conversation history. Use Transfer Call to reach a person or outside number; use Agent Transfer to switch between AI agents.
  </Accordion>

  <Accordion title="Agentic warm transfer involves a second AI agent. Isn't that the same as Agent Transfer?">
    No. They solve different problems, and the second AI agent talks to a different person in each case.

    **[Agent Transfer](/build/single-multi-prompt/transfer-agent)** swaps which AI is talking to *your caller*. There's no new phone call: it's the same call, the same call ID, and the conversation history carries over. Use it to move between AI agents, for example from a general receptionist to a billing specialist.

    **Agentic warm transfer** places a real outbound call to a phone number. The transfer agent talks to *the person who answers at the destination*, never to your caller, who is on hold the whole time. Use it to screen a human before handing off to them.

    Once the transfer agent bridges, your caller and the destination are connected directly and every AI drops off the call. In Agent Transfer, an AI is still on the line.
  </Accordion>

  <Accordion title="When should I use warm transfer instead of cold transfer?">
    Use **cold transfer** when you just need to route the caller and drop off. Use **warm transfer** when the agent should wait for a real person to answer, brief the destination privately, or introduce the caller before connecting. Use **agentic warm transfer** when you want an AI transfer agent to screen or brief the destination first.
  </Accordion>

  <Accordion title="The transfer destination sees the wrong caller ID. What's wrong?">
    Showing the caller's number requires caller ID override support from your telephony provider. Retell Twilio numbers support it on both warm and cold transfer; Retell Telnyx numbers support it only via cold transfer (SIP REFER). If override isn't supported, the transfer fails. Switch **Displayed Caller ID** to **Retell Agent's Number** or use a supported number.
  </Accordion>
</AccordionGroup>
