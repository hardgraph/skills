> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Vonage

> Connect Retell to a Vonage account using SIP trunking — configure termination and origination, set up credentials, and import Vonage numbers for calling.

Connect a Vonage number to Retell with [SIP trunking](/deploy/custom-telephony), so your agents can make and receive calls on numbers you own in Vonage. Set up termination (outbound) and origination (inbound), add credentials, then import the number into Retell.

<Steps>
  <Step title="Create elastic SIP trunking">
    1. Locate the SIP section in the Vonage dashboard, and select "Something else" for the provider type.

    <img height="200" src="https://mintcdn.com/retellai/S_iZ2qGP5QpXG6x_/images/custom-telephony/vonage/sip.jpeg?fit=max&auto=format&n=S_iZ2qGP5QpXG6x_&q=85&s=2a11ea60d2f68dae2e2271c2e6d78209" data-path="images/custom-telephony/vonage/sip.jpeg" />

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/vonage/choose-provider.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=91a84549630ea9efba5c877f5c945102" data-path="images/custom-telephony/vonage/choose-provider.jpeg" />

    From here, you can follow these instructions step by step.

    2. Set up termination (for outbound)

    * The termination SIP URI, username, and password here are important; you'll use them in later steps, so note them down.

    <img height="200" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/custom-telephony/vonage/termination.jpeg?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=ae07c26cab48ca1ddd1b0eb357695600" data-path="images/custom-telephony/vonage/termination.jpeg" />

    3. Set up origination (for inbound)

    * Here you will specify Retell's SIP server address as the origination SIP URI:
      `sip.retellai.com`.

    <img height="200" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/custom-telephony/vonage/origination.jpeg?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=2358642d1db6f27324c64a9a0ab67f58" data-path="images/custom-telephony/vonage/origination.jpeg" />
  </Step>

  <Step title="Move numbers to elastic SIP trunking">
    You've created the elastic SIP trunk. Now purchase numbers or link
    existing numbers to this trunk.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/vonage/add-number.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=00328c683a6c52c135d59b0ecf13c396" data-path="images/custom-telephony/vonage/add-number.jpeg" />
  </Step>

  <Step title="Import numbers to Retell">
    Now that the number is set up with your elastic SIP trunk, import it to Retell so we know how to route the call.

    Here you supply the termination SIP URI, username, and password you set up in Step 1.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/vonage/import.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=f1938fc65d930aa91376966506137078" data-path="images/custom-telephony/vonage/import.jpeg" />

    You can also import numbers programmatically via [Import Number API](/api-references/import-phone-number).

    Now that the number is imported, you can make and receive calls with it just like a number
    purchased from Retell. It shows up in your dashboard, and you can bind agents and make phone calls from the
    dashboard directly.

    Check out the [Make outbound calls](/deploy/outbound-call) guide to learn how to place calls, and [Receive inbound calls](/deploy/inbound-call) to bind an agent to your number.
  </Step>
</Steps>

## Troubleshooting

If an inbound call fails, check the origination SIP URI on your Vonage trunk. If an outbound call fails, check the termination SIP URI, username, and password you supplied to Retell. See [Debug outbound call](/reliability/debug-outbound-call) for common SIP failures, or [debug SIP calls with PCAP files](/reliability/debug-calls-pcap) to inspect the signaling in Wireshark.
