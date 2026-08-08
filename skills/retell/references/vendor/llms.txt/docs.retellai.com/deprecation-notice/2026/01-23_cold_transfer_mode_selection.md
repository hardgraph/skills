> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Explicit cold transfer mode selection (01/23/2026)

> On 01/23/2026 the `show_transferee_as_caller` field changes meaning — Retell switches to explicit cold transfer mode between SIP REFER and SIP INVITE.

## Cold Transfer Mode Selection

The behavior of the `show_transferee_as_caller` parameter in Cold Transfer options is changing. Previously, this parameter was used to toggle between SIP REFER and SIP INVITE transfer modes.

**Affected APIs:**

* [Create Retell LLM](/api-references/create-retell-llm)
* [Update Retell LLM](/api-references/update-retell-llm)
* [Create Conversation Flow](/api-references/create-conversation-flow)
* [Update Conversation Flow](/api-references/update-conversation-flow)
* [Create Conversation Flow Component](/api-references/create-conversation-flow-component)
* [Update Conversation Flow Component](/api-references/update-conversation-flow-component)

**What's changing:**

* The `show_transferee_as_caller` parameter will no longer control the transfer mode (SIP REFER vs SIP INVITE).
* Use the new `cold_transfer_mode` parameter to explicitly choose between `sip_refer` and `sip_invite`.
* The `show_transferee_as_caller` parameter will only control caller ID display and will only take effect when `cold_transfer_mode` is set to `sip_invite`.

**Migration:**

* Set `cold_transfer_mode` to `sip_refer` or `sip_invite` to choose the transfer method.
* Set `show_transferee_as_caller` to `true` only if you want to show the transferee as the caller when using `sip_invite` mode.

**Effective date:** After 01/23/2026, `show_transferee_as_caller` will no longer affect the transfer mode selection.
