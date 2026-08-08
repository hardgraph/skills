> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Send & receive SMS

> Enable SMS on Retell with Twilio numbers to send messages during calls, receive texts and MMS mid-call, and run two-way text conversations with chat agents.

Retell agents can send SMS during calls, receive texts and MMS mid-call, and hold two-way SMS conversations through [chat agents](/build/create-chat-agent). This guide shows how to enable SMS with [Retell Twilio numbers](/deploy/purchase-number) or your own Twilio number.

<Warning>
  SMS is available for Retell Twilio numbers and custom telephony numbers that have passed A2P applications; Telnyx is not supported yet. SMS (A2P 10DLC) is limited to US phone numbers, excluding toll-free numbers — the SMS add-on is disabled for non-US numbers in the dashboard. For other custom telephony providers, check your provider's documentation on enabling SMS.
</Warning>

## Enable SMS capabilities

This step is a prerequisite for sending SMS from your own number during calls and for two-way SMS conversations. Receiving SMS during calls works out of the box for Retell Twilio numbers without this step.

<Tip>
  If you only need to send SMS during calls and want to skip the A2P application, you can send from an **SMS-approved Retell number** instead. See [Option 3](#option-3-use-an-sms-approved-retell-number-no-a2p-required) for details.
</Tip>

### Option 1: Enable SMS for Retell Twilio numbers

Enabling SMS on a Retell Twilio number goes through three application and approval steps:

* Get approved for a business profile (free)
* Get approved for the brand based on the business profile (\$4 one-time application fee for low-volume, \$45 for standard)
* Get approved for an SMS campaign (\$15 one-time application fee)

Once approved, the SMS add-on costs \$20/month per number plus \$0.01 per SMS sent.

<Frame caption="The SMS add-on under Advanced Add-Ons on the phone number page — click it to start the application.">
  <img src="https://mintcdn.com/retellai/5MK89Yey4iR4zlyL/images/sms/sms-application-start.png?fit=max&auto=format&n=5MK89Yey4iR4zlyL&q=85&s=61273ede1af8ca1837e35f43947fa1c3" alt="Phone number detail page in the Retell dashboard showing the Advanced Add-Ons section, with the SMS add-on highlighted" width="1600" height="900" data-path="images/sms/sms-application-start.png" />
</Frame>

The entire application takes around 2-3 weeks, sometimes longer, as it's a manual review process on the telephony provider side. We notify you via email when the application is approved or rejected with the reason. If your application is stuck in pending review for over a month, reach out to support and we will help escalate.

#### Detailed steps

The following video walks through the entire application. The written steps below cover the same flow in detail.

<iframe className="w-full aspect-video rounded-xl" src="https://www.youtube.com/embed/jdCmCMkOdnI" title="Enable SMS on Retell: A2P application walkthrough" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />

<Steps>
  <Step title="Get approved for a business profile">
    You can reuse an existing business profile or create a new one here. Follow the instructions at [Business Profile](/build/telephony/business-profile) to get approved for a business profile.
  </Step>

  <Step title="Select a brand type and get approved for the brand">
    <Warning>Only one brand can be created per business profile and it cannot be changed once created. If you need to change the brand type, you have to create a new business profile.</Warning>

    Here you get to select two types of brands:

    * Low-volume:
      * \$4 one-time application fee
      * send fewer than 6,000 message segments per day to the US (2,000 message segments per day to T-Mobile)
    * Standard:
      * \$45 one-time application fee
      * SMS limit may fall between 6,000 and 400,000 message segments per day to the US (2,000–200,000 per day to T-Mobile)

    Depending on your company's type, you might be asked to fill out additional information.
  </Step>

  <Step title="Get approved for an SMS campaign">
    <Frame caption="The campaign application form: use case, description, sample messages, and end-user consent.">
      <img src="https://mintcdn.com/retellai/aS6kWND7Eh5Z5FzB/images/sms/sms-campaign-application.jpeg?fit=max&auto=format&n=aS6kWND7Eh5Z5FzB&q=85&s=b2f48780b6f307b7de9ff5b392e36fce" alt="Add Campaign Application dialog with fields for application name, use case, SMS description, two sample messages, and how end users consent to receive messages" width="495" height="730" data-path="images/sms/sms-campaign-application.jpeg" />
    </Frame>

    Here you fill out the use case and sample messages you intend to send. See Twilio's [A2P 10DLC Campaign Onboarding Guide](https://help.twilio.com/articles/11847054539547-A2P-10DLC-Campaign-Onboarding-Guide) for how to write an approvable campaign. SMS rules are strict: if your actual traffic doesn't match the sample messages, your number or telephony subaccount might be suspended, so provide answers that match your intended use case. If you ever need to send different SMS messages, you can create a new campaign (see the FAQ below).

    Each campaign submission costs a \$15 fee, which is non-refundable even if the campaign isn't approved.
  </Step>
</Steps>

#### Rejected application FAQ

<AccordionGroup>
  <Accordion title="What are some of the common reasons for application rejection?">
    There are a few common reasons for application rejection:

    * The campaign application did not provide detailed information on the use case and sample messages.
    * The opt-in workflow was not explained clearly.
    * The business profile or brand was not approved, due to missing or incorrect business information.

    Feel free to read more at [Twilio's A2P 10DLC documentation](https://help.twilio.com/articles/15778026827291-Why-Was-My-A2P-10DLC-Campaign-Registration-Rejected-).
  </Accordion>

  <Accordion title="If my application is rejected, what should I do?">
    Please check the rejection reason and update your application accordingly. If you are rejected at a later stage, you might be able to reuse the previous steps. For example, if your business profile and brand are approved, but the SMS campaign is rejected, you can reuse the business profile and brand approval, and create a new SMS campaign application.
  </Accordion>

  <Accordion title="If my application is rejected, will I get a refund?">
    No, you will not get a refund, as the charge is for the telephony provider's manual review process, and that will be charged regardless of the application outcome. The \$15 campaign fee is charged for each submission.
  </Accordion>

  <Accordion title="How to create a new SMS campaign if I want to change my use case for the number?">
    You can delete the SMS capability on the number (this will not delete your approved business profile, brand, or campaign — those can still be reused), and create a new SMS application with a new campaign while reusing the business profile and brand.
  </Accordion>
</AccordionGroup>

### Option 2: Bring your own Twilio number

If you already have a Twilio number with SMS capabilities enabled, you can integrate it with Retell by following these steps:

<Steps>
  <Step title="Open Setup SMS Function">
    Navigate to your phone number settings in the Retell dashboard and click **Setup SMS Function** under **Advanced Add-Ons**.
  </Step>

  <Step title="Provide your Twilio credentials">
    Enter the following information from your Twilio account:

    * **Account SID**: your unique Twilio account identifier
    * **Twilio Auth Token**: your Twilio authentication token for API access

    <Info>You can find these credentials in your Twilio Console under Account Info.</Info>

    <Frame caption="The Setup SMS Function dialog asking for the Twilio Account SID and Auth Token.">
      <img src="https://mintcdn.com/retellai/_y-iu8M6jQ67ZrqV/images/custom-twilio.png?fit=max&auto=format&n=_y-iu8M6jQ67ZrqV&q=85&s=4963b87c5017b0a13c2e7748e4ccdb3c" alt="Setup SMS Function dialog with SMS Provider set to Twilio and empty fields for Account SID and Twilio Auth Token" width="1062" height="1046" data-path="images/custom-twilio.png" />
    </Frame>
  </Step>

  <Step title="Start using SMS">
    Once your credentials are verified, your Twilio number is integrated with Retell's SMS capabilities: sending SMS during calls and two-way SMS conversations.

    If you use two-way SMS, also update your Twilio Messaging Service so incoming messages are handled by the number's own webhook (the `useInboundWebhookOnNumber` setting) — otherwise inbound texts won't reach Retell.
  </Step>
</Steps>

<Note>
  Make sure your Twilio number already has SMS capabilities enabled and is compliant with SMS regulations before integration. For more information about A2P 10DLC compliance, see [Twilio's A2P 10DLC documentation](https://www.twilio.com/docs/proxy/flex-a2p-10dlc).
</Note>

### Option 3: Use an SMS-approved Retell number (no A2P required)

If you only need to send SMS during active phone calls and want to skip the A2P application entirely, you can send from Retell's **pool of SMS-approved numbers**. No setup or approval steps are needed — select this option when configuring your SMS node or tool, and you can start sending right away. A number from the pool is automatically selected for each call, and all SMS sent within the same call use the same number.

<Warning>
  When sending from an SMS-approved Retell number, the message content is a **preset template provided by Retell** — you cannot customize the text or use a prompt. This option is for in-call SMS only and does not support two-way SMS conversations.
</Warning>

<iframe className="w-full aspect-video rounded-xl" src="https://www.youtube.com/embed/jJus-KuWMyc" title="How to send SMS from Retell numbers" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />

## Send SMS during call

You can configure agents to send SMS messages to the user during an active phone call. Refer to the following docs for setup details:

* For Conversation Flow: [SMS Node](/build/conversation-flow/sms-node).
* For Single/Multi Prompt: [Send SMS](/build/single-multi-prompt/send-sms).

## Receive SMS during call

Agents can receive and understand SMS messages during an active phone call — even if the agent has not sent any SMS. This lets users send supplementary information mid-conversation, such as a photo of a document, a screenshot, or a reference number.

For Retell Twilio numbers, this works out of the box without enabling SMS. For custom telephony numbers that have passed A2P applications, this is also supported.

**Multimedia (MMS)** is supported in addition to plain text: users can send images, audio, and video as carrier-supported MMS attachments. Whatever is delivered to Retell as part of the message is incorporated into the ongoing voice conversation so the agent can respond with that context.

<Frame caption="A call transcript where the user texted a photo mid-call and the agent describes what it received.">
  <img src="https://mintcdn.com/retellai/aS6kWND7Eh5Z5FzB/images/sms/receive-sms-example.png?fit=max&auto=format&n=aS6kWND7Eh5Z5FzB&q=85&s=abb4a12ef299ea6e2e3e3a8b7b638184" alt="Call transcript showing the agent asking for a photo of a car collision, followed by an In-Call SMS entry with the received image link and the agent's detailed description of the damage" width="1164" height="582" data-path="images/sms/receive-sms-example.png" />
</Frame>

## Set up two-way SMS conversation

Once SMS capability is enabled for the number, you can set up a two-way SMS conversation by attaching [chat agents](/build/create-chat-agent) to the number, so it can receive SMS and reply to the user.

<Frame caption="The Inbound SMS agent and Outbound SMS agent selectors on the phone number page, shown once the SMS add-on is enabled.">
  <img src="https://mintcdn.com/retellai/5MK89Yey4iR4zlyL/images/sms/sms-set-agent.png?fit=max&auto=format&n=5MK89Yey4iR4zlyL&q=85&s=190e4217b0727596baeaef81f3b25ab2" alt="Phone number detail page with the Inbound SMS agent and Outbound SMS agent dropdown sections highlighted below the call agent fields, and the SMS add-on marked as Added under Advanced Add-Ons" width="1600" height="900" data-path="images/sms/sms-set-agent.png" />
</Frame>

Once the chat agent is attached, inbound SMS starts working immediately. Use the inbound webhook to filter and add context to inbound SMS — read more at [Inbound Webhook](/features/inbound-call-webhook).

For outbound SMS, click **Make an outbound SMS** in the dashboard, or call the [Create Outbound SMS API](/api-references/create-sms-chat) to send it programmatically.

## FAQ

<AccordionGroup>
  <Accordion title="Do I need to complete the A2P application just to receive SMS during calls?">
    No. Receiving SMS and MMS during calls works out of the box for Retell Twilio numbers, with no A2P application. The A2P application is required to send SMS from your own number and to run two-way SMS conversations.
  </Accordion>

  <Accordion title="Is Telnyx supported for SMS?">
    Not yet. SMS is available for Retell Twilio numbers and custom telephony numbers that have passed A2P applications.
  </Accordion>

  <Accordion title="Can I send SMS from a toll-free number?">
    No. SMS (A2P 10DLC) is limited to US phone numbers and excludes toll-free numbers, so the SMS add-on is disabled for non-US and toll-free numbers.
  </Accordion>

  <Accordion title="Can I send a custom message without going through A2P?">
    No. If you skip A2P and send from an SMS-approved Retell number, the content is a preset template provided by Retell that you can't customize, and it supports in-call SMS only. Custom message content requires enabling SMS on your own number.
  </Accordion>

  <Accordion title="How long does the A2P application take?">
    Around 2-3 weeks, sometimes longer, since it's a manual review on the telephony provider side. You're notified by email when it's approved or rejected. If it's stuck in pending review for over a month, reach out to support to escalate.
  </Accordion>
</AccordionGroup>
