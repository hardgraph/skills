> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# SMS Node

> Use an SMS node to have a Retell voice agent send a text mid-call — to the caller or a different number — using an SMS-approved Retell or imported number.

SMS node is used to send an SMS during a phone call. You can send to the caller's number or a different number.

<Note> This node only works for phone numbers that have SMS enabled, or when using an SMS-approved Retell number. Read more about [enabling SMS](/deploy/enable-sms). </Note>

The SMS will be sent when entering this node.

## Choose where to send from

You can choose one of two options for the sending number:

* **SMS-approved Retell number**: Send from Retell's pool of numbers that are already approved for SMS. This bypasses the A2P application process entirely. The message content is a **preset template provided by Retell** — you cannot customize the text or use a prompt.
* **Agent's associated number**: Send from the phone number bound to the agent. This requires your number to have SMS enabled through the [A2P application](/deploy/enable-sms#enable-sms-capabilities).

<Frame caption="In-Call SMS node on the conversation flow canvas">
  <img src="https://mintcdn.com/retellai/aS6kWND7Eh5Z5FzB/images/sms/sms-node-retell-number.png?fit=max&auto=format&n=aS6kWND7Eh5Z5FzB&q=85&s=e2535ce73c4fc48ef0b8ccb1817b71a7" alt="In-Call SMS node showing fixed text content and transition edges" width="50%" data-path="images/sms/sms-node-retell-number.png" />
</Frame>

<Tip>Agents can also **receive SMS during an active call** and understand the content, including text, images, audio, and video. This works out of the box for Retell Twilio numbers, and for custom telephony numbers that passed A2P applications. Read more at [Receive SMS during call](/deploy/enable-sms#receive-sms-during-call).</Tip>

## Configure SMS content

* **When sending from the agent's associated number**: You can write a prompt to let the agent infer the SMS content, or use static SMS content. Dynamic variables are supported for static SMS content.
* **When sending from an SMS-approved Retell number**: The message is a preset template provided by Retell. You cannot edit the content.

## Configure SMS destination

By default, the SMS is sent to the caller's number. You can also choose to send to a different number — either a static number or a dynamic variable (e.g. `{{customer_phone}}`).

## When Can Transition Happen

The node would transition to the next node once the SMS is successfully sent or fails to send. It should take less than 2 seconds to get that result. It will transition out of the node purely based on the SMS result.

## Node Settings

* **Global Node**: read more at [Global Node](/build/conversation-flow/global-node)
* **LLM**: choose a different model for this particular node. Will be used for function argument generation, and potentially speak during execution message generation.
