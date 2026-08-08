> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Set & parse custom SIP headers

> Parse inbound and set outbound custom SIP headers in Retell to pass metadata between systems — supported for inbound, outbound, and transfer scenarios.

Use custom SIP headers to pass metadata between your systems and Retell on a call — for example, forwarding an account ID or a caller reference from your [custom telephony](/deploy/custom-telephony) platform so the agent can use it during the conversation.

<Note>SIP headers are available for phone calls only. Retell receives SIP headers on inbound calls, and sets them on outbound calls and transfers.</Note>

Many SIP parsers and middleboxes assume 1024 bytes is a reasonable maximum for a header line, and may reject or truncate longer headers unless configured to allow more.

## Parse custom SIP headers for inbound calls

For inbound calls, custom SIP headers (those starting with `X-` or `x-`) are received and extracted automatically into `call.custom_sip_headers`. They are also added to your [dynamic variables](/build/dynamic-variables), where you access them by stripping the `X-` or `x-` prefix. No configuration is needed.

For example, if this is the SIP header received for an inbound call:

```json theme={"dark"}
{
  "X-test-header": "something",
  "X-another-header": "something else",
  "from": "1234567890",
  "to": "0987654321"
}
```

Then here's your `call.custom_sip_headers` field, and they will be added to your dynamic variables.

```json theme={"dark"}
{
  "test-header": "something",
  "another-header": "something else",
}
```

If you have already specified dynamic variables with the same name for this inbound call, it will override the value received from the SIP header.

## Set custom SIP headers for outbound calls

<img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/deploy/phone-call/outbound-call.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=a19d395e5d5d2005d6f3937c6aa35055" alt="Outbound call dialog with a custom SIP header key and value being added." width="1032" height="1378" data-path="images/deploy/phone-call/outbound-call.png" />

For outbound calls, you can add custom SIP headers as needed. Each must start with `X-`.

## Set custom SIP headers for call transfers

For call transfers, you can also add custom SIP headers as needed. Each must start with `X-`. You can use a dynamic variable for the header value to pass information extracted from the call to the receiving party.

<Warning>For cold transfer with transferee number, it will use SIP REFER to transfer the call. Different telephony providers may or may NOT honor the custom SIP headers in the REFER request. Twilio for example, does not honor the custom SIP headers in the REFER request, so the SIP headers you set in the transfer call tool will not work for cold transfer with transferee number when using Twilio.</Warning>

If you are using:

* Conversation flow agents, check out [call transfer node](/build/conversation-flow/call-transfer-node#configure-transfer) for more details.
* Single / multi agent, check out [call transfer function](/build/single-multi-prompt/transfer-call) for more details.
