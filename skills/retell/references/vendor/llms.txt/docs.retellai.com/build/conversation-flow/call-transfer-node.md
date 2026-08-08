> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Call transfer node in conversation flow

> Use a call transfer node to hand a Retell phone call off to a different number — supports Retell and imported numbers, with optional pre-transfer messages.

<Warning>
  This node only works during phone calls instead of web calls. It's available for Retell numbers and imported numbers.
</Warning>

Call transfer node is used to transfer the call to another number. The agent will not speak when it's in this node. If you want the agent to say things like `Let me transfer you right away` before performing the actual transfer, you can do so by putting a conversation node (with `skip response` turned on) before this node.

<Frame>
  <img src="https://mintcdn.com/retellai/CusS6gidcUzaxvEx/images/cf/transfer-call-node.png?fit=max&auto=format&n=CusS6gidcUzaxvEx&q=85&s=d00f0caa67dcf341e314c56091eba8d7" width="1156" height="1082" data-path="images/cf/transfer-call-node.png" />
</Frame>

## When Can Transition Happen

Transition happens when transfer fails. There's already a pre-populated edge for this, feel free to connect that to a node to handle transfer failure.

## Configure Transfer

<Steps>
  <Step title="Setup Transfer To Target">
    Set transfer number to be either:

    * a number in e.164 format, or a SIP URI in the format of `sip:username@domain` (e.g. `sip:user@retellai.com`).
    * [dynamic variable](/build/dynamic-variables) that gets substituted at runtime
    * (Optional) if your transfer destination is not in e.164 format then you can choose to keep the input as is by choosing raw format. This only applies when you are using custom telephony and does not apply when you are using Retell Telephony. This can be useful when you want to transfer to internal pseudo numbers.

    <Frame>
      <img src="https://mintcdn.com/retellai/CusS6gidcUzaxvEx/images/cf/transfer-call-e164.png?fit=max&auto=format&n=CusS6gidcUzaxvEx&q=85&s=ba54ab3635ca7cd934d213eb68414143" width="1182" height="1098" data-path="images/cf/transfer-call-e164.png" />
    </Frame>

    Set the transfer number extension if needed. Extension must be 0-9, '\*', '#' (E.g. 123#)
  </Step>

  <Step title="Configure Transfer Type">
    Choose between cold transfer, warm transfer, or agentic warm transfer:

    * **Cold transfer**: The call is transferred to a destination number and that's it.
    * **Warm transfer**: After the call is transferred to the destination number, the AI agent can attempt to detect if the other side is human, leave private messages that are not heard by the user, do a three-way introduction, etc. This is a direct warm transfer flow (not agentic warm transfer). (more details below).
    * **Agentic warm transfer**: A transfer agent has a two-way conversation with the transfer target and then decides to either bridge the original caller or cancel the transfer.
  </Step>

  <Step title="Configure Transfer Ring Duration (All Modes)">
    Use this slider to set how long the destination should ring for this transfer.

    * The value you set here applies only to this transfer.
    * If you do not set it, we use your agent-level ring duration setting.
    * This works for cold transfer, warm transfer, and agentic warm transfer.

    <Frame>
      <img src="https://mintcdn.com/retellai/XoiOOpBkax1duk4y/images/cf/transfer-dial-timeout.png?fit=max&auto=format&n=XoiOOpBkax1duk4y&q=85&s=91457ca85cf9ece8067d8ef983eb1214" width="816" height="118" data-path="images/cf/transfer-dial-timeout.png" />
    </Frame>
  </Step>

  <Step title="Configure Caller ID (Optional)">
    You can configure which caller id shows up to the transfer destination:

    1. **Retell Agent's number**: The transfer destination will see the Retell agent's number

    2. **User's Number**: The transfer destination will see the number of the user. Please note that the telephony provider must support caller id override for this feature to work.
       * For warm transfer, it's using SIP DIAL, and we are setting `from` and `P-Asserted-Identity` headers to the user's number.
       * For cold transfer, it's using SIP REFER, and it's up to the telephony provider to support caller id override for SIP REFER.
       * Retell Twilio numbers support showing user's number on both warm and cold transfer, Retell Telnyx numbers only support this when using SIP REFER via cold transfer.
       * If caller id override is not supported, the transfer would fail.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/_QrxQEoxP5f0zRlL/images/telephony/caller-id-setting.png?fit=max&auto=format&n=_QrxQEoxP5f0zRlL&q=85&s=6f73a184623714e31913189556b39f59" data-path="images/telephony/caller-id-setting.png" />
    </Frame>
  </Step>

  <Step title="Configure Cold Transfer Specific Settings">
    For cold transfer, you can configure the following settings:

    * **Cold transfer modes**: You can choose `SIP INVITE` or `SIP REFER`.
    * **What SIP is**: SIP (Session Initiation Protocol) is the signaling protocol used to set up and route VoIP calls.
    * **SIP INVITE**: This is the default transfer method. It establishes or updates the active call path, then bridges the transfer. You can choose which caller ID to use.
    * **SIP REFER**: This asks an endpoint to start a separate call to a third party for transfer handoff. Use this only if your telephony provider supports SIP REFER. Caller ID behavior depends on provider support and configuration.
    * **Caller ID behavior**: `show transferee as caller` only applies when cold transfer mode is `SIP INVITE`.

    <Frame>
      <img src="https://mintcdn.com/retellai/CusS6gidcUzaxvEx/images/cf/cold-transfer-settings.png?fit=max&auto=format&n=CusS6gidcUzaxvEx&q=85&s=c94a233055d022b53239d625249633d8" width="794" height="1092" data-path="images/cf/cold-transfer-settings.png" />
    </Frame>
  </Step>

  <Step title="Configure Warm Transfer Specific Settings (Non-Agentic)">
    For warm transfer (non-agentic), you can configure the following settings:

    * **On-hold music**: The audio played to the caller while they are on hold. The default is a standard ringtone.
    * **Navigate IVR**: Provide a prompt to help you navigate if the transfer target is an IVR system.
    * **Enable human detection**: When enabled, the agent will check if a human is present after the transfer target answers. The original caller will only be connected once a human is detected.
    * **Auto-greet**: If enabled, the agent will immediately say “Hello” when the transfer target picks up. This encourages a response, increasing the likelihood of detecting a human.
    * **Agent detection timeout**: The maximum amount of time the AI agent will wait to determine whether the transfer target is a human. The caller is connected only if human detection succeeds within this timeframe. Otherwise, the transfer is marked as failed. The default timeout is 30 seconds.
    * **Whisper message (optional)**: A message spoken privately to the transfer target before connecting them to the original caller.
    * **Three-way message (optional)**: A message spoken to both the transfer target and the original caller once the connection is established.

    <Frame>
      <img height="700" src="https://mintcdn.com/retellai/_QrxQEoxP5f0zRlL/images/telephony/warm-transfer-settings.png?fit=max&auto=format&n=_QrxQEoxP5f0zRlL&q=85&s=d813bc6a8f2fdb7c3dbb1ae8b8078c4b" data-path="images/telephony/warm-transfer-settings.png" />
    </Frame>
  </Step>

  <Step title="Configure Agentic Warm Transfer Specific Settings">
    In an agentic warm transfer, a second AI agent — called a **transfer agent** — answers the handoff, talks with the transfer target, and then decides whether to connect (bridge) your caller or cancel. For this transfer type you can configure:

    * **On-hold music**: What the original caller hears while the transfer agent is working.
    * **Two-way conversation agent**: The transfer agent (and version) that talks to the transfer target and decides whether to bridge or cancel. See **Choose or create a transfer agent** in the next step.
    * **Wait time for agent answer**: How long to give the transfer agent to make a decision.
    * **Action on timeout**: What happens if that wait time runs out — **Cancel transfer** (treat it as a failed transfer) or **Bridge the transfer** (connect the caller anyway).
    * **Three-way ring tone**: While the transfer agent is handling the handoff, the original caller hears the selected ring tone/on-hold audio until the call is bridged or canceled.
    * **Three-way message (optional)**: A message shared with both parties when the call is bridged. You can write it as a **Prompt** (instructions the agent turns into a sentence) or a **Static Sentence** (the exact words to say).

    <Frame>
      <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/transfer-agents/cf-agentic-warm-transfer-settings.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=f85540d891b3f229365b7bf0ea2fbf82" width="786" height="1294" data-path="images/transfer-agents/cf-agentic-warm-transfer-settings.png" />
    </Frame>
  </Step>

  <Step title="Choose or create a transfer agent">
    A **transfer agent** is a separate AI agent whose only job is to receive agentic warm transfers — think of it as an AI teammate who answers the handoff, speaks with the transfer target, and decides whether to connect your caller. Transfer agents are kept separate from your normal agents, so they won't clutter your main agent list.

    **1. Open the picker.** Click the **Two-way conversation agent** selector (it reads "Transfer screening agent" until one is chosen) to open the **Select transfer agent** window.

    <Frame>
      <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/transfer-agents/cf-agentic-warm-transfer-settings.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=f85540d891b3f229365b7bf0ea2fbf82" width="786" height="1294" data-path="images/transfer-agents/cf-agentic-warm-transfer-settings.png" />
    </Frame>

    **2. Pick an existing one.** Use the search box to find a transfer agent, click it to see a quick preview on the right, then click **Confirm selection**.

    <Frame>
      <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/transfer-agents/select-transfer-agent-preview.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=d24e40314911030208991cc266a45842" width="2146" height="1418" data-path="images/transfer-agents/select-transfer-agent-preview.png" />
    </Frame>

    **3. Or create a new one.** Click **Create new**, choose the type — **Single Prompt** (simplest: one set of instructions) or **Conversation Flow** (a step-by-step visual flow) — then click **Create**. The new transfer agent opens so you can set it up right away, either in a tab inside the builder or in a new browser tab depending on your agent type.

    <Frame>
      <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/transfer-agents/create-transfer-agent-type.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=018c18f9959e60f471e4ced16858e827" width="2150" height="1406" data-path="images/transfer-agents/create-transfer-agent-type.png" />
    </Frame>

    **4. Manage a transfer agent.** Hover a selected agent and open its **⋯** menu to **Rename** it, choose a specific **Version** to lock to (otherwise it always uses the latest), or **Delete** it. Deleting removes all versions and also removes it from any transfer that was using it.

    <Frame>
      <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/transfer-agents/transfer-agent-card-menu.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=315c32ce62eb96309d40312c2b163967" width="2150" height="1412" data-path="images/transfer-agents/transfer-agent-card-menu.png" />
    </Frame>
  </Step>

  <Step title="Add Custom SIP Headers (Optional)">
    Add custom SIP headers for outbound calls. Custom SIP headers (usually prefixed with `X-`) let you pass session-specific data, such as user IDs or campaign codes, between VoIP endpoints.
    These headers are forwarded to your SIP provider on SIP INVITE and are useful for custom routing and tagging.

    <Warning>Custom SIP headers are preserved only when transferring the call directly to a SIP endpoint. They may be stripped if you are transferring the call to a PSTN number.</Warning>

    <code>All header names must start with `X-` or must be `User-To-User` (case insensitive)</code>

    <Frame style={{ marginTop: '1rem' }}>
      <img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/custom-sip-headers.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=e598e83c6f9a6a6b018a340f36f97e8e" width="380" height="178" data-path="images/cf/custom-sip-headers.png" />
    </Frame>
  </Step>
</Steps>

## Rest of Node Settings

* **Talk While Waiting**: when enabled, a text input box will show up where you can write instructions for the agent to follow to generate an utterance like `Let me check that for you.` to say while the function is being executed. You can choose between `Prompt` and `Static Sentence`.
* **Global Node**: read more at [Global Node](/build/conversation-flow/global-node)
* **LLM**: choose a different model for this particular node. Will be used for function argument generation, and potentially talk while waiting message generation.
