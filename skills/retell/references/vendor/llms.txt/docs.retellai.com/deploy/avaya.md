> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Avaya

> A step-by-step guide to integrate Avaya Aura with Retell's SIP endpoints using Avaya SBCE as the border element to send and receive phone calls.

<Note>
  Customers with an enterprise or paid support plan can contact us via the <a href="https://support.retellai.com/">Customer Support Portal</a> or [support@retellai.com](mailto:support@retellai.com) for step-by-step guidance to connect with your Avaya infrastructure.
</Note>

## Overview

This guide covers connecting **Avaya Aura** to Retell using **Avaya Session Border Controller for Enterprise (SBCE)** as the SIP border element between Retell and the Avaya core (Session Manager and Communication Manager).

**Architecture:**

```
Retell ↔ Avaya SBCE (B1 external) | (A1 internal) ↔ Session Manager ↔ Communication Manager
```

The SBCE handles topology hiding, codec interworking, and security policy between the external Retell SIP trunk and your internal Aura infrastructure. All configuration flows through **System Manager** (the central management UI for Aura).

For the general model of how Retell connects to a third-party provider over SIP, see [custom telephony](/deploy/custom-telephony).

**Retell SIP details:**

* SIP server URI: `sip.retellai.com`
* IP ranges: `18.98.16.120/30` (all regions), `143.223.88.0/21` (certain US traffic), `161.115.160.0/19` (certain US traffic)
* Recommended transport: TCP (also supports UDP and TLS/SRTP)
* Supported audio codecs: PCMU (G.711 µ-law), PCMA (G.711 A-law), G.722

**Official Avaya documentation references:**

* [Administering Avaya SBCE](https://documentation.avaya.com/bundle/AdministeringAvayaSBCERelease8/)
* [Administering Avaya Aura Session Manager](https://documentation.avaya.com/bundle/AdministeringSMRelease10/)
* [Administering Avaya Aura Communication Manager](https://documentation.avaya.com/bundle/AdministeringAuraCommunicationManager/)

***

<Steps>
  <Step title="Firewall considerations">
    The SBCE has two network interfaces:

    * **A-side (external / untrusted)** — faces the internet / Retell.
    * **B-side (internal / trusted)** — faces Session Manager and Communication Manager.

    Apply the following rules on your perimeter firewall and on the SBCE's built-in packet filter.

    **Allow inbound to SBCE A-side from Retell IP ranges:**

    | CIDR Block         | Coverage           |
    | ------------------ | ------------------ |
    | `18.98.16.120/30`  | All regions        |
    | `143.223.88.0/21`  | Certain US traffic |
    | `161.115.160.0/19` | Certain US traffic |

    **Ports to open on the SBCE A-side (from/to Retell):**

    | Protocol  | Port       | Purpose                                 |
    | --------- | ---------- | --------------------------------------- |
    | TCP / UDP | 5060       | SIP signaling                           |
    | TCP       | 5061       | SIP over TLS (if using TLS transport)   |
    | UDP       | 1024–65535 | RTP / SRTP media (configurable on SBCE) |

    **Ports to open on the SBCE B-side (toward Session Manager):**

    | Protocol  | Port       | Purpose                                        |
    | --------- | ---------- | ---------------------------------------------- |
    | TCP / UDP | 5060       | SIP signaling to Session Manager               |
    | TCP       | 5061       | SIP/TLS to Session Manager (if TLS internally) |
    | UDP       | 1024–65535 | RTP media toward Communication Manager         |

    <Note>
      The SBCE's media port range is configurable under **SBCE** > **Global Profiles** > **Media Interface**. Match your firewall rules to whatever range you configure there.
    </Note>
  </Step>

  <Step title="Enable SIP trunking on the Avaya system">
    Before creating the trunk, verify SIP trunking is enabled across the Aura stack.

    **On Communication Manager (via System Manager):**

    1. Log in to **Avaya System Manager** (`https://<smgr-ip>/SMGR`).
    2. Navigate to **Elements** > **Communication Manager** > select your CM instance > **System Parameters** > **Customer Options**.
    3. Confirm **SIP Trunking** is set to **y** (enabled). If not, contact your Avaya partner to activate the license.
    4. Confirm **Maximum Administered SIP Trunks** is set high enough to accommodate your expected [concurrent calls](/deploy/concurrency).

    **On Session Manager:**

    1. In System Manager, go to **Elements** > **Session Manager** > **System Status** > **Security Module Status**.
    2. Verify the Session Manager instance shows **Registered** and the SIP signaling service is active.
  </Step>

  <Step title="Configure the SBCE">
    All SBCE configuration is done through the **Avaya SBCE EMS (Element Management System)** web UI (`https://<sbce-ip>/`). Refer to [Administering Avaya SBCE](https://documentation.avaya.com/bundle/AdministeringAvayaSBCERelease8/) for full field-level reference.

    **1. Create Server Interworking Profiles**

    Server Interworking Profiles define how the SBCE adapts SIP behavior for each side of the trunk. You need one for the internal Avaya side and one for the external Retell (service provider) side.

    *Avaya-side profile:*

    1. Go to **Configuration Profiles** > **Server Interworking** > **Add**.
    2. Name it `Avaya`.
    3. Configure:
       * **SIPS Required**: unchecked (not required for this trunk type)
       * **Hold Support**: `None`
       * **Record Route**: `Both Sides`
       * **Include Endpoint IP for Context Lookup**: checked
       * **Extensions**: `Avaya`
       * **Has Remote SBC**: checked
       * **DTMF Support**: `None`
    4. Click **Finish**.

    *Retell (service provider) side profile:*

    1. Click **Add** again and name it `Service-Provider`.
    2. Configure:
       * **SIPS Required**: unchecked
       * **Delayed SDP Handling**: checked
       * **Record Route**: `Both Sides`
       * **Include Endpoint IP for Context Lookup**: checked
       * **Extensions**: `Avaya`
       * **DTMF Support**: `None`
         <Note>If your provider requires RFC 2833 DTMF relay or inband DTMF, change this to `RFC 2833` or `Inband` respectively. Check your provider's application note on [Avaya DevConnect](https://devconnect.avaya.com) for the correct value.</Note>
    3. Click **Finish**.

    ***

    **2. Add SIP Servers**

    SIP Servers represent the endpoints that the SBCE sends traffic to and receives traffic from.

    *Retell (service provider) server:*

    1. Go to **Configuration Profiles** > **Server Configuration** > **Add**.
    2. Name it `Service-Provider`.
    3. Configure:
       * **Server Type**: `Trunk Server`
       * **IP Address / FQDN**: `sip.retellai.com`
       * **Port**: `5060` (use `5061` if TLS)
       * **Transport**: `TCP` (recommended), `UDP`, or `TLS` — match what you configured on the Signaling Interface and what your firewall permits. TCP is preferred for reliability; use TLS if end-to-end encryption is required (see TLS step).
    4. Click **Next**. Leave authentication blank (IP-based trunk, no credentials required).
    5. On the **Heartbeat** tab:
       * Enable **SIP OPTIONS** ping, interval `30` seconds.
       * **From URI**: `sip@<sbce-b1-public-ip>` (your external interface's public IP).
       * **To URI**: `sip@sip.retellai.com`
    6. Click **Next**. Leave **Registration** and **Ping** defaults.
    7. Ensure **Enable Grooming** is unchecked (not applicable for non-TLS trunks; leave unchecked for TCP/UDP).
    8. Set **Interworking Profile** to `Service-Provider`.
    9. Click **Finish**.

    *Avaya Session Manager server:*

    1. Click **Add** and name it `Avaya`.
    2. Configure:
       * **Server Type**: `Call Server`
       * **IP Address**: your primary Session Manager IP (e.g., `10.x.x.x`)
       * **Port**: `5061`
       * **Transport**: `TLS`
    3. Click **Add** to include a secondary Session Manager IP with the same port and transport.
    4. Select the appropriate **TLS Client Profile** for your SBCE A1 interface (created during initial SBCE setup).
    5. Click **Next**. Skip authentication and registration — not required for Session Manager.
    6. Set **Interworking Profile** to `Avaya`.
    7. Click **Finish**.

    ***

    **3. Create Routing Profiles**

    Routing Profiles tell the SBCE where to forward calls for each direction.

    *Route to Retell (outbound):*

    1. Go to **Configuration Profiles** > **Routing** > **Add**.
    2. Name it `Route-to-Retell`.
    3. Click **Add**, set **Priority/Weight** to `1`, select **Server** `Service-Provider`.
    4. Click **Finish**.

    *Route to Session Manager (inbound):*

    1. Click **Add** and name it `Route-to-SessionManager`.
    2. Click **Add**, set priority `1` for your primary Session Manager and priority `2` for the secondary.
    3. Select the `Avaya` server for each entry.
    4. Click **Finish**.

    ***

    **4. Configure Topology Hiding**

    Topology Hiding rewrites SIP headers so internal IP addresses are not exposed externally, and so Session Manager receives the correct SIP domain.

    *Service Provider side:*

    1. Go to **Configuration Profiles** > **Topology Hiding** > **Add**.
    2. Name it `Service-Provider`.
    3. Set all header fields to `IP Domain`, leave all values as `Auto`.
    4. Click **Finish**.

    *Avaya side:*

    1. Click **Add** and name it `Avaya`.
    2. Set all header fields to `IP Domain`.
    3. For the following three fields, overwrite the value with your internal SIP domain (found in **System Manager** > **Routing** > **Domains**, e.g., `avaya.example.com`):
       * **To**
       * **From**
       * **Request Line**
    4. Click **Finish**.

    ***

    **5. Create Signaling Interfaces**

    Signaling Interfaces bind a specific IP address and port for SIP signaling on each SBCE network interface.

    *External interface (facing Retell):*

    1. Go to **Network & Flows** > **Network Management** > **Signaling Interface** > **Add**.
    2. Configure:
       * **Name**: `Service-Provider-External`
       * **IP Address**: your SBCE B1 (external) IP address
       * Enable only the port(s) matching your chosen transport:
         * **TCP Port**: `5060` — if using TCP
         * **UDP Port**: `5060` — if using UDP
         * **TLS Port**: `5061` — if using TLS (disable TCP and UDP)
       * Disable any ports not in use.
    3. Click **Finish**.

    *Internal interface (facing Session Manager):*

    1. Click **Add** and configure:
       * **Name**: `Avaya-Internal`
       * **IP Address**: your SBCE A1 (internal) IP address
       * Disable UDP and TCP ports — only TLS is used on the internal leg
       * **TLS Port**: `5061`
       * **TLS Profile**: select your SBCE A1 TLS client certificate profile
    2. Click **Finish**.

    ***

    **6. Create Media Interfaces**

    Media Interfaces bind the IP addresses used for RTP/SRTP on each leg.

    *External media interface:*

    1. Go to **Network & Flows** > **Network Management** > **Media Interface** > **Add**.
    2. Configure:
       * **Name**: `Service-Provider-External`
       * **IP Address**: your SBCE B1 (external) IP address
       * **Port Range**: default (e.g., `35000–40000`) — adjust to match your firewall rules
    3. Click **Finish**.

    *Internal media interface:*

    1. Click **Add** and configure:
       * **Name**: `Avaya-Internal`
       * **IP Address**: your SBCE A1 (internal) IP address
       * **Port Range**: default
    2. Click **Finish**.

    ***

    **7. Create Server Flows**

    Server Flows tie together the signaling interface, media interface, routing profile, topology hiding profile, and interworking profile for each call direction. You need two flows: one for calls coming in from Retell, and one for calls going out to Retell.

    Go to **Network & Flows** > **End-Point Flows** > **Server Flows**.

    *Flow 1 — Trunk Server (inbound from Retell → Session Manager):*

    | Field                       | Value                       |
    | --------------------------- | --------------------------- |
    | **Name**                    | `Trunk-Server`              |
    | **Server Configuration**    | `Service-Provider`          |
    | **Received Interface**      | `Service-Provider-External` |
    | **Signaling Interface**     | `Avaya-Internal`            |
    | **Media Interface**         | `Service-Provider-External` |
    | **Routing Profile**         | `Route-to-SessionManager`   |
    | **Topology Hiding Profile** | `Service-Provider`          |

    Click **Finish**.

    *Flow 2 — Call Server (outbound from Session Manager → Retell):*

    | Field                       | Value                       |
    | --------------------------- | --------------------------- |
    | **Name**                    | `Call-Server`               |
    | **Server Configuration**    | `Avaya`                     |
    | **Received Interface**      | `Avaya-Internal`            |
    | **Signaling Interface**     | `Service-Provider-External` |
    | **Media Interface**         | `Avaya-Internal`            |
    | **Routing Profile**         | `Route-to-Retell`           |
    | **Topology Hiding Profile** | `Avaya`                     |

    Click **Finish**.
  </Step>

  <Step title="Configure Session Manager — SIP entities and routing">
    Session Manager configuration is done through **System Manager** (`https://<smgr-ip>/SMGR`). Navigate to **Elements** > **Routing**.

    **1. Add a SIP Entity for the SBCE (if not already present)**

    1. Go to **SIP Entities** > **New**.
    2. Configure:
       * **Name**: `SBCE-Retell-Trunk`
       * **FQDN or IP**: SBCE A1 (internal) IP address — the interface facing Session Manager.
       * **Type**: `SIP Trunk`
       * **Adaptation**: leave blank unless header manipulation is needed.
       * **Location**: select your site location.
    3. Under **Port**, add:
       * **Port**: `5061`, **Protocol**: `TLS`
    4. Save.

    **2. Add an Entity Link between Session Manager and the SBCE**

    1. Go to **Entity Links** > **New**.
    2. Configure:
       * **Name**: `SM-to-SBCE-Retell`
       * **SIP Entity 1**: your Session Manager instance.
       * **Protocol**: `TLS`
       * **Port**: `5061`
       * **SIP Entity 2**: `SBCE-Retell-Trunk`.
       * **Connection Policy**: `Trusted`.
    3. Save.

    **3. Create a Routing Policy for Retell**

    1. Go to **Routing Policies** > **New**.
    2. Configure:
       * **Name**: `Route-via-Retell`
       * **SIP Entity**: `SBCE-Retell-Trunk`.
       * **Retries**: `1`.
    3. Save.

    **4. Configure Dial Patterns (Incoming Call Route)**

    Dial patterns map DIDs arriving from Retell to the correct destination within Aura (e.g., a Communication Manager trunk group or a SIP entity).

    1. Go to **Dial Patterns** > **New**.
    2. Configure:
       * **Pattern**: your DID or DID range (e.g., `+1212555` prefix or full E.164 `+12125551234`).
       * **Min / Max**: set to match the digit length.
       * **SIP Domain**: your internal SIP domain.
       * **Routing Policy**: `Route-via-Retell` (for outbound to Retell) or a policy pointing to Communication Manager (for inbound).
    3. Save and **Commit** the routing configuration.
  </Step>

  <Step title="Configure Communication Manager — trunk group and incoming route">
    Communication Manager configuration is accessed through **System Manager** > **Elements** > **Communication Manager** > **Launch Element Manager**, or directly via SAT terminal.

    **1. Create a SIP Signaling Group**

    1. In CM Element Manager, go to **Telephony** > **Trunks** > **Signaling Groups** > **Add**.
    2. Configure:
       * **Group Type**: `sip`
       * **Transport Method**: `tcp` (or `tls`)
       * **Near-end Node Name**: the CM node name for your processor interface.
       * **Near-end Listen Port**: `5060`
       * **Far-end Node Name**: the node name for Session Manager (defined under **IP Node Names**).
       * **Far-end Listen Port**: `5060`
       * **Far-end Network Region**: assign to the network region used for Retell traffic.
       * **Direct IP-IP Audio Connections**: `y` (enables media hairpinning bypass where possible).
       * **DTMF over IP**: `rtp-payload` (RFC 2833 — compatible with Retell).
    3. Save.

    **2. Create a Trunk Group**

    1. Go to **Telephony** > **Trunks** > **Trunk Groups** > **Add**.
    2. Configure:
       * **Group Type**: `sip`
       * **Group Name**: `Retell-Trunk`
       * **COR**: assign an appropriate Class of Restriction.
       * **Signaling Group**: the signaling group number from the previous step.
       * **Number of Members**: set to your expected max concurrent calls.
    3. Under the **Trunk Parameters** tab:
       * **Codecs**: ensure PCMU/PCMA is included (CM negotiates via SDP with Session Manager).
       * **Incoming Destination**: leave blank or set to a default VDN/extension.
    4. Save.

    **3. Configure Incoming Call Handling (Incoming Call Route)**

    Map inbound DIDs to extensions, VDNs, or hunt groups in CM.

    1. Go to **Telephony** > **Trunks** > **Trunk Groups** > select your trunk group > **Incoming Call Handling**.
    2. Add entries mapping your DID numbers:
       * **Trunk Group**: your Retell trunk group number.
       * **Incoming Destination**: the VDN, extension, or hunt group to ring.
       * **Delete Digits / Insert**: modify dialed digits if your routing requires digit translation.
    3. Save.

    **4. Configure Outbound Routing (AAR / ARS)**

    To route outbound calls from CM agents through the Retell trunk:

    1. Go to **Routing** > **AAR Analysis** (for internal routing) or **ARS Analysis** (for PSTN routing).
    2. Add a route pattern:
       * **Route Pattern**: assign a route pattern number.
       * **Grp No**: your Retell trunk group number.
       * **FRL**: set the Facility Restriction Level.
       * **Numbering Format**: `public` (E.164).
    3. Update your **Dial Plan** and **ARS Digit Analysis** to steer calls to this route pattern.
    4. Save.
  </Step>

  <Step title="TLS and SRTP with Retell">
    Use TLS and SRTP when end-to-end signaling and media encryption is required between Retell and the Avaya SBCE.

    **How it works:**

    * **TLS** encrypts the SIP signaling between Retell and the SBCE A-side.
    * **SRTP** encrypts the RTP media. Retell requires TLS transport to enable SRTP.
    * The SBCE terminates TLS on the A-side and can re-encrypt or pass media unencrypted on the B-side (toward Session Manager), depending on your internal security policy.

    **Retell SIP URI for TLS:**

    ```
    sip:sip.retellai.com;transport=tls
    ```

    Use this URI when configuring the Server Configuration on SBCE (port `5061`) and when importing the number into Retell.

    **SBCE TLS configuration:**

    1. In the SBCE EMS, go to **TLS Management** > **Certificates**.
    2. Import the Retell Root CA and intermediate CA so the SBCE trusts Retell's certificate during TLS handshake:
       * **Root CA**: Amazon Root CA 1 — download `AmazonRootCA1.pem` from the [Amazon Trust Services repository](https://www.amazontrust.com/repository/).
       * **Intermediate CA**: `C=US, O=Amazon, CN=Amazon RSA 2048 M01` — also available at the [Amazon Trust Services repository](https://www.amazontrust.com/repository/). Install this if the SBCE requires the full chain for validation.
    3. In **Signaling Interface** (`Service-Provider-External`), ensure **TLS Port** `5061` is configured.
    4. In **Server Configuration** (`Service-Provider`), set **Transport** to `TLS` and **Port** to `5061`.
    5. Under **Security Rules** (used in the End Point Policy Group), create or select a rule that enforces:
       * **TLS**: enabled.
       * **SRTP**: enabled.
       * **SRTP Cipher**: `AES_CM_128_HMAC_SHA1_80` (recommended and compatible with Retell).

    **Session Manager TLS (internal leg):**

    If you also want TLS between the SBCE B-side and Session Manager, update the Entity Link to use `TLS` on port `5061` and ensure the Session Manager identity certificate is trusted by the SBCE.

    **Firewall additions for TLS/SRTP:**

    | Protocol | Port       | Purpose                                |
    | -------- | ---------- | -------------------------------------- |
    | TCP      | 5061       | SIP/TLS between Retell and SBCE A-side |
    | UDP      | 1024–65535 | SRTP media (same port range as RTP)    |
  </Step>

  <Step title="Save and commit configuration">
    Changes across the Aura stack must be saved and committed before they take effect.

    **SBCE:**

    * Most changes in the SBCE EMS take effect immediately on save, but verify under **Monitoring** > **Server Status** that the SBCE service is active and no alarms are raised.

    **Session Manager / System Manager:**

    1. In System Manager, after saving routing changes (SIP Entities, Entity Links, Dial Patterns, Routing Policies), click **Commit** in the top-right of the Routing module.
    2. Confirm the commit succeeds and Session Manager shows the updated routing in **Elements** > **Session Manager** > **System Status**.

    **Communication Manager:**

    1. If using SAT terminal: type `save translation` to persist changes.
    2. If using CM Element Manager: changes are saved per screen — verify all trunk group and signaling group entries show the expected values.
    3. For busy production systems, schedule a maintenance window before committing trunk group changes.
  </Step>

  <Step title="Import the number into Retell">
    After the Avaya side is configured, import the phone number into Retell so Retell knows which trunk to use for outbound calls and which agent to assign for inbound.

    1. In the Retell dashboard, go to **Phone Numbers** > **Import Number**.
    2. Fill in:
       * **Phone Number**: E.164 format (e.g., `+12137771234`).
       * **Termination SIP URI**: The SBCE A-side IP or FQDN with port (e.g., `<sbce-public-ip>:5060` or `<sbce-fqdn>:5060`). For TLS: `<sbce-fqdn>:5061` and ensure you use `sip:...;transport=tls`.
       * **SIP Username / Password**: Only required if you enabled inbound digest authentication on the SBCE End Point Policy Group.
    3. Save the number and assign a Retell agent to it.

    You can also import numbers programmatically via the [Import Number API](/api-references/import-phone-number).

    Once imported, the number works for both inbound and outbound calls — see [Make Outbound Calls](/deploy/outbound-call) and [Receive Inbound Calls](/deploy/inbound-call).

    To pass metadata such as a caller or account ID between Avaya and your agent, use [custom SIP headers](/build/telephony/sip-headers).
  </Step>

  <Step title="Test the SIP trunk">
    **Inbound test (external call → Avaya DID → Retell agent):**

    1. Call the DID from an external phone.
    2. Verify the call traverses: PSTN → CM → SM → SBCE → Retell.
    3. Confirm the call appears in Retell's **Calls** dashboard and the correct agent handles it.
    4. Verify bidirectional audio and clean call termination.

    **Outbound test (Retell → SBCE → CM → PSTN):**

    1. Place an outbound call via the Retell dashboard or [Create Phone Call API](/api-references/create-phone-call) using the imported number.
    2. Confirm the receiving party sees the correct caller ID (E.164 format).
    3. Verify audio quality in both directions.

    **SIP OPTIONS ping test:**
    If you enabled heartbeat/OPTIONS on the SBCE Server Configuration, check **SBCE EMS** > **Monitoring** > **Server Status** to confirm the trunk shows as **Active** based on OPTIONS responses from `sip.retellai.com`.
  </Step>

  <Step title="Debug call issues">
    For Retell-side diagnostics, see [debug outbound connection issues](/reliability/debug-outbound-call) for `not_connected` failures and disconnection reasons, and [debug SIP calls using PCAP files](/reliability/debug-calls-pcap) to analyze signaling and media in Wireshark.

    **1. SBCE Trace (SIP and Media)**

    The SBCE provides packet-level and SIP-level traces via the EMS.

    * Go to **Monitoring** > **Incidents** for active alarms and recent error events.
    * Go to **Monitoring** > **Tracing** > **Packet Capture** to capture SIP and RTP packets on the A-side interface during a test call.
    * Go to **Monitoring** > **Tracing** > **Call Trace** (if available on your SBCE release) to see SIP message flow per call.

    Look for:

    * `403 Forbidden` → Retell IP not in SBCE's trusted source list. Check **End-Point Policy Group** and **Server Flow** source matching.
    * `480 Temporarily Unavailable` / `503` → SBCE cannot reach `sip.retellai.com`. Verify DNS resolution and firewall on port 5060/5061.
    * `488 Not Acceptable Here` → Codec mismatch in SDP. Ensure PCMU or PCMA is in the SBCE Media Rule.
    * No audio / one-way audio → RTP being blocked. Check SBCE media port range firewall rules and verify SDP `c=` lines in the trace show the correct SBCE A-side IP (topology hiding should replace internal IPs).

    **2. Session Manager Logs**

    In System Manager, go to **Elements** > **Session Manager** > **System Status** > **Call Routing Test** to simulate a call and verify dial pattern matching.

    Check SM logs under **Elements** > **Session Manager** > **System Logs** for SIP routing rejections or entity link failures.

    **3. Communication Manager**

    On CM, use the SAT commands:

    * `list trace tac <tac>` — trace calls on a specific trunk access code.
    * `status trunk <group>/<member>` — check the state of individual trunk members.
    * `list measurements trunk-group <group> last-hour` — view call volume and failure stats per trunk group.

    **4. Common Issues**

    | Symptom                    | Likely Cause                            | Fix                                                                 |
    | -------------------------- | --------------------------------------- | ------------------------------------------------------------------- |
    | 403 on inbound from Retell | SBCE rejecting Retell IP                | Add Retell CIDRs to SBCE trusted source / Server Flow               |
    | 503 to Retell              | DNS failure or firewall blocking egress | Check DNS for `sip.retellai.com`, open port 5060 outbound           |
    | No audio                   | RTP port range blocked                  | Open UDP media ports on perimeter firewall and SBCE                 |
    | One-way audio              | NAT / topology hiding issue             | Verify SBCE A-side IP is in SDP `c=`; check topology hiding profile |
    | 488 codec mismatch         | No shared codec in SDP                  | Add PCMU/PCMA to SBCE Media Rule                                    |
    | TLS handshake failure      | Missing CA cert on SBCE                 | Import Amazon Root CA 1 and intermediate into SBCE TLS store        |
    | Trunk shows Down           | Options ping failing                    | Check firewall allows SIP OPTIONS from SBCE to Retell IPs           |
    | Calls drop at \~30s        | SIP re-INVITE blocked mid-call          | Allow mid-call SIP signaling through firewall                       |

    <Note>
      If you cannot resolve the issue, collect the SBCE packet capture, the SM call routing test result, the CM trace, and the Retell Call ID from the Retell dashboard, and send them to us on the <a href="https://support.retellai.com/">Customer Support Portal</a> or [support@retellai.com](mailto:support@retellai.com).
    </Note>
  </Step>
</Steps>
