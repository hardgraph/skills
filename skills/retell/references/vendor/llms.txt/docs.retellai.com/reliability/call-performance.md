> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Debug call transfer failure

> Debug Retell call transfer failures — verify the `transfer_call` function is configured correctly for single, multi-prompt, and conversation flow agents.

When experiencing call transfer issues, follow these troubleshooting steps to identify and resolve common problems.

## Agent-Specific Troubleshooting

### For Single/Multi-Prompt Agents

If call transfer is not triggered in your single or multi-prompt agent:

<Steps>
  <Step title="Verify transfer_call function implementation">
    1. Check your agent configuration
    2. Confirm that you've added the transfer\_call function to your agent's function list
    3. Visit [Function Calling Guide](/build/single-multi-prompt/function-calling) for more details on implementing the function
  </Step>

  <Step title="Review transfer conditions">
    1. Update your agent's prompt to clearly define transfer conditions
    2. Ensure the transfer\_call function description is specific and unambiguous
    3. Test with sample scenarios to validate transfer triggers
  </Step>
</Steps>

For more detailed guidance on specific features, visit [Call Transfer Setup](/build/single-multi-prompt/transfer-call).

### For Conversation Flow Agents

If call transfer is not triggered in your conversation flow agent:

<Steps>
  <Step title="Check transfer node configuration">
    1. Verify you have a transfer node in your conversation flow
    2. Ensure the transfer node is properly connected to other nodes
    3. Check that transition conditions are correctly set up
  </Step>

  <Step title="Review transfer node settings">
    1. Confirm the transfer destination is correctly configured
    2. Verify the transfer conditions in the node are clear and specific
    3. Test the flow to ensure the transfer node is reachable
  </Step>
</Steps>

For more detailed guidance, visit [Conversation Flow Transfer Setup](/build/conversation-flow/call-transfer-node).

## General Troubleshooting

### Call Type Verification

<Note>Call transfer is only supported for phone calls, **not web calls**</Note>

### If Call Transfer is Triggered but Failed

* **Telephony issues**: Call transfer is similar to placing an outbound call, and failures occur for similar reasons as outbound call failures. The SIP connection log is available in the call logs and is useful for diagnosing the failure reason.
  Please refer to [Understand Reasons for Outbound Call Failure](/reliability/debug-outbound-call) for more details.
* **Could not detect human**: If human detection is enabled, the call may fail if a human is not successfully detected. Possible reasons include:
  * No human was actually present (e.g. it was an IVR or voicemail).
  * The other party spoke, but only after the detection timeout expired.
  * The speech was too similar to an IVR or voicemail and was not recognized as human.
