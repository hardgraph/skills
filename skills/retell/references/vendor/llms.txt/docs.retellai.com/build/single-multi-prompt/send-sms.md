> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Send SMS

> Use the Send SMS tool so a Retell single or multi-prompt agent can text the caller or another number mid-call from SMS-enabled Retell or imported numbers.

This tool is used to send an SMS during a phone call. You can send to the caller's number or a different number.

<Note> This tool only works for phone numbers that have SMS enabled, or when using an SMS-approved Retell number. Read more about [enabling SMS](/deploy/enable-sms). </Note>

<Tip>Agents can also **receive SMS during an active call** and understand the content, including text, images, audio, and video. This works out of the box for Retell Twilio numbers, and for custom telephony numbers that passed A2P applications. Read more at [Receive SMS during call](/deploy/enable-sms#receive-sms-during-call).</Tip>

<img src="https://mintcdn.com/retellai/aS6kWND7Eh5Z5FzB/images/sms/send-sms.jpeg?fit=max&auto=format&n=aS6kWND7Eh5Z5FzB&q=85&s=71aa61a6bcfea415589a9c9d58c2e72c" width="432" height="453" data-path="images/sms/send-sms.jpeg" />

## Choose where to send from

<Frame caption="Send from options for in-call SMS (Single/Multi Prompt)">
  <img src="https://mintcdn.com/retellai/aS6kWND7Eh5Z5FzB/images/sms/sms-tool-retell-number.png?fit=max&auto=format&n=aS6kWND7Eh5Z5FzB&q=85&s=f8360b62c242b7e3614dcad4ef5966fd" alt="Send In-Call SMS tool configuration showing SMS-approved Retell number and agent's associated number options" width="50%" data-path="images/sms/sms-tool-retell-number.png" />
</Frame>

You can choose one of two options for the sending number:

* **SMS-approved Retell number**: Send from Retell's pool of numbers that are already approved for SMS. This bypasses the A2P application process entirely. The message content is a **preset template provided by Retell** — you cannot customize the text or use a prompt.
* **Agent's associated number**: Send from the phone number bound to the agent. This requires your number to have SMS enabled through the [A2P application](/deploy/enable-sms#enable-sms-capabilities).

## Configure SMS content

* **When sending from the agent's associated number**: You can write a prompt to let the agent infer the SMS content, or use static SMS content. Dynamic variables are supported for static SMS content.
* **When sending from an SMS-approved Retell number**: The message is a preset template provided by Retell. You cannot edit the content.

## Configure SMS destination

By default, the SMS is sent to the caller's number. You can also choose to send to a different number — either a static number or a dynamic variable (e.g. `{{customer_phone}}`).

## Talk while waiting

Enable this option to have the agent say a short phrase while the SMS is being sent. You can configure the message as a prompt or a static sentence.

## Speak after sending message

Enable this option to have the agent speak after the SMS has been sent. This is useful for confirming to the user that the message was delivered.
