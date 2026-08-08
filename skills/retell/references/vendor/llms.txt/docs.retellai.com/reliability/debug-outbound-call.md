> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Debug outbound connection issues

> Diagnose `not_connected` outbound Retell calls — work through `invalid_destination`, `no_valid_payment`, carrier blocks, and other disconnection reasons.

When an [outbound call](/deploy/outbound-call) has a status of `not_connected`, the call never established a connection to the destination number. The `disconnection_reason` field tells you why. This page covers the `not_connected` reasons; for the full list of reasons across all call statuses, see [debug call disconnection](/reliability/debug-call-disconnect).

## Disconnection reasons for not connected calls

* `invalid_destination`: the destination phone number is invalid. It may contain spaces or invalid characters, or your telephony provider requires a specific format (such as E.164).
* `telephony_provider_permission_denied`: the SIP trunk authentication failed.
* `telephony_provider_unavailable`: the telephony provider is unavailable or returning errors.
* `sip_routing_error`: there are loops or other issues in the SIP routing.
* `marked_as_spam`: the call was marked as spam. See the root cause and remediation below.
* `user_declined`: the user explicitly declined the call.
* `dial_failed`: no SIP error code is available, or the error is unknown.
* `dial_busy`: the number dialed is busy.
* `dial_no_answer`: the number dialed did not answer.

## Steps to troubleshoot

<Steps>
  <Step>
    Check the call history and detailed log. It contains the disconnection reason, the error message and optionally a **[SIP error code](/reliability/debug-calls-pcap#common-sip-response-code-reference)**. If the logs contain a SIP error then you can see the details by checking the PCAP file available in the call logs — see [Debug calls with PCAP](/reliability/debug-calls-pcap) for information on how to debug using PCAP files.
    In most cases, this would be enough for you to identify the root cause.

    <img src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/reliability/images/pcap_download.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=9419cf630d085b11a262ef73d1f293c7" alt="Call log detail with the PCAP file download link for a not_connected call." width="608" height="692" data-path="reliability/images/pcap_download.png" />

    <Note>
      PCAP files are only available when the agent's data retention is set to **Everything** and the SIP transport is **UDP/TCP**. Calls using TLS transport or any other data retention setting will not have a PCAP available.
    </Note>
  </Step>

  <Step>
    If the call detailed log and PCAP file do not provide enough information, you can try the following:

    * If using custom telephony
      1. Double check your configuration and make sure you imported the right information. Refer to [FAQ](/deploy/custom-telephony#faq) for more info.
      2. If the configuration is not correct, delete the imported number and re-import.
      3. If that does not solve it, please check with your telephony provider to see what's the error on their side.

    * If using numbers purchased from Retell
      1. Make sure the destination number can accept the call. Currently, numbers purchased from Retell can only make calls to US numbers.
  </Step>
</Steps>

### Number marked as spam

When a number experiences a high outbound call volume spike without any warmup, and/or has a low pickup rate, it might be marked as spam by carriers. When this happens, the number will get blocked frequently. To remedy this, you can try to:

* Purchase a new number, and warm it up before pouring all traffic to it (slowly add outbound traffic to it)
* Increase the pickup rate, read more at [Increase Pickup Rate](/build/telephony/call_efficiency_overview)
* Register the number with our spam remediation feature, read more at [verified phone number](/build/telephony/verified-phone)
