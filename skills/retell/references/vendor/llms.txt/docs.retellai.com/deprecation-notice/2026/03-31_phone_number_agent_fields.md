> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Phone number single-agent fields removed (03/31/2026)

> On 03/31/2026 Retell deprecates single-agent fields on phone numbers in favor of weighted inbound, outbound, and SMS agent lists across the phone number APIs.

## Phone number single-agent fields

The single-agent fields on phone number configuration are deprecated in favor of weighted agent lists for inbound/outbound calls and SMS.

**Affected APIs:**

* [Create Phone Number](/api-references/create-phone-number)
* [Import Phone Number](/api-references/import-phone-number)
* [Update Phone Number](/api-references/update-phone-number)
* [Get Phone Number](/api-references/get-phone-number)
* [List Phone Numbers](/api-references/list-phone-numbers)

**Deprecated fields:**

* `inbound_agent_id`, `inbound_agent_version`
* `outbound_agent_id`, `outbound_agent_version`
* `inbound_sms_agent_id`, `inbound_sms_agent_version`
* `outbound_sms_agent_id`, `outbound_sms_agent_version`

**Use instead:**

* `inbound_agents`
* `outbound_agents`
* `inbound_sms_agents`
* `outbound_sms_agents`

**Migration:**

* For a single agent, set the corresponding `*_agents` list to a single entry with `weight: 1`.
* For multiple agents, split weights so they sum to 1.
* For SMS, use `*_sms_agents` with the same weighting rules.

**Note:** Existing data is converted automatically and no action is required. Until the deprecation date, the APIs remain backwards-compatible as long as only a single agent is used for each of the deprecated fields.

**Example:**

* Before:
  ```json theme={"dark"}
  {
    "inbound_agent_id": "oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD",
    "inbound_agent_version": 3
  }
  ```
* After:
  ```json theme={"dark"}
  {
    "inbound_agents": [
      { "agent_id": "oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD", "agent_version": 3, "weight": 1 }
    ]
  }
  ```

**Effective date:** After 03/31/2026, the deprecated single-agent fields will no longer be supported.
