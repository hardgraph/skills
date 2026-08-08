> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Inbound call & SMS webhook

> Use the Retell inbound webhook to dynamically pick the agent, set dynamic variables, reject calls, and pass per-call context for inbound phone and SMS.

You often want to use different agents under the same number, or provide caller-specific context for a call. For outbound calls and chats, you supply that call-specific information in the API request. For inbound calls or SMS, you don't initiate the request, so you need a way to be notified when one arrives and then process it.

The inbound webhook handles this. Once set up, you can override the agent ID, set dynamic variables, and set other per-call fields, then process the call or SMS. It's part of your [number configuration](/deploy/inbound-call) and works for numbers you've purchased or imported. It applies to inbound [phone calls](/deploy/inbound-call) and inbound [SMS](/deploy/enable-sms).

This webhook does not apply to [dial to sip calls](/deploy/custom-telephony#method-2-dial-to-sip-uri), since you provide call-specific information when you register the phone call.

## Where to set the inbound webhook URL

The inbound webhook is configured **per phone number** (not at the account or agent level, which are for [call event webhooks](/features/register-webhook)):

1. Open the Retell dashboard and go to the **Phone Numbers** page.
2. Click the number you want to configure.
3. In the number's settings, set the **Inbound Webhook URL** field to your endpoint.
4. Save the changes.

Retell will `POST` the payload described below to that URL whenever an inbound call or SMS arrives on that number.

## Use cases

* Filter and reject unwanted inbound calls / SMS
* Add context (dynamic variables, metadata) to inbound calls / SMS
* Override agent id / version / specific agent settings for inbound calls / SMS
* Pause the call / SMS to pick it up with some delay
* Internal system records of the inbound call / SMS

## Webhook spec

The webhook `POST`s the payload to your endpoint with a 10-second timeout. If no success status (2xx) is received within 10 seconds, the webhook is retried up to 3 times.

Verify the webhook using your Retell API key to confirm it comes from Retell AI. Read more at [Secure the webhook](/features/secure-webhook).

### Request payload

These fields might be provided in the payload depending on your configuration:

* `agent_id`: if the number has inbound agent id set, you will see it in payload
* `agent_version`: if the number has inbound agent version set, you will see it in payload
* `from_number`: this will always show up in payload, helps you identify the caller and process the call / SMS accordingly
* `to_number`: this will always show up in payload, helps you identify the receiver and process the call / SMS accordingly
* `custom_sip_headers` (inbound call only): an object containing custom SIP headers extracted from the inbound `INVITE`. Only headers whose name starts with `X-` (case-insensitive), plus the allowlisted headers `User-to-User`, `Diversion`, `History-Info`, and `P-Asserted-Identity`, are forwarded. Header names are emitted in lowercase. The object may be empty or omitted when no qualifying headers are present, so treat it as optional. Use this to pass per-call context from your SIP trunk into the webhook (for example, to programmatically manage sessions).

Note that the call / SMS is not connected, and a call / SMS object is not yet created (and if you decided not to take the call for example, the call object will not be created). Therefore you will not have a call / SMS object and call / SMS id inside the payload.

Here's a sample payload for inbound call:

<CodeGroup>
  ```json Inbound Call  theme={"dark"}
  {
    "event": "call_inbound",
    "event_timestamp": 1780012672105,
    "call_inbound": {
      "agent_id": "agent_12345",
      "agent_version": 1,
      "from_number": "+12137771234",
      "to_number": "+12137771235",
      "custom_sip_headers": {
        "x-my-header": "my-value",
        "user-to-user": "616263;encoding=hex"
      }
    }
  }
  ```

  ```json Inbound SMS  theme={"dark"}
  {
    "event": "chat_inbound",
    "chat_inbound": {
      "agent_id": "agent_12345",
      "agent_version": 1,
      "from_number": "+12137771234",
      "to_number": "+12137771235"
    }
  }
  ```
</CodeGroup>

### Response

We expect a JSON response with a successful status code (2xx) with fields grouped under `call_inbound` or `chat_inbound`. Here are the allowed fields (all of them are optional):

* `reject`: set to `true` to decline this inbound call / SMS. See [Reject an inbound call or SMS](#reject-an-inbound-call-or-sms) below.
* `override_agent_id`: if you want to override the agent id, you can set it here
* `override_agent_version`: if you want to override the agent version, you can set it here
* `dynamic_variables`: if you want to set dynamic variables for this inbound call, you can set it here
* `metadata`: if you want to set metadata for this inbound call, you can set it here
* `agent_override`: if you want to override the agent settings.

#### Agent override

You can also override per-call / per-chat agent behavior without modifying the saved agent by returning an `agent_override` object. The override applies only for this session.

Supported groups:

* `agent`: Partial Agent settings (voice agents). Useful fields include `voice_id`, `voice_model`, `fallback_voice_ids`, `voice_temperature`, `voice_speed`, `volume`, `language`, `pronunciation_dictionary`, `boosted_keywords`, `stt_mode`, `vocab_specialization`, `denoising_mode`, `responsiveness`, `interruption_sensitivity`, `enable_backchannel`, `backchannel_frequency`, `backchannel_words`, `end_call_after_silence_ms`, `max_call_duration_ms`, `begin_message_delay_ms`, `ring_duration_ms`, `reminder_trigger_ms`, `reminder_max_count`, `ambient_sound`, `ambient_sound_volume`, `allow_user_dtmf`, `user_dtmf_options`, `voicemail_option`, `webhook_url`, `webhook_timeout_ms`, `data_storage_setting`, `opt_in_signed_url`, `pii_config`, `post_call_analysis_data`, `post_call_analysis_model`.
* `retell_llm`: Partial Retell LLM settings. Supported keys include `model`, `s2s_model`, `model_temperature`, `knowledge_base_ids`, `kb_config`, `start_speaker`, `begin_after_user_silence_ms`, `begin_message`.
* `conversation_flow`: Partial Conversation Flow settings. Supported keys include `model_choice`, `model_temperature`, `knowledge_base_ids`, `kb_config`, `start_speaker`, `begin_after_user_silence_ms`, `begin_message`.

Notes:

* If both `override_agent_id`/`override_agent_version` and `agent_override` are provided, we first resolve the target agent by id/version, then apply `agent_override` on top for this call.
* Overrides must satisfy the same validation rules as agent creation (e.g. voice/language compatibility, value ranges). Invalid overrides may cause the call to be rejected.
* Overrides do not persist back to the saved agent.

Here's a sample response for inbound call, for inbound SMS, simply replace `call_inbound` with `chat_inbound`:

```json theme={"dark"}
{
  "call_inbound": {
    "override_agent_id": "agent_12345",
    "override_agent_version": 1,
    "agent_override": {
      "agent": {
        "voice_id": "11labs-Adrian",
        "voice_temperature": 0.6,
        "interruption_sensitivity": 0.8,
        "max_call_duration_ms": 1800000
      },
      "retell_llm": {
        "model": "gpt-4o-mini",
        "model_temperature": 0.2,
        "knowledge_base_ids": ["kb_abc123"],
        "start_speaker": "agent",
        "begin_message": "Hi {{customer_name}}, thanks for calling."
      }
    },
    "dynamic_variables": {
        "customer_name": "John Doe"
    },
    "metadata": {
        "random_id": "12345"
    }
  }
}
```

### Reject an inbound call or SMS

To decline an inbound call or SMS, return `reject: true` in the response. This works whether or not the number has an inbound agent set, so you can keep a default agent configured and reject only the calls you don't want.

```json theme={"dark"}
{
  "call_inbound": {
    "reject": true
  }
}
```

Behavior:

* Only the boolean `true` rejects. Any other value (including the strings `"true"` / `"false"`, `1`, or omitting the field) is ignored and the call / SMS proceeds as normal.
* Rejecting takes priority over agent selection — `override_agent_id` and `agent_override` are ignored when `reject` is `true`. We recommend returning only `{ "reject": true }` when declining: other fields such as `dynamic_variables` and `metadata` are still validated, and an invalid value there is treated as a bad response, in which case `reject` is not applied.
* For calls, Retell hangs up before any agent or voice setup. No call object is created.
* For SMS, Retell drops the message without replying. No chat object is created.

Use this to filter unwanted numbers, block traffic outside business hours, or turn away callers you can't serve.

## FAQ

<AccordionGroup>
  <Accordion title="What would happen to the inbound call when the webhook response is not received yet?">
    The call would continue to stay in ringing state.
  </Accordion>

  <Accordion title="What would happen to the inbound SMS when the webhook response is not received yet?">
    The SMS will not get a reply.
  </Accordion>

  <Accordion title="What would happen if webhook was not successful?">
    It would get retried up to 3 times. If all of those attempts fail, it will check whether this number has an inbound agent id set. If it does, it will then try to connect the call to that agent. If not, it will then disconnect the call.
  </Accordion>

  <Accordion title="Can I use this webhook to decline inbound calls / SMS based on incoming number?">
    Yes. Check `from_number` in the webhook request body, and respond with `reject: true` for the numbers you want to turn away. See [Reject an inbound call or SMS](#reject-an-inbound-call-or-sms). Unlike the older approach of omitting `override_agent_id`, `reject` works even when the number has an inbound agent set, so you can serve most callers with your default agent and decline only the ones you don't want.
  </Accordion>
</AccordionGroup>
