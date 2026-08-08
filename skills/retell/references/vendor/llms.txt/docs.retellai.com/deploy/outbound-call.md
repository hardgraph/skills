> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Make outbound calls

> Make outbound phone calls with Retell agents — bind agents to your numbers, prepare API credentials, and trigger calls programmatically or from the dashboard.

## Overview

Make outbound calls with your Retell agents: bind an agent to a phone number, then trigger calls from the dashboard or the API.

## Prerequisites

* **Phone Number**: A [Retell-managed or imported number](/deploy/purchase-number)
* **Agent**: A configured agent ready for outbound calls
* **API Key**: Your Retell API key for authentication

## Step 1: Bind agents to phone numbers

Before making calls, assign agents to your phone numbers. This configuration determines how your number handles both inbound and outbound calls.

### Configuration options

| Setting            | Purpose                                 | Use case                      |
| ------------------ | --------------------------------------- | ----------------------------- |
| **Inbound Agent**  | Handles incoming calls to this number   | Customer support, callbacks   |
| **Outbound Agent** | Used when making calls from this number | Sales outreach, notifications |

### Flexible agent assignment

* **Different agents**: Use specialized agents for inbound vs outbound
* **Outbound only**: Leave inbound agent unset to prevent callbacks
* **Inbound only**: Configure only inbound agent for receive-only numbers

<Frame>
  <img height="400" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/deploy/phone-call/bind-agent.jpeg?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=e65cbf9714ec24c493ffc4fa914d8772" alt="Phone number configuration showing inbound and outbound agent assignment" data-path="images/deploy/phone-call/bind-agent.jpeg" />
</Frame>

<Tip>
  After binding an inbound agent, your number is immediately ready to receive calls. Test it by calling the number!
</Tip>

### A/B testing

See [A/B Testing](/deploy/ab-testing).

## Step 2: Make outbound calls

### International calling restrictions

<Warning>
  **Retell-purchased numbers**: Retell supports calling to [15 countries](/deploy/international-call).

  **Imported numbers**: International calling depends on your telephony provider's settings.
</Warning>

### Call parameters

When making outbound calls (v2), these parameters are supported:

| Parameter                      | Type           | Example                         | Description                                                                                 |
| ------------------------------ | -------------- | ------------------------------- | ------------------------------------------------------------------------------------------- |
| `from_number`                  | string (E.164) | `+14157774444`                  | Your Retell-managed or imported number.                                                     |
| `to_number`                    | string (E.164) | `+12137774445`                  | Destination number.                                                                         |
| `override_agent_id`            | string         | `agent_abc123`                  | Override the agent used for this call (optional).                                           |
| `override_agent_version`       | integer        | `1`                             | Version of the override agent; defaults to latest if omitted.                               |
| `agent_override`               | object         | See below                       | Per-call partial overrides for agent/response engine (optional).                            |
| `metadata`                     | object         | `{ "customer_id": "cust_123" }` | Free-form metadata stored with the call (size limit applies).                               |
| `retell_llm_dynamic_variables` | object         | `{ "name": "John" }`            | Key–value strings injected into prompts/tools (optional).                                   |
| `custom_sip_headers`           | object         | `{ "X-Call-ID": "123" }`        | Outbound [SIP headers](/build/telephony/sip-headers) forwarded to your provider (optional). |
| `ignore_e164_validation`       | boolean        | `false`                         | Only for custom telephony. Bypass E.164 validation for special routing.                     |

Agent overrides let you adjust per-call behavior without changing the saved agent. See “Agent Overrides” in the [Create Phone Call API](/api-references/create-phone-call) for supported fields and examples.

<Frame>
  <img src="https://mintcdn.com/retellai/60-FWXALU9phBb-D/images/quick-start-outbound-call-e164.png?fit=max&auto=format&n=60-FWXALU9phBb-D&q=85&s=b2d5b15364dd7b530f13751ca54e2164" width="862" height="916" data-path="images/quick-start-outbound-call-e164.png" />
</Frame>

### API implementation

For complete parameter documentation, see [Create Phone Call API Reference](/api-references/create-phone-call).

<CodeGroup>
  ```typescript Node theme={"dark"}
  const registerCallResponse = await retell.call.createPhoneCall({
    from_number: '+14157774444', // replace with the number you purchased
    to_number: '+12137774445',  // replace with the number you want to call
    // Optional: per-call agent selection and overrides
    override_agent_id: 'agent_abc123',
    override_agent_version: 0, // or omit to use latest
    agent_override: {
      agent: {
        voice_speed: 1.1,
        enable_backchannel: true,
      },
      // retell_llm or conversation_flow overrides are also supported
    },

    retell_llm_dynamic_variables: { // dynamic variables (optional, string values only)
      name: 'John Doe',
      blood_group: 'B+'
    },  
    custom_sip_headers: { // replace with custom sip headers you want to send (optional)
      'X-Custom-Header': 'Custom Value'
    }
  });
  console.log(registerCallResponse);
  ```

  ```python Python theme={"dark"}
  # Initiate an outbound call using the newly created agent
  call = client.call.create_phone_call(
      from_number="+14157774444", # replace with the number you purchased
      to_number="+12137774445",  # replace with the number you want to call
      # Optional: per-call agent selection and overrides
      override_agent_id="agent_abc123",
      override_agent_version=0,  # or omit to use latest
      agent_override={
        "agent": {
          "voice_speed": 1.1,
          "enable_backchannel": True,
        }
      },

      retell_llm_dynamic_variables={ # dynamic variables (optional, string values only)
        "name": "John Doe",
        "blood_group": "B+"
      },
      custom_sip_headers={ # replace with custom sip headers you want to send (optional)
        "X-Custom-Header": "Custom Value"
      }
  )
  print(call)
  ```
</CodeGroup>

## Step 3: Configure CPS (calls per second)

### Understanding CPS limits

CPS (calls per second) controls how many outbound calls you can start each second.

### Default limits and scaling

| Provider             | Default CPS | Maximum CPS | Notes                                                                        |
| -------------------- | ----------- | ----------- | ---------------------------------------------------------------------------- |
| **Twilio**           | 1           | 5           | Changes take up to 10 minutes                                                |
| **Telnyx**           | 1           | 16          | Instant updates                                                              |
| **Custom Telephony** | 1           | 150         | Check your custom provider's SIP trunk capacity before increasing this limit |

### Important considerations

1. **Throttling protection**: Exceeding limits results in rejected calls
2. **Gradual scaling**: Start low and increase based on actual needs
3. **Provider limits**: Your telephony provider may have additional restrictions
4. **Cost impact**: Higher CPS may increase telephony costs
5. **Concurrency**: CPS controls how fast calls start; the number that can run at once is capped separately by your [concurrency limit](/deploy/concurrency)

<div style={{ margin: 'auto' }}>
  <Frame>
    <img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/deploy/phone-call/config-cps.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=1d5b137cf76503120a8f2f405f7b1f73" width="2332" height="1468" data-path="images/deploy/phone-call/config-cps.png" />
  </Frame>
</div>

### Best practices for high-volume calling

<Tip>
  **Implement retry logic** with exponential backoff to handle throttling gracefully:

  ```javascript theme={"dark"}
  // Example retry logic
  const maxRetries = 3;
  let retryDelay = 1000; // Start with 1 second

  for (let i = 0; i < maxRetries; i++) {
    try {
      await makeCall();
      break;
    } catch (error) {
      if (error.code === 'rate_limited') {
        await sleep(retryDelay);
        retryDelay *= 2; // Exponential backoff
      }
    }
  }
  ```
</Tip>

## Step 4: Monitor call details

### Available monitoring methods

#### 1. API polling

Use [Get Call API](/api-references/get-call) to retrieve:

* Full transcript
* Call recording
* Latency metrics
* Function call logs
* Call duration and status

#### 2. Real-time webhooks

Set up [webhooks](/features/webhook-overview#event-types) to receive instant notifications for:

* **Call Started**: When the call connects
* **Call Ended**: Final status and duration
* **Call Analyzed**: Transcript and analysis ready
* **Call Failed**: Error details and reasons

<Note>
  Webhooks provide real-time updates without polling, making them ideal for production systems.
</Note>

<b>Triggering outbound calls (using Make.com)</b>

<iframe width="360" height="200" src="https://www.youtube.com/embed/CdbHLDDmi1A?si=Ym4b580riW4Q5l1k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

### Additional resources

* [Community Templates](https://docs.google.com/document/d/1hx6hdTEjAR4y4xXZ7RLMH2byQNVW1ABxC8S4FwvTx_Y/edit?tab=t.0#heading=h.wf5bktkelope): Real-world outbound calling examples
* [Batch calling](/deploy/make-batch-call): For high-volume campaigns
* [Handle voicemail and IVR](/build/handle-voicemail): Detect voicemail or IVR menus on outbound calls
* [Capture DTMF input](/build/user-dtmf): Collect keypad digits from the caller
* [Debug outbound calls](/reliability/debug-outbound-call): Troubleshoot connection issues
* [Webhook setup](/features/webhook-overview): Configure real-time notifications
