> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Receive calls

> Bind voice agents to Retell or imported numbers to answer inbound calls, route SIP traffic, handle overflow, and pass per-call context.

Bind an agent to a [Retell-managed or imported number](/deploy/purchase-number) so it can answer inbound calls. If you route calls through your own carrier, see [custom telephony](/deploy/custom-telephony).

### Bind voice agents

* A number can receive and make calls only after you bind an agent to it.
* You can assign different inbound and outbound agents to the number.
* Leave an agent unset to disable that direction. For example, if you only make outbound calls and don't want callbacks, leave `inbound_agent_id` unset.

<Frame caption="Bind inbound and outbound agents to your number">
  <img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/deploy/phone-call/bind-agent.jpeg?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=e65cbf9714ec24c493ffc4fa914d8772" alt="Phone number configuration showing agent binding for inbound and outbound" width="504" height="296" data-path="images/deploy/phone-call/bind-agent.jpeg" />
</Frame>

After you bind an inbound agent, the number can receive calls.

### Handle inbound overflow

Inbound calls count toward your workspace concurrency limit. To keep inbound calls available during
high outbound volume, you can reserve part of your concurrency for inbound calls. See
[Reserved Inbound Concurrency](/deploy/concurrency#reserved-inbound-concurrency).

If all concurrency slots are in use, Retell briefly waits for a slot to open. If no slot opens after
about 40 seconds, Retell transfers the call to the phone number's `fallback_number` when one is
configured. Without a fallback number, the call ends with `concurrency_limit_reached`.

### A/B testing

See [A/B Testing](/deploy/ab-testing).

### Inbound call webhook

You may want to use different agents for inbound calls to the same number, or provide dynamic variables and other per-call fields.

Read more at [Inbound Call Webhook](/features/inbound-call-webhook).

### Route calls to Retell's SIP endpoint

If you're routing inbound calls from your own carrier or PBX, point your SIP trunk or dial plan at Retell's SIP server:

* **SIP server URI**: `sip:sip.retellai.com`
* **Transports**: TCP (recommended), UDP, TLS, mTLS. Append `;transport=tcp`, `;transport=udp`, or `;transport=tls` to the URI to select one.
* **Media encryption**: SRTP (requires TLS transport)
* **Audio codecs**: PCMU, PCMA, G.722 (HD)
* **IP blocks to whitelist**: `18.98.16.120/30`, `3.42.144.0/23`, `153.57.128.0/18`, `143.223.88.0/21` (certain US traffic), `161.115.160.0/19` (certain US traffic)

For the full setup — elastic SIP trunking, dial-to-SIP-URI, mTLS, and provider-specific guides (Twilio, Telnyx, Vonage, Avaya, Genesys, Five9, Amazon Connect) — see [custom telephony](/deploy/custom-telephony).

### Inbound custom SIP headers

* You can use [custom SIP headers](/build/telephony/sip-headers) to pre-set dynamic variables for a call.
* Retell extracts any header starting with `sip.h.x-`, along with common SIP headers like `Diversion`, `History-Info`, `User-To-User` and `P-Asserted-Identity`, and converts each into a dynamic variable by stripping the `sip.h.` prefix.
  * E.g. `sip.h.x-caller: abc` -> `x-caller: abc`
  * E.g. `sip.h.p-asserted-identity: +12345678910` -> `p-asserted-identity: +12345678910`
  * E.g. `sip.h.diversion: <sip:2000@192.168.254.254>;privacy=off;reason=no-answer;counter=1;screen=no` -> `diversion: <sip:2000@192.168.254.254>;privacy=off;reason=no-answer;counter=1;screen=no`

### Get call detail

* API: Use the [Get Call API](/api-references/get-call) to get information like transcript, recording, and latency tracking.
* Webhook: Set up webhooks to receive real-time updates when a call is initiated, ends, and is analyzed.
  Read more at [Call Webhook Guide](/features/webhook-overview#event-types).
