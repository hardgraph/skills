> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Update Call restricted to ended calls (08/31/2026)

> Update Call becomes ended-calls-only on 08/31/2026; override_dynamic_variables and mid-call data_storage_setting changes move to Update Live Call.

## Update Call is moving to ended calls only

The [Update Call](/api-references/update-call) API (`PATCH /v2/update-call/{call_id}`) is changing to operate on **ended calls only**. As part of this change, the `override_dynamic_variables` input on Update Call is deprecated.

Previously, Update Call could change settings mid-call — for example, updating `data_storage_setting` would take effect on the ongoing call. After this change, updates you make through Update Call (including `data_storage_setting`) take effect only on **ended calls** and no longer affect ongoing calls. To override dynamic variables or change settings like `data_storage_setting` on an ongoing call, use the new [Update Live Call](/api-references/update-live-call) API (`PATCH /v2/update-live-call/{call_id}`).

**Affected APIs:**

* [Update Call](/api-references/update-call)

**Deprecated field:**

* `override_dynamic_variables` (on Update Call)

**Use instead:**

* For ongoing (live) calls: [Update Live Call](/api-references/update-live-call) (`PATCH /v2/update-live-call/{call_id}`). Dynamic variable overrides go under `fields_to_override.override_dynamic_variables`; the same request can also override `metadata` and `data_storage_setting`, and control the live agent via `call_control` (`trigger_response`, `additional_context`).
* For ended calls: continue using [Update Call](/api-references/update-call) to update `metadata`, `data_storage_setting`, and `custom_attributes`.

**What's changing on 08/31/2026:**

1. Update Call will only accept requests for calls that have already ended. Requests targeting ongoing calls will be rejected.
2. Updates you make through Update Call — including `data_storage_setting` — will take effect only on ended calls, not on ongoing calls.
3. The `override_dynamic_variables` input on Update Call will no longer be accepted.
4. To change settings on an ongoing call (dynamic variables, `data_storage_setting`, etc.), use the Update Live Call API instead.

**Migration:**

* If you call Update Call on an ongoing call to change dynamic variables, switch to [Update Live Call](/api-references/update-live-call) and move the values under `fields_to_override.override_dynamic_variables`.
* If you call Update Call on an ongoing call to change `data_storage_setting`, switch to [Update Live Call](/api-references/update-live-call); through Update Call, `data_storage_setting` takes effect only after the call ends.
* If you call Update Call on an ended call, no change is needed for `metadata`, `data_storage_setting`, or `custom_attributes`. Remove `override_dynamic_variables` from those requests — it has no effect on an ended call.

<CodeGroup>
  ```json Before — PATCH /v2/update-call/{call_id} theme={"dark"}
  {
    "override_dynamic_variables": { "additional_discount": "15%" }
  }
  ```

  ```json After — PATCH /v2/update-live-call/{call_id} theme={"dark"}
  {
    "fields_to_override": {
      "override_dynamic_variables": { "additional_discount": "15%" }
    }
  }
  ```
</CodeGroup>

**Effective date:** After 08/31/2026, Update Call will only work for ended calls and will no longer accept `override_dynamic_variables`. Use the [Update Live Call](/api-references/update-live-call) API (`PATCH /v2/update-live-call/{call_id}`) to update ongoing calls.
