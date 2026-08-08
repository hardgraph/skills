> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect Retell voice agents to custom telephony

> Integrate Retell voice agents with your own telephony provider using elastic SIP trunking or imported numbers from Twilio, Telnyx, and Vonage.

## Overview

This guide shows how to integrate Retell agents with your telephony provider and use your own numbers. This works with any agent type.

There are two ways to integrate:

1. **Elastic SIP trunking**: The **recommended** option if your telephony provider supports elastic SIP trunking. You set up a SIP trunk, configure your number to point to it, and import that number to Retell.
2. **Dial to SIP URI**: If your provider does not support elastic SIP trunking, or you have a more complex telephony setup, you can dial the call to a specific SIP URI.

<Note>
  * Retell SIP server uri: `sip:sip.retellai.com`
  * IP block for traffic: `18.98.16.120/30` (All regions), `3.42.144.0/23` (All regions), `153.57.128.0/18` (All regions), `143.223.88.0/21` (certain United States traffic), `161.115.160.0/19` (certain United States traffic)

  Use these IP blocks to whitelist traffic to Retell's SIP server. Many telephony providers require this.
</Note>

Transport method supported:

* TCP (Recommended)
* UDP
* TLS
* mTLS ([Learn more](#mutual-tls-mtls))

To use a different transport for inbound calls, append the transport method to the SIP server URL:

* For TCP, the SIP server URL needs to be `sip:sip.retellai.com;transport=tcp`
* For UDP, the SIP server URL needs to be `sip:sip.retellai.com;transport=udp`
* For TLS, the SIP server URL needs to be `sip:sip.retellai.com;transport=tls`

Media encryption supported:

* SRTP (Transport should be set to TLS to use SRTP)

Audio Codecs supported:

* PCMU
* PCMA
* G.722(HD)

## Method 1: Elastic SIP trunking (Recommended)

Elastic SIP trunking is a service offered by cloud communications platforms that lets
organizations connect their existing PBX (Private Branch Exchange) or VoIP (Voice over IP)
infrastructure to the Public Switched Telephone Network (PSTN) over the internet using
SIP (Session Initiation Protocol).

Here, elastic SIP trunking connects Retell's VoIP with PSTN so your agents can [make outbound calls](/deploy/outbound-call) and [receive inbound calls](/deploy/inbound-call). All telephony features supported by Retell numbers work here too, as long as your provider supports them.

Many telephony providers offer SIP trunking, so yours most likely supports it.

Here are detailed guides for some telephony providers:

* [Twilio](/deploy/twilio)
* [Telnyx](/deploy/telnyx)
* [Vonage](/deploy/vonage)

Other telephony providers that support SIP trunks work too; use these guides as a reference.

Using a contact center platform instead? See the dedicated guides for [Avaya](/deploy/avaya), [Genesys Cloud](/deploy/genesys), [Five9](/deploy/five9), and [Amazon Connect](/deploy/amazon-connect).

### FAQ

<AccordionGroup>
  <Accordion title="Will it throw errors if the details or configuration is incorrect?">
    No, Retell will not be able to know if the setup you provide works or not until a call is made.
  </Accordion>

  <Accordion title="My inbound call is not connecting, what should I do?">
    Please check your origination setting in your SIP trunking provider. Also check the logs in your telephony provider,
    and perhaps open a ticket with them.
  </Accordion>

  <Accordion title="My outbound call is not connecting, what should I do?">
    Please check your termination setting in your SIP trunking provider, and make sure you provide the right termination URL to Retell.
    Also check the logs in your telephony provider, and perhaps open a ticket with them. See [Debug outbound call](/reliability/debug-outbound-call) for common SIP failures, and [Debug SIP calls with PCAP files](/reliability/debug-calls-pcap) to inspect the signaling in Wireshark.
  </Accordion>

  <Accordion title="Will I be able to transfer call using SIP trunking?">
    Yes you can use the transfer call feature with SIP trunking. Please note that if you intend to use SIP REFER (cold transfer
    with the transferee's number), you will need to configure that in your SIP trunking provider to allow SIP REFER and PSTN transfer,
    with the transferee's number showing as caller id. To pass call metadata over the trunk, see [custom SIP headers](/build/telephony/sip-headers).
  </Accordion>

  <Accordion title="Will I be able to press digit using SIP trunking?">
    Yes, you can. See [Capture DTMF input](/build/user-dtmf) to read digits the caller presses.
  </Accordion>

  <Accordion title="Can I view and manage the numbers I import using SIP trunking?">
    Yes, you can.
  </Accordion>

  <Accordion title="I need to change the configuration of the imported number, how can I do that?">
    Right now you'd need to delete and re-import the number.
  </Accordion>
</AccordionGroup>

## Method 2: Dial to SIP URI

If your telephony provider does not support elastic SIP trunking, or you have a more complicated
telephony setup that cannot use elastic SIP trunking, you can use this method.

When using this method, Retell does not directly make or receive calls, but instead
relies on your system to dial the call to the respective SIP URI. This would require you to
have some code to handle integration with your telephony provider. All traffic looks like
inbound to Retell in this case, so it's up to you to specify the call direction.

<Warning>When using this method, you will not be able to use Retell's transfer call feature, as we do not
have access and control over the telephony provider and number, and cannot initiate transfer for you. You can however implement your own transfer logic and use a custom function to trigger a call transfer.</Warning>

Here we assume you already have your call handling setup.
For Retell to know what agent to use to handle the call, and for you to
**obtain SIP URI to use** for this call, you would call the
[Register Phone Call API](/api-references/register-phone-call).
You will get a `call_id` back from this API, and you would use it to piece
together the SIP URI.

<Note>SIP URI: `sip:{call_id}@sip.retellai.com`</Note>

<Warning>
  You must dial the call to the SIP URI within **5 minutes** of calling Register Phone Call. If the call is not connected within that window, it disconnects with `registered_call_timeout`. See [Debug call disconnection](/reliability/debug-call-disconnect) for disconnection reasons.
</Warning>

### Example code

Here's a simplified example that handles the inbound call
webhook from Twilio and dials the call to the SIP URI.

```Typescript Node theme={"dark"}
const client = new Retell({
  apiKey: 'YOUR_RETELL_API_KEY',
});

server.app.post(
  "/voice-webhook",
  async (req: Request, res: Response) => {
    // Register the phone call to get call id
    const phoneCallResponse = await client.call.registerPhoneCall({
      agent_id: 'oBeDLoLOeuAbiuaMFXRtDOLriTJ5tSxD',
      from_number: "+12137771234", // optional
      to_number: "+12137771235", // optional
      direction: "inbound", // optional
    });

    // Start phone call websocket
    const voiceResponse = new VoiceResponse();
    const dial = voiceResponse.dial();
    dial.sip(
      `sip:${phoneCallResponse.call_id}@sip.retellai.com`,
    );
    res.set("Content-Type", "text/xml");
    res.send(voiceResponse.toString());
  },
);

```

### FAQ

<AccordionGroup>
  <Accordion title="How to transfer call/end call using dial to SIP method?">
    You will need to handle the transfer call/end call logic yourself, meaning that you will need to write a custom function which internally
    interacts with your telephony provider to transfer the call/end the call.
  </Accordion>

  <Accordion title="Will I be able to press digit using dial to SIP method?">
    Yes.
  </Accordion>

  <Accordion title="Two entries created for a single dial?">
    Check your code that interacts with the telephony provider. Some providers like Twilio can call the webhook multiple times for a single call.
    In those cases, you will need to dedupe the call.
  </Accordion>

  <Accordion title="Can I use telephony provider's other features like AMD detection using this method?">
    Yes, using this method basically gives you full control over telephony functionalities, so you should be able to use telephony provider's
    other features. Retell also has built-in [voicemail and IVR detection](/build/handle-voicemail) if you'd rather handle it on our side.
  </Accordion>
</AccordionGroup>

## Security center

Retell secures your voice communications. We recommend using **TLS 1.2 or higher** to secure all SIP signaling channels between your infrastructure and Retell's SIP servers.

To establish a secure TLS connection with Retell's SIP server, you may need to add Retell's server-side root CA to your trust store. You can download it [here](https://www.amazontrust.com/repository/G2-RootCA1.pem).

### Mutual TLS (mTLS)

Mutual TLS (mTLS) is an extension of the standard TLS protocol that provides two-way authentication between a client and a server. Unlike standard TLS — where only the server presents a certificate to prove its identity to the client — mTLS requires **both sides** to present and verify certificates. This ensures that only trusted, authenticated parties can establish a connection, significantly reducing the risk of man-in-the-middle attacks and unauthorized access.

In a typical TLS handshake:

1. The server presents its certificate to the client.
2. The client verifies the server's certificate and the connection is established.

With mTLS, an additional step is added:

1. The server presents its certificate to the client.
2. The **client also presents its certificate** to the server.
3. Both parties verify each other's certificates before the connection is established.

This mutual verification makes mTLS particularly valuable for SIP-based voice infrastructure, where securing signaling and media traffic between trusted systems is critical.

**Retell supports Mutual TLS (mTLS)** for SIP connections. If you require mTLS for your setup, reach out to us via the [Customer Support Portal](https://support.retellai.com/) or at [support@retellai.com](mailto:support@retellai.com) to get it configured for your account.

Retell uses a client certificate issued by AWS Private Certificate Authority (PCA). To validate Retell's TLS client certificate, you **must** add the [root certificate](https://retell-trust-store.s3.us-west-2.amazonaws.com/pca/client-root-ca.pem) to your SIP server's trusted certificate store — without this, your server will reject incoming connections from Retell.

## Video tutorials

### Elastic SIP trunking

<div style={{ marginBottom: "8px", fontWeight: "bold" }}>
  Twilio
</div>

<div style={{ marginBottom: "24px" }}>
  <iframe width="360" height="200" src="https://www.youtube.com/embed/JC3PV_R-Z1Y?si=JCV66VZwhtutLrWG" title="Twilio SIP Trunking Integration with Retell" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />
</div>

<div style={{ marginBottom: "8px", fontWeight: "bold" }}>
  Telnyx
</div>

<div style={{ marginBottom: "24px" }}>
  <iframe width="360" height="200" src="https://www.loom.com/embed/780f5eafbb0d4e4cbe7d6f68bd371a4f?autoplay=false&hide_owner=true&hide_share=true&hide_title=true" title="Telnyx SIP Trunking Integration with Retell" frameBorder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />
</div>

<div style={{ marginBottom: "8px", fontWeight: "bold" }}>
  Vonage
</div>

<div style={{ marginBottom: "24px" }}>
  <iframe width="360" height="200" src="https://www.loom.com/embed/4cb20655656c4415896b2a0e6e8fa1fa?autoplay=false&hide_owner=true&hide_share=true&hide_title=true" title="Vonage SIP Trunking Integration with Retell" frameBorder="0" allowFullScreen style={{ border: "none" }} />
</div>

### Dial to SIP URI

<div style={{ marginBottom: "24px" }}>
  <iframe width="360" height="200" src="https://www.youtube.com/embed/1_BdZA8o1h0?si=IGuaBg8pArbrLNzA" title="Custom Telephony Integration Demo" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />
</div>

## Telephony partners

If you need to manipulate the SIP call flow or have custom needs Retell doesn't directly support, check our telephony partners to see if they can help.

* [Jambonz](https://www.retellai.com/app-partner/jambonz): Jambonz is a SIP server that supports static IP address and can be used to connect to Retell's SIP server to manipulate the SIP call flow. A dedicated Slack support channel is available for Retell users.
* [Cloudonix](https://www.retellai.com/app-partner/cloudonix): Cloudonix is a CPaaS that can be connected to Retell's SIP server to manipulate the SIP call flow.
