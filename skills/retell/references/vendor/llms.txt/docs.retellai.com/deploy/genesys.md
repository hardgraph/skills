> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect Genesys Cloud to Retell

> Set up a Genesys Cloud SIP phone trunk, number plans, and outbound routes to send inbound and outbound calls between Genesys and Retell agents.

<Note>
  Customers with an enterprise or paid support plan can contact us via the <a href="https://support.retellai.com/">Customer Support Portal</a> or [support@retellai.com](mailto:support@retellai.com) for step-by-step guidance to connect with your Genesys infrastructure.
</Note>

## Overview

This guide walks through connecting Genesys Cloud to Retell using a **SIP Phone Trunk**. Retell registers with Genesys as a SIP endpoint, allowing Genesys to route inbound calls to Retell agents and allowing Retell to place outbound calls through Genesys numbers.

For the general model of how Retell connects to a third-party provider over SIP, see [custom telephony](/deploy/custom-telephony).

**Retell SIP details:**

* SIP server URI: `sip.retellai.com`
* IP ranges: `18.98.16.120/30` (all regions), `143.223.88.0/21` (certain US traffic), `161.115.160.0/19` (certain US traffic)
* Recommended transport: TCP (also supports UDP and TLS/SRTP)
* Supported audio codecs: PCMU (G.711 µ-law), PCMA (G.711 A-law), G.722

***

<Steps>
  <Step title="Firewall considerations">
    Before creating the trunk, ensure your network allows SIP signaling and RTP media between Genesys Cloud and Retell. Genesys Cloud best practice is to specify IP subnets or addresses rather than allowing all traffic — see [About Trunks](https://help.genesys.cloud/articles/about-trunks/) for Genesys security guidance.

    **Whitelist Retell IP ranges (allow inbound SIP from Retell to Genesys):**

    | CIDR Block         | Coverage           |
    | ------------------ | ------------------ |
    | `18.98.16.120/30`  | All regions        |
    | `143.223.88.0/21`  | Certain US traffic |
    | `161.115.160.0/19` | Certain US traffic |

    **Ports to open bidirectionally:**

    | Protocol  | Port        | Purpose                                         |
    | --------- | ----------- | ----------------------------------------------- |
    | TCP / UDP | 8060        | SIP signaling (Genesys SIP phone trunk default) |
    | TCP       | 8061        | SIP over TLS (if using TLS transport)           |
    | UDP       | 16384–32766 | RTP / SRTP media                                |

    <Note>
      Genesys Cloud uses ports **8060** (UDP/TCP) and **8061** (TLS) for SIP phone trunks — different from the standard SIP port 5060/5061. Ensure your firewall and any SBC rules use these Genesys-specific ports.
    </Note>

    **Genesys Cloud media server IPs:**

    Genesys Cloud media traffic originates from AWS regions. Whitelist the IP ranges for your deployed [AWS region](https://help.mypurecloud.com/articles/aws-regions-for-genesys-cloud-deployment/) so Retell's servers can receive RTP from Genesys. Your Genesys Cloud login URL indicates your region (e.g., `login.mypurecloud.com` = US East, `login.mypurecloud.de` = Frankfurt).

    If you are routing calls through an on-premises SBC, apply these rules at the SBC rather than the perimeter firewall.
  </Step>

  <Step title="Create a SIP phone trunk in Genesys Cloud">
    A SIP Phone Trunk in Genesys Cloud lets Retell register as a SIP endpoint that Genesys can send calls to and receive calls from. Follow the steps below, or refer to the official [Create a SIP Phone Trunk](https://help.genesys.cloud/articles/create-sip-phone-trunk/) guide.

    1. In Genesys Cloud, go to **Admin** > **Telephony** > **Trunks** (or **Menu** > **Digital and Telephony** > **Telephony** > **Trunks**).

    2. Select the **Phone Trunks** tab.

    3. Click **Create New**.

    4. Enter a name in the **Phone Trunk Name** field (e.g., `Retell-SIP-Trunk`).

    5. Set **Type** to **SIP**.

    6. Verify **Trunk State** is set to **In-Service**.

    7. Under **Protocol and Listen Port**, select your transport and corresponding port:
       * **UDP** or **TCP** → port `8060` (recommended)
       * **TLS** → port `8061` (use this if you require SRTP media encryption)

    8. Under **Registrations**, set the **Max Registration Rate** to limit REGISTER request frequency if required.

    9. Under **SIP Access Control**, configure source IP restrictions:

       * Set **Use Source Address** to **Yes**.
       * Add each Retell IP subnet in CIDR notation:
         * `18.98.16.120/30`
         * `143.223.88.0/21`
         * `161.115.160.0/19`
       * Leave **Always Deny** blank.

           <Note>
             Genesys best practice is to always specify IP subnets here rather than allowing all. This restricts SIP INVITE acceptance to Retell's known IP ranges.
           </Note>

    10. Click **Save Phone Trunk**.

    **Codec configuration:**

    Genesys Cloud supports several codecs on SIP phone trunks (see [SIP Phone Trunk Settings](https://help.genesys.cloud/articles/sip-phone-trunk-settings/)). Configure at least one of the three codecs Retell supports — PCMU is recommended:

    | Codec              | Genesys format | Notes                                   |
    | ------------------ | -------------- | --------------------------------------- |
    | PCMU (G.711 µ-law) | `audio/PCMU`   | Recommended — standard in North America |
    | PCMA (G.711 A-law) | `audio/PCMA`   | Standard outside North America          |
    | G.722              | `audio/G722`   | Wideband (HD voice)                     |

    **TLS / SRTP (optional):**

    If you selected TLS transport, Genesys defaults to TLS v1.2. You can choose from AES-based cipher suites and enable SRTP for media encryption. Mutual TLS authentication is disabled by default. See [SIP Phone Trunk Settings](https://help.genesys.cloud/articles/sip-phone-trunk-settings/) for the full list of TLS and SRTP cipher options.

    **Advanced settings:**

    For call limits, NAT traversal (FENT), inbound digest authentication, and diagnostic captures, refer to [Configure Advanced SIP Phone Trunk Settings](https://help.genesys.cloud/articles/configure-advanced-sip-phone-trunk-settings/). Genesys recommends using defaults unless you have a specific reason to change them.
  </Step>

  <Step title="Configure phone numbers to route through Retell">
    After creating the trunk, assign or configure phone numbers so inbound calls are directed to Retell and outbound calls from Retell use the correct caller ID.

    **Assign numbers to the SIP trunk:**

    1. Go to **Admin** > **Telephony** > **Phone Numbers**.
    2. Select the number you want to use with Retell.
    3. Edit the number and assign it to the `Retell-SIP-Trunk` you created in Step 2.

    **Import the number into Retell:**

    Retell needs to know the number and how to reach your Genesys trunk for outbound calls.

    1. In the Retell dashboard, go to **Phone Numbers** > **Import Number**.
    2. Fill in the following fields:
       * **Phone Number**: The E.164 format number (e.g., `+12137771234`).
       * **Termination SIP URI**: Your Genesys SIP trunk endpoint. This is the SIP address Retell will dial for outbound calls — typically the FQDN or IP of your Genesys Edge or SBC, on port `8060` (e.g., `your-edge.genesys.com:8060`). Check your Genesys trunk configuration or contact your Genesys admin for the correct address.
       * **SIP Username / Password**: If you configured inbound digest authentication on the Genesys trunk, enter those credentials here.
    3. Save the number and assign a Retell agent to handle calls on it.

    You can also import numbers programmatically via the [Import Number API](/api-references/import-phone-number).

    Once imported, the number appears in your Retell dashboard and you can place and receive calls just like a Retell-purchased number — see [Make Outbound Calls](/deploy/outbound-call) and [Receive Inbound Calls](/deploy/inbound-call).

    To pass metadata such as a caller or account ID between Genesys and your agent, use [custom SIP headers](/build/telephony/sip-headers).
  </Step>

  <Step title="Configure inbound call routing in Genesys">
    For inbound calls arriving at your Genesys number that should be handled by a Retell agent:

    1. In Genesys Cloud, go to **Admin** > **Routing** > **Call Routing** (or use **Architect** for more complex flows).
    2. Create an **Inbound Call Flow** in Architect that transfers the call to the Retell SIP endpoint:
       * Add a **Transfer to SIP** action.
       * Set the SIP URI to `sip:{call_id}@sip.retellai.com`, where `call_id` comes from a prior call to the [Register Phone Call API](/api-references/register-phone-call).
       * Alternatively, if you imported the number into Retell with the correct termination URI (Step 3), Retell handles call routing automatically when a call is placed to that number — Genesys forwards the SIP INVITE to `sip.retellai.com` via the trunk.
    3. Under **Admin** > **Routing** > **Call Routing**, create a **DID Route** mapping your phone number to this call flow.
    4. Publish the flow.

    **Configure the Site so Genesys routes transferred calls through the Retell trunk:**

    For calls that Genesys transfers out (for example, when a Retell agent hands the call back to Genesys and Genesys then transfers it to another PSTN destination), Genesys uses the caller's **Site** to decide which trunk to use. You must add a number plan and outbound route that select the Retell SIP trunk. See [Manage sites](https://help.genesys.cloud/articles/manage-sites/) and [About number plans](https://help.genesys.cloud/articles/about-number-plans/) for Genesys reference.

    1. Go to **Admin** > **Telephony** > **Sites** and open the Site your users or flows are assigned to.
    2. On the **Number Plans** tab, click **Add** and create a plan that matches the destinations you will transfer to:
       * **Name**: e.g., `Retell-Outbound-US`.
       * **Classification**: choose or create a classification (e.g., `US-Domestic`).
       * **Match Type**: `E.164 Number List`, `Number List`, or `Regular Expression` depending on your dial pattern.
       * **Numbers / Regex**: the destinations that should be routed through Retell (e.g., `^\+1\d{10}$` for US E.164, or a specific number list).
       * **Normalization**: keep or set to E.164 so the number arrives at Retell in `+1XXXXXXXXXX` format.
    3. Save the number plan and drag it above any other plans that would otherwise match the same numbers — plans are evaluated top-down.
    4. On the **Outbound Routes** tab, click **Add** and create a route:
       * **Name**: e.g., `Retell-Outbound-Route`.
       * **Classifications**: select the classification you used in the number plan (e.g., `US-Domestic`).
       * **External Trunks**: select `Retell-SIP-Trunk` (the trunk created in Step 2). If you have multiple trunks listed, order them so the Retell trunk is preferred.
       * **Distribution**: `Sequential` (fail over in order) or `Random` if you want load-balancing across trunks.
    5. Save the outbound route. Republish any Architect flows that reference the Site so the routing change takes effect.

    <Note>
      If transferred calls are still leaving through the wrong carrier, the most common causes are: the classification on the number plan does not match any classification on the outbound route, another number plan higher in the list is matching first, or the user/flow initiating the transfer is assigned to a different Site than the one you edited.
    </Note>

    **Placing outbound calls from Retell through Genesys:**

    Use the Retell dashboard or the [Create Phone Call API](/api-references/create-phone-call) to place outbound calls. Retell sends a SIP INVITE to the Genesys termination URI you configured in Step 3, and Genesys routes the call to the PSTN with the assigned number as the caller ID.
  </Step>

  <Step title="TLS and SRTP with Retell">
    Use TLS transport and SRTP media encryption when you need end-to-end signaling and media security between Genesys and Retell. This is optional — TCP is sufficient for most deployments.

    **How it works:**

    * **TLS** encrypts the SIP signaling channel (the INVITE, BYE, etc.).
    * **SRTP** encrypts the RTP audio stream. Retell requires TLS transport to use SRTP; you cannot use SRTP over TCP or UDP.

    **Retell SIP URI for TLS:**

    When importing the number into Retell or configuring the termination URI, append the transport parameter:

    ```
    sip:sip.retellai.com;transport=tls
    ```

    **Genesys trunk configuration for TLS:**

    1. In the trunk settings (**Admin** > **Telephony** > **Trunks** > **Phone Trunks** > your trunk), set **Protocol** to **TLS** and **Listen Port** to `8061`.
    2. Under [SIP Phone Trunk Settings](https://help.genesys.cloud/articles/sip-phone-trunk-settings/), configure:
       * **TLS Version**: `TLS v1.2` (Genesys default — matches Retell's supported version).
       * **Cipher Suite**: Choose an AES cipher. Recommended: `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384` or `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`.
       * **Mutual TLS**: Disabled by default. Leave disabled unless your security policy requires client certificate authentication from Retell. If you need mutual TLS, contact the <a href="https://support.retellai.com/">Customer Support Portal</a> or [support@retellai.com](mailto:support@retellai.com) as this requires coordination with Retell's infrastructure team.
    3. Enable **SRTP** under the media/security settings. Genesys supports the following SRTP cipher suites, all of which Retell accepts:
       * `AES_CM_128_HMAC_SHA1_80` (recommended)
       * `AES_CM_128_HMAC_SHA1_32`
       * `AES_CM_256_HMAC_SHA1_80`
       * `AES_CM_256_HMAC_SHA1_32`

    **Firewall for TLS:**

    Ensure port `8061` (TCP) is open bidirectionally between Retell's IP ranges and your Genesys Cloud edge, in addition to the RTP media ports.

    | Protocol | Port        | Purpose                        |
    | -------- | ----------- | ------------------------------ |
    | TCP      | 8061        | SIP over TLS                   |
    | UDP      | 16384–32766 | SRTP media (same ports as RTP) |

    <Note>
      Retell's SIP server presents a certificate signed by the **Amazon Trust Services** CA (Amazon runs Retell's infrastructure). Genesys Cloud validates this automatically for most deployments. If certificate validation fails and calls do not connect — particularly when an on-premises SBC or Edge device is in the path — you may need to install Retell's Root CA on that device.

      Download the following certificates from the [Amazon Trust Services repository](https://www.amazontrust.com/repository/) and install them in the trusted CA store of your Genesys Edge or SBC:

      * **Root CA**: Amazon Root CA 1 (`AmazonRootCA1.pem`)
      * **Intermediate CA** (optional, install if your device requires the full chain): `C=US, O=Amazon, CN=Amazon RSA 2048 M01`

      The intermediate certificate is typically not required if your device performs online certificate chain validation, but some on-premises SBCs and Edge appliances require the full chain to be pre-installed locally. If you see TLS handshake failures with an "unknown CA" or "incomplete chain" error, install both the root and the intermediate.

      Also check that your SBC is not intercepting TLS with its own self-signed certificate before forwarding to Retell, as this will cause validation to fail on Retell's side.
    </Note>
  </Step>

  <Step title="Test the integration">
    **Inbound test (external call → Genesys number → Retell agent):**

    1. Call the number assigned to the `Retell-SIP-Trunk` from an external phone.
    2. Confirm the call appears in Retell's **Calls** dashboard and is handled by the correct agent.
    3. Verify bidirectional audio and that the call completes cleanly.

    **Outbound test (Retell → Genesys → PSTN):**

    1. Initiate an outbound call from the Retell dashboard or API using the imported number.
    2. Confirm the receiving party sees the correct caller ID.
    3. Check audio quality in both directions.
  </Step>

  <Step title="Debug call issues on Genesys">
    If calls fail or have audio problems, use the following tools. For Retell-side diagnostics, see [debug outbound connection issues](/reliability/debug-outbound-call) for `not_connected` failures and disconnection reasons, and [debug SIP calls using PCAP files](/reliability/debug-calls-pcap) to analyze signaling and media in Wireshark.

    **1. Interaction Detail Records**

    Go to **Performance** > **Workspace** > **Interactions** (or **Contact Center** > **Interactions**). Search by ANI, DNIS, or time range. Click an interaction to see the full SIP event timeline, call path, and any error codes:

    * `403 Forbidden` → SIP access control mismatch; verify Retell IP subnets are whitelisted on the trunk.
    * `404 Not Found` → Incorrect SIP URI or trunk routing misconfiguration.
    * `503 Service Unavailable` → Retell SIP server unreachable; check DNS for `sip.retellai.com` and firewall rules.

    **2. Trunk Status**

    Go to **Admin** > **Telephony** > **Trunks** > **Phone Trunks** and check the status of `Retell-SIP-Trunk`. A **Down** or **Degraded** state indicates a connectivity or registration problem. Verify firewall rules and that Retell's IPs are correctly added to SIP Access Control.

    **3. Protocol Capture (SIP Trace)**

    Enable protocol capture under [Advanced SIP Phone Trunk Settings](https://help.genesys.cloud/articles/configure-advanced-sip-phone-trunk-settings/) in the **Diagnostic** section. Use this together with Genesys Cloud Technical Support to inspect raw SIP messages. Look for:

    * Codec mismatch in SDP offer/answer — ensure at least one common codec (e.g., PCMU) is in both Genesys and Retell's codec lists.
    * Missing or rejected `Contact` / `Via` headers.
    * Authentication challenges (401/407) — configure inbound digest auth credentials consistently on both sides.
    * NAT issues — if Genesys is behind NAT, enable Far-End NAT Traversal (FENT) in the transport section of advanced trunk settings.

    **4. One-way or no audio**

    One-way audio is almost always a firewall or NAT issue blocking RTP in one direction:

    * Confirm UDP 16384–32766 is open bidirectionally between Genesys media servers and Retell's IP ranges.
    * Check the SIP trace SDP `c=` and `m=` lines to confirm the IP address and port Genesys is advertising for media.
    * Verify Retell received and is sending audio by checking the call in the Retell dashboard.

    **5. Quick reference**

    | Symptom                  | Likely Cause                             | Fix                                             |
    | ------------------------ | ---------------------------------------- | ----------------------------------------------- |
    | Call not reaching Retell | Trunk misconfigured or firewall blocking | Check SIP Access Control IPs and firewall rules |
    | 403 Forbidden            | IP not whitelisted on trunk              | Add Retell CIDR blocks to SIP Access Control    |
    | 503 Service Unavailable  | `sip.retellai.com` unreachable           | Check DNS resolution and firewall on port 8060  |
    | One-way audio            | RTP blocked or NAT issue                 | Open UDP 16384–32766, check SDP IPs in trace    |
    | No audio (codec)         | No shared codec in SDP                   | Add PCMU/PCMA to Genesys trunk codec list       |
    | Call drops at \~30s      | Mid-call SIP re-INVITE blocked           | Allow mid-call signaling through firewall       |
    | Call drops on register   | Max Registration Rate too low            | Increase Max Registration Rate on trunk         |

    <Note>
      If you cannot resolve the issue, capture the Genesys Interaction ID and the Retell Call ID (from the Retell dashboard) and contact the <a href="https://support.retellai.com/">Customer Support Portal</a> or [support@retellai.com](mailto:support@retellai.com).
    </Note>
  </Step>
</Steps>
