> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Twilio

> Connect Retell to your Twilio account using elastic SIP trunking — set up termination, origination, credentials, and import Twilio numbers into Retell.

Connect a Twilio number to Retell with [elastic SIP trunking](/deploy/custom-telephony), so your agents can make and receive calls on numbers you own in Twilio. Set up termination (outbound) and origination (inbound), add credentials, then import the number into Retell.

## Steps

<Steps>
  <Step title="Create elastic SIP trunking">
    1. Create the trunk, give it a name, and toggle some general settings

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/general.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=45e4528d4346100ac95626af7b68ecd1" data-path="images/custom-telephony/twilio/general.jpeg" />

    2. Set up termination (this is for outbound)

    * The termination SIP URI here is important; you'll use it in later steps. Use a localized termination URI near your region. You can expand and view your localized URIs in the Twilio console.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/termination.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=914580b920340ea68cc0538f86b4634f" data-path="images/custom-telephony/twilio/termination.jpeg" />

    * For your elastic SIP trunk to accept our outbound request, you need to whitelist
      an IP address or create an auth with a username and password.
      * If you opt for the auth route, you need to specify the username and password
        in the next step when importing the number to Retell.
          <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/auth.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=b80e3f28082c1994258c80e484a32008" data-path="images/custom-telephony/twilio/auth.jpeg" />
      * You need to whitelist Retell SIP SBC CIDR block 18.98.16.120/30 like the following:
          <img height="200" src="https://mintcdn.com/retellai/7L57j1f5OEkhUYOX/images/custom-telephony/twilio/acl.jpeg?fit=max&auto=format&n=7L57j1f5OEkhUYOX&q=85&s=c2c2fb7e31abc2dd483a233aa639a2ce" data-path="images/custom-telephony/twilio/acl.jpeg" />

    3. Set up origination (this is for inbound)

    * Here you will specify Retell's SIP server address as the origination SIP URI:
      `sip:sip.retellai.com`.

    <img height="200" src="https://mintcdn.com/retellai/YUAGucMAZXdWZD8p/images/custom-telephony/twilio/origination.jpeg?fit=max&auto=format&n=YUAGucMAZXdWZD8p&q=85&s=4267334c1b7365c242ab6f271d3efc66" data-path="images/custom-telephony/twilio/origination.jpeg" />
  </Step>

  <Step title="Move numbers to elastic SIP trunking">
    You've created the elastic SIP trunk. Now purchase numbers or move
    existing numbers to this trunk.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/numbers.jpeg?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=c0dc1323bc03138d736eaa31305f5d3a" data-path="images/custom-telephony/twilio/numbers.jpeg" />
  </Step>

  <Step title="Import numbers to Retell">
    Now that the number is set up with your elastic SIP trunk, import it to Retell so we know how to route the call.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/import-number.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=19c4a219923eb0adc03c807cb8d767c3" data-path="images/custom-telephony/import-number.png" />

    Here you supply the termination SIP URI you set up in Step 1. If you set up
    auth via credentials, supply the username and password as well.

    <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/import-number-detail-twilio.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=da61515fce726ff48b2a00846f81e190" data-path="images/custom-telephony/import-number-detail-twilio.png" />

    You can also import numbers programmatically via [Import Number API](/api-references/import-phone-number).

    Now that the number is imported, you can make and receive calls with it just like a number
    purchased from Retell. It shows up in your dashboard, and you can make phone calls from the
    dashboard directly. You can also use the [Create Phone Call API](/api-references/create-phone-call)
    to create calls programmatically.
    To have Retell stop using this number, delete it from the dashboard or via
    the [Delete Number API](/api-references/delete-phone-number).
  </Step>
</Steps>

## Common issues

**1. After connecting, inbound works but outbound does not?**

* Check your termination SIP URI.
  If there's a space in it, remove it. Also use a localized termination URI near your region. See this [doc](https://www.twilio.com/docs/global-infrastructure/localized-uris/termination) to select one.

* Check your username and credentials.
  Make sure you entered the username and credentials shown in this dialog.
  Note that the username is not the friendly name shown in the credential list;
  double-check that you didn't confuse the two.

<img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/credentials.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=9261227c219742c9a8d02d29dde0d326" data-path="images/custom-telephony/twilio/credentials.png" />

**2. How do I set up dialing to international countries?**

* Search "geo" to find the "Voice Geographic Permissions" setting.
  <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/geo.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=a0c345fdd79fd5ae1a1018630ec9c472" data-path="images/custom-telephony/twilio/geo.png" />

* Choose "Elastic SIP Trunking" in the selector, and select the countries you would like to dial. See [International calling](/deploy/international-call) for Retell's supported destinations and fees.
  <img height="200" src="https://mintcdn.com/retellai/32uO5g9DswfoJ9j7/images/custom-telephony/twilio/international.png?fit=max&auto=format&n=32uO5g9DswfoJ9j7&q=85&s=e63218daa41a0660de38538b952785e8" data-path="images/custom-telephony/twilio/international.png" />

Still stuck? See [Debug outbound call](/reliability/debug-outbound-call) for common SIP failures, or [debug SIP calls with PCAP files](/reliability/debug-calls-pcap) to inspect the signaling in Wireshark.

## Phone number masking

If you have a personal phone number or a trusted business number you'd like to display to the callee, you can import verified phone numbers into Twilio to serve as the caller ID.

### Add a caller ID

1. Go to the [Verified Caller IDs page](https://www.twilio.com/console/phone-numbers/verified?_gl=1*l511c3*_gcl_aw*R0NMLjE3NTQ0MTE4NjUuQ2p3S0NBancxZExEQmhCb0Vpd0FRTlJpUVk5QWVqRDFrc2hEZl9ycVdOVlI4bmR3RVNTei1nM3JBcGpORVFXX3BCLXdMRFZPTnM3ei14b0NWNmdRQXZEX0J3RQ..*_gcl_au*MTAzNDAwNDYyNi4xNzUyNTE4OTQy*_ga*MjA5ODY1MzAyMS4xNzUyNTE4OTQy*_ga_RRP8K4M4F3*czE3NTczNzcyODAkbzI0JGcxJHQxNzU3MzgwOTUxJGo2MCRsMCRoMA..)
2. Click **Add a new Caller ID**
3. Enter the desired phone number to verify, select the desired verification method, and then click **Verify Number**

<img height="200" src="https://mintcdn.com/retellai/wDbYgGV0r_bHeVyI/images/custom-telephony/twilio/add-caller-id.jpeg?fit=max&auto=format&n=wDbYgGV0r_bHeVyI&q=85&s=9b365e1a769c73efccf907d0431497b1" data-path="images/custom-telephony/twilio/add-caller-id.jpeg" />

4. The number entered will receive an OTP Authentication code for verification. Enter this verification code on the next window.

<img height="200" src="https://mintcdn.com/retellai/T6odbQPcAHafphF3/images/custom-telephony/twilio/otp-code.jpeg?fit=max&auto=format&n=T6odbQPcAHafphF3&q=85&s=6af725385dab140d234749ffb9a82ef7" data-path="images/custom-telephony/twilio/otp-code.jpeg" />

5. Once you click **Submit**, if the correct OTP code was entered, you will receive a **Successful** notification and the number will be added to your account as a verified caller ID.

### Configure SIP header rules to display caller ID

This uses Twilio's own header manipulation. To set or parse [custom SIP headers](/build/telephony/sip-headers) on the Retell side, see the SIP headers guide.

1. In the Twilio Console, navigate to Elastic SIP Trunking → Trunks → \[your trunk] → Termination
2. Scroll down to Header Manipulation and open **View all SIP header manipulation policies**

<img height="200" src="https://mintcdn.com/retellai/xg079QHgWguuhP1T/images/custom-telephony/twilio/header-policies.png?fit=max&auto=format&n=xg079QHgWguuhP1T&q=85&s=1bcd8c4549e65c7d06945d4cd88b6123" data-path="images/custom-telephony/twilio/header-policies.png" />

3. Click on **Create a policy** in the top right and give your policy a friendly name
4. Click on **+ Add request rule** and give the rule a friendly name
5. Under the Actions section, set the following values:
   * **SIP header field**: **From number**
   * **Action**: **Replace with**
   * **Value**: Your caller ID in E.164 format (e.g. +18881230987)

<img height="200" src="https://mintcdn.com/retellai/xg079QHgWguuhP1T/images/custom-telephony/twilio/actions.png?fit=max&auto=format&n=xg079QHgWguuhP1T&q=85&s=ab914fc9a20b9bfb5c33e8c619a93900" data-path="images/custom-telephony/twilio/actions.png" />

6. Click **Add rule**, and then click **Save policy**
7. Return to the Termination tab within your trunk, and select the new header manipulation policy from the dropdown
8. Now, any number imported from this Twilio trunk into your Retell account will use your verified caller ID. Place an outbound test call to validate the changes.
   * *Note:* You will need at least one number purchased from Twilio in your SIP trunk in order to apply the caller ID.
