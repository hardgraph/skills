> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect Five9 to Retell

> Connect Five9 to Retell AI over the pre-built SIP trunk: import your Five9 number, add a ThirdPartyTransfer node, and route IVR calls to a voice agent.

<Note>
  Customers with an enterprise or paid support plan can contact us via the <a href="https://support.retellai.com/">Customer Support Portal</a> or [support@retellai.com](mailto:support@retellai.com) to receive dedicated assistance from Retell to connect your Five9 contact center solution.
</Note>

## Overview

Retell and Five9 are connected over a pre-built SIP trunk that already exists between the two platforms, so you don't build or configure a trunk yourself. You import your Five9 number into Retell, then point a Five9 IVR script at that number.

Once the connection is in place you can:

* Transfer inbound calls from a Five9 IVR script to your Retell agent.
* Place outbound calls from Retell using the same number.

**Example:** a healthcare provider keeps their existing Five9 IVR as the front door. During business hours the IVR routes callers to live agents; after hours the same script hands the call to a Retell agent that takes prescription refill requests and books callbacks.

Budget about 20 minutes of configuration, plus Five9 Support turnaround time for the trunk routing request (as of August 2026).

For the general model of how Retell connects to a third-party provider over SIP, see [custom telephony](/deploy/custom-telephony).

## Before you start

| Requirement             | Details                                                                               |
| ----------------------- | ------------------------------------------------------------------------------------- |
| Retell account          | With an [agent](/build/overview) already created.                                     |
| Five9 VCC Administrator | The desktop application (v13.x), with permission to create IVR scripts and campaigns. |
| Your phone number       | The number must already exist in your Five9 Numbers Inventory.                        |
| Trunk routing           | Five9 Support must route that number to Retell over the underlying pre-built trunk.   |

<Warning>
  Trunk routing is managed by Five9 Support, so raise a ticket with them before you begin. If this isn't done, calls fail at the transfer even though your IVR script looks correct.
</Warning>

### Who does what

You won't complete this setup alone. Plan the work with your Five9 representative before you start:

| Step                                     | Who runs it                                                                                                                   |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 1. Import your Five9 number into Retell  | You, in the Retell dashboard.                                                                                                 |
| 2–5. IVR script, transfer node, campaign | **You and your Five9 representative**, in the Five9 VCC Administrator. Retell staff have no access to your Five9 environment. |
| 6. Test the integration                  | **You, Five9, and Retell together.**                                                                                          |

<Note>
  Book time with your Five9 representative for steps 2 through 5, and line up Five9 and Retell for the step 6 test. Waiting until you're mid-setup to arrange it is the most common reason a Five9 rollout stalls.
</Note>

<Steps>
  <Step title="Import your Five9 number into Retell">
    1. In the Retell dashboard, go to **Phone Numbers** and choose **Connect to your own number**.
    2. Enter your number in E.164 format, for example `+19995550106`.
    3. In **Termination URI**, enter the Five9 SBC signaling IP for your data center, followed by the TLS port `5061`, for example `208.69.29.18:5061`. See [Five9 SBC signaling IPs](#five9-sbc-signaling-ips) below.
    4. Set **Outbound Transport** to `TLS`.
    5. Leave **SIP Trunk User Name** and **SIP Trunk Password** blank. The pre-built trunk doesn't use digest authentication, so credentials here cause the call to fail. Optionally add a **Nickname** to make the number easier to identify.
    6. Click **Save**, then assign your Retell agent to this number for inbound calls and, if required, outbound calls.

    <Frame caption="SIP trunk settings for an imported Five9 number in the Retell dashboard.">
      <img src="https://mintcdn.com/retellai/u27PZbC05UlJ3sJ0/images/deploy/five9/five9-retell-import-number.png?fit=max&auto=format&n=u27PZbC05UlJ3sJ0&q=85&s=e6404ab9f17ff4b2a47bcc2e50c19383" alt="Edit SIP Trunk Phone Number Settings modal with the Termination URI set to 208.69.29.18:5061 (1), Outbound Transport set to TLS (2), and the Save button (3). SIP Trunk User Name and Password are left empty." style={{ maxHeight: 560 }} width="616" height="661" data-path="images/deploy/five9/five9-retell-import-number.png" />
    </Frame>

    <Warning>
      Import the **pseudo number** provisioned by Five9, not the customer-facing DID associated with it. Importing the DID is the most common cause of a setup that looks correct but never connects.
    </Warning>

    <Note>
      No IP whitelisting is required on either side. The pre-built trunk between Retell and Five9 is already permitted at both ends.
    </Note>

    You can also import numbers programmatically via the [Import Number API](/api-references/import-phone-number).

    ### Five9 SBC signaling IPs

    Use the SBC for the Five9 data center your domain is hosted in.

    | Five9 data center   | Region         | Termination URI       |
    | ------------------- | -------------- | --------------------- |
    | Atlanta (ATL06)     | US East        | `208.69.29.18:5061`   |
    | Santa Clara (SCL06) | US West        | `162.213.153.61:5061` |
    | Montréal (MTL10)    | Canada         | `74.114.192.15:5061`  |
    | Montréal (MTL3)     | Canada         | `74.114.193.15:5061`  |
    | London (LND03)      | United Kingdom | `212.187.211.56:5061` |
    | Frankfurt (FRK)     | Germany        | `185.111.41.18:5061`  |
    | Amsterdam (AMS03)   | Netherlands    | `185.111.42.17:5061`  |
    | Tokyo (TOK)         | Japan          | `103.169.228.18:5061` |
    | Sydney (SYD)        | Australia      | `103.169.229.18:5061` |
    | São Paulo (SAO)     | Brazil         | `209.14.129.18:5061`  |

    <Note>
      Confirm the termination IP for your data center with Five9 Support before you rely on it, and especially if you can't see your region listed. Five9 provisions trunk connectivity per data center, so the correct address for your domain is the one Five9 confirms.
    </Note>
  </Step>

  <Step title="Create the IVR script in Five9 (with your Five9 rep)">
    **Steps 2 through 5 all run in the Five9 VCC Administrator, alongside your Five9 representative.** Retell staff can't access your Five9 environment.

    The IVR script is what tells Five9 to hand the call over to Retell.

    1. Open the Five9 **VCC Administrator** application.
    2. In the left-hand tree, select **IVR Scripts**.
    3. Click the **+** button in the toolbar to create a new script.
    4. Give the script a recognizable name, for example `RetellAITransfer`.

    <Frame caption="The IVR Scripts list in Five9 VCC Administrator.">
      <img src="https://mintcdn.com/retellai/u27PZbC05UlJ3sJ0/images/deploy/five9/five9-ivr-scripts-list.png?fit=max&auto=format&n=u27PZbC05UlJ3sJ0&q=85&s=a957d584f1a1bc55a0cbecf24087a897" alt="Five9 VCC Administrator window with IVR Scripts selected in the left-hand tree (1) and the toolbar's add button ringed (2). The list shows a script named RetellAITransfer alongside Sample_IVR." style={{ maxHeight: 560 }} width="1038" height="496" data-path="images/deploy/five9/five9-ivr-scripts-list.png" />
    </Frame>
  </Step>

  <Step title="Add the ThirdPartyTransfer node">
    Build your call flow to suit your business process. At the point where you want the call handed to your Retell agent, add a **ThirdPartyTransfer** node and connect it into the flow.

    <Frame caption="A ThirdPartyTransfer node placed at the end of an IVR call flow.">
      <img src="https://mintcdn.com/retellai/u27PZbC05UlJ3sJ0/images/deploy/five9/five9-ivr-thirdpartytransfer-node.png?fit=max&auto=format&n=u27PZbC05UlJ3sJ0&q=85&s=337b0314c267ac701cb11736e595e7e3" alt="Five9 IVR script editor showing a call flow of IncomingCall4 to Play4 to a ThirdPartyTransfer node, with the ThirdPartyTransfer node ringed (1)." style={{ maxHeight: 560 }} width="642" height="390" data-path="images/deploy/five9/five9-ivr-thirdpartytransfer-node.png" />
    </Frame>

    Then configure the transfer:

    1. Double-click the **ThirdPartyTransfer** node to open its properties.
    2. Leave **Third Party Number** set to **Constant**.
    3. Enter the number you imported into Retell, in E.164 format, for example `+19995550106`.
    4. Click **Save**, then save the IVR script.

    <Frame caption="3rd Party Transfer Module properties, set to transfer to the Retell number.">
      <img src="https://mintcdn.com/retellai/u27PZbC05UlJ3sJ0/images/deploy/five9/five9-thirdpartytransfer-properties.png?fit=max&auto=format&n=u27PZbC05UlJ3sJ0&q=85&s=77546d0f0a9f09a91b4a31e1bbefd581" alt="3rd Party Transfer Module Properties dialog with Third Party Number set to Constant (1), the value +19995550106 entered in E.164 format (2), and the Save button (3). Return After 3rd Party Call is unchecked." style={{ maxHeight: 560 }} width="1225" height="707" data-path="images/deploy/five9/five9-thirdpartytransfer-properties.png" />
    </Frame>

    <Note>
      The transfer is a blind, single-step transfer: Five9 hands the call to Retell and drops out of the conversation. Leave **Return After 3rd Party Call** unchecked unless you want the caller returned to the IVR after the Retell agent finishes.
    </Note>
  </Step>

  <Step title="Create an inbound campaign">
    The campaign is what connects an inbound number to your IVR script.

    1. In the left-hand tree, select **Campaigns**.
    2. Click the **+** button in the toolbar to create a new campaign.
    3. Set the campaign type to **Inbound** and give it a recognizable name, for example `RetellAIAgentCampaign`.

    <Frame caption="The Campaigns list in Five9 VCC Administrator, with an inbound campaign.">
      <img src="https://mintcdn.com/retellai/u27PZbC05UlJ3sJ0/images/deploy/five9/five9-campaigns-list.png?fit=max&auto=format&n=u27PZbC05UlJ3sJ0&q=85&s=e16562f44cc436b03894723c5c81956d" alt="Five9 VCC Administrator window with Campaigns selected in the left-hand tree (1) and the toolbar's add button ringed (2). The list shows RetellAIAgentCampaign with type Inbound." style={{ maxHeight: 560 }} width="1038" height="478" data-path="images/deploy/five9/five9-campaigns-list.png" />
    </Frame>
  </Step>

  <Step title="Attach the IVR script to the campaign">
    1. Double-click your campaign to open its **Properties** window.
    2. Go to the **IVR** tab and click **Add** to create a new schedule rule.
    3. Give the rule a name, then open the **IVR Script** dropdown.
    4. Select the script you created in step 2, for example `RetellAITransfer`.
    5. Set the schedule. Choose **All day long** with your required days of the week, or a specific date range or time interval.
    6. Under **Five9 VCC Channels**, tick **Voice**.
    7. Click **OK**, then save the campaign.

    <Frame caption="The campaign's IVR schedule rule, pointing at the Retell transfer script.">
      <img src="https://mintcdn.com/retellai/u27PZbC05UlJ3sJ0/images/deploy/five9/five9-campaign-ivr-schedule.png?fit=max&auto=format&n=u27PZbC05UlJ3sJ0&q=85&s=111f7ed2b301dd0fab046e1019ef13df" alt="Schedule rule dialog inside campaign properties: the IVR Script dropdown open (1) with RetellAITransfer selected (2), schedule options set to All day long (3), the Voice channel ticked (4), and the OK button (5)." style={{ maxHeight: 560 }} width="1125" height="783" data-path="images/deploy/five9/five9-campaign-ivr-schedule.png" />
    </Frame>
  </Step>

  <Step title="Test the integration (with Five9 and Retell)">
    **This step is run jointly by you, Five9, and Retell.** All three verify the Five9 campaign and the Retell agent's configuration and performance together before you go live.

    1. Call the number assigned to your campaign.
    2. Confirm the call transfers and that your Retell agent answers and holds a conversation.
    3. Place an outbound call from Retell using the imported number and confirm it connects.
    4. Cross-check the Retell call log against the Five9 call report to confirm both platforms recorded the same call.
  </Step>
</Steps>

## Troubleshooting

| Symptom                                            | Likely cause and fix                                                                                                                                       |
| -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The call reaches the IVR but drops at the transfer | The number isn't in your Five9 Numbers Inventory, or trunk routing hasn't been configured for it. Contact Five9 Support.                                   |
| Calls don't connect at all                         | Check the **Termination URI** in Retell. It must be the correct SBC IP for your data center followed by `:5061`, with **Outbound Transport** set to `TLS`. |
| The transfer connects but the agent never answers  | You imported the customer-facing DID instead of the Five9 pseudo number. Re-import using the pseudo number.                                                |
| DTMF digits aren't detected                        | DTMF must be RFC 2833 with payload type `101`. See [debug SIP calls using PCAP files](/reliability/debug-calls-pcap) to confirm what's negotiated.         |
| Transfer fails with an invalid number error        | Both Retell and the ThirdPartyTransfer node need the number in E.164 format, including the `+` and country code.                                           |
| The IVR script never runs                          | The campaign schedule rule is inactive or outside its configured hours, or the **Voice** channel isn't enabled on the rule.                                |

For Retell-side diagnostics, see [debug outbound connection issues](/reliability/debug-outbound-call) for `not_connected` failures and disconnection reasons.

## FAQ

<AccordionGroup>
  <Accordion title="Do I need to build a SIP trunk between Five9 and Retell?">
    No. The trunk between the two platforms already exists. You only need Five9 Support to route your number over it, then import that number into Retell.
  </Accordion>

  <Accordion title="Do I import the DID or the pseudo number?">
    The **pseudo number** provisioned by Five9. Don't use the customer-facing DID associated with it. Importing the DID produces a setup that looks correct but fails at the transfer.
  </Accordion>

  <Accordion title="Do I need to whitelist any IP addresses?">
    No. The pre-built trunk is already permitted at both ends, so no firewall or IP allowlist changes are needed on either side.
  </Accordion>

  <Accordion title="Why should SIP Trunk User Name and Password stay blank?">
    The pre-built trunk doesn't use digest authentication. Entering credentials makes Retell attempt an authenticated registration the trunk doesn't expect, and the call fails.
  </Accordion>

  <Accordion title="Can the Retell agent transfer the call back to a human Five9 agent?">
    Yes. Add a transfer to your Retell agent that hands the call to a number or SIP destination routing back into Five9. See [call transfer node](/build/conversation-flow/call-transfer-node) for conversation flow agents, or [transfer call](/build/single-multi-prompt/transfer-call) for single and multi-prompt agents. To carry context such as a caller or account ID across the handoff, use [custom SIP headers](/build/telephony/sip-headers).
  </Accordion>

  <Accordion title="How many simultaneous calls can I run through this trunk?">
    Retell's limit is set by your plan's [concurrency](/deploy/concurrency), and Five9 applies its own capacity limits to the trunk. Size both before a high-volume launch.
  </Accordion>
</AccordionGroup>

## Related

* [Custom telephony](/deploy/custom-telephony) — how Retell's SIP integration works in general.
* [Receive inbound calls](/deploy/inbound-call) and [make outbound calls](/deploy/outbound-call) — using your imported number.
* [Custom SIP headers](/build/telephony/sip-headers) — pass metadata between Five9 and your agent.
* [Concurrency](/deploy/concurrency) — plan call capacity before launch.
