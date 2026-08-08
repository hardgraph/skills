> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Telnyx

> Connect Retell to a Telnyx account with elastic SIP trunking — create the trunk, configure FQDN routing, set up outbound auth, and import Telnyx numbers.

Connect a Telnyx number to Retell with [elastic SIP trunking](/deploy/custom-telephony), so your agents can make and receive calls on numbers you own in Telnyx. Create an FQDN trunk, configure routing and codecs, set up outbound auth, then import the number into Retell.

<Steps>
  <Step title="Create elastic SIP trunking">
    1. Create the trunk, select FQDN as the type, and give it a name.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/telnyx/create.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=a0b5c74d7b0f9d0a27d6a2fb903b232b" data-path="images/custom-telephony/telnyx/create.jpeg" />

    2. Add FQDN

    Add the FQDN of Retell's SIP server: `sip.retellai.com`. Select `SRV` as the
    DNS record type.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/telnyx/setting.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=5c21b0780b66885597c416d98dd8fe1e" data-path="images/custom-telephony/telnyx/setting.jpeg" />

    3. Set up outbound calls authentication

    Select credentials as the authentication method, and add the username and password.
    You will need to use this username and password when importing the number to Retell.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/telnyx/outbound-auth.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=87ba94312ebb879e7dc9b19ed27e5bf6" data-path="images/custom-telephony/telnyx/outbound-auth.jpeg" />

    <Note>
      Telnyx requires the header `X-Telnyx-Username: <username>` to be included in the outbound calls when using credentials as the authentication mechanism.
      See Telnyx [documentation](https://support.telnyx.com/en/articles/5271423-guide-to-sip-anchorsite-settings#h_0b59d6992a) for more details.
      You can find details on adding [custom SIP headers](/build/telephony/sip-headers), or on the [Make outbound calls](/deploy/outbound-call#step-2:-make-outbound-calls) page.
    </Note>

    4. Set up inbound setting

    * Select `+E.164` as the number format.
    * Select `G722`, `G711U`, `G711A` as the codecs.
    * Select `TCP` as the transport method. (TCP is recommended over UDP for reliability.)
    * Select your SIP region.

    <img height="200" src="https://mintcdn.com/retellai/TRHbNQLLwdXdtkVi/images/custom-telephony/telnyx/inbound-setting.jpeg?fit=max&auto=format&n=TRHbNQLLwdXdtkVi&q=85&s=202c89ac9ebd5fe08ae5e24a9698541c" data-path="images/custom-telephony/telnyx/inbound-setting.jpeg" />

    5. Set up outbound setting

    Create a new outbound voice profile

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/telnyx/voice-profile.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=eb72458506714003dab29aec2140cdbb" data-path="images/custom-telephony/telnyx/voice-profile.jpeg" />

    And select that in the outbound setting

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/telnyx/outbound-setting.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=7a33bb6784839e1e0dbb662c4064c5bd" data-path="images/custom-telephony/telnyx/outbound-setting.jpeg" />
  </Step>

  <Step title="Move numbers to elastic SIP trunking">
    You've created the elastic SIP trunk. Now purchase numbers or move
    existing numbers to this trunk.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/telnyx/assign-number.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=a7118a51ec9841b2bed9c3d62d5b6a86" data-path="images/custom-telephony/telnyx/assign-number.png" />
  </Step>

  <Step title="Import numbers to Retell">
    Now that the number is set up with your elastic SIP trunk, import it to Retell so we know how to route the call.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/import-number.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=19c4a219923eb0adc03c807cb8d767c3" data-path="images/custom-telephony/import-number.png" />

    Here you will supply Telnyx's FQDN as the termination SIP URI. You can find your FQDN based on
    your choice of SIP region in this [doc](https://sip.telnyx.com/) (e.g. sip.telnyx.com). You will also need to supply
    the username, password and any additional SIP headers you set up earlier in the outbound authentication as well.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/import-number-detail-telnyx.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=bdd4e9d406b74aa12a779c94c9d8697d" data-path="images/custom-telephony/import-number-detail-telnyx.png" />

    You can also import numbers programmatically via [Import Number API](/api-references/import-phone-number).

    Now that the number is imported, you can make and receive calls with it just like a number
    purchased from Retell. It shows up in your dashboard, and you can make phone calls from the
    dashboard directly. You can also use the [Create Phone Call API](/api-references/create-phone-call)
    to create calls programmatically.
    To have Retell stop using this number, delete it from the dashboard or via
    the [Delete Number API](/api-references/delete-phone-number).
  </Step>
</Steps>

## Troubleshooting

If an inbound call fails, check the FQDN and inbound settings on your Telnyx trunk. If an outbound call fails, check the termination FQDN, credentials, and the `X-Telnyx-Username` header you supplied to Retell. See [Debug outbound call](/reliability/debug-outbound-call) for common SIP failures, or [debug SIP calls with PCAP files](/reliability/debug-calls-pcap) to inspect the signaling in Wireshark.
