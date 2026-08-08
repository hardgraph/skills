> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Stop caller ID service

> Stop active Retell caller-ID services — branded calls and verified phone numbers — without interrupting your underlying telephony or live agent calls.

You can stop an active caller ID service at any time. These services — [branded calls](/build/telephony/branded-call) and [verified phone numbers](/build/telephony/verified-phone) — are add-ons layered on top of your underlying telephony. **Stopping them does not interrupt calls.** Your agents will continue making and receiving calls; only the associated caller ID enhancement or number verification will be removed.

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/delete_service.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=1ab297f0299ad72e27b8966f4530ea4d" alt="Delete service" data-path="images/delete_service.png" />
</Frame>

## What gets removed

* **Branded Calls** — your branded caller ID will no longer appear on the recipient's screen. Calls will still connect, but will present as a standard unbranded call.
* **Verified Phone Numbers** — your number's spam remediation registration is removed. The number may begin appearing as "Spam Risk" or "Scam Likely" on recipient devices until re-registered.

## Billing

### Branded Calls

Billing continues until the end of your current billing cycle. You will retain branded caller ID display for the remainder of the paid period even after initiating the stop.

### Verified Phone Numbers

Billing stops immediately upon service deletion. You will only be charged for the portion of the billing cycle the service was active.

<Note>
  If you plan to [re-register a verified phone number](/build/telephony/verified-phone) in the future, be aware that the re-registration process can take time. Your number may be flagged as spam in the interim.
</Note>
