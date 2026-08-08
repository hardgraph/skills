> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Trigger Retell calls from HubSpot workflows

> Install and configure the Retell AI HubSpot app to trigger outbound voice agent calls from HubSpot workflows, pass contact context, and sync call results back.

This guide provides instructions for setting up and using the **Retell AI** application within **HubSpot** to automate outbound phone calls using voice agents.

<Card title="Install Retell AI for HubSpot" icon="download" href="https://app.hubspot.com/marketplace/46771873/listing/retell-ai">
  Open the Retell AI integration in the HubSpot Marketplace.
</Card>

<Note>
  This app lets HubSpot trigger calls. If you want the reverse, HubSpot contacts synced into Retell and call activity written back to the timeline, set up the [HubSpot CRM integration](/integrations/hubspot) instead. The two are independent and can be used together.
</Note>

## Overview

The Retell AI application enables the **Make a Phone Call** action in HubSpot workflows. This action creates an outbound call using your AI agents and pauses the workflow until the call is finished.

Once a call is completed, HubSpot is automatically updated with:

* **Activity Timeline**: Post-call analysis and call summary
* **Call Log**: Recording and detailed call transcript
* **Company Record**: Call logs also appear on the associated company timeline

## Installing the Application

<Steps>
  <Step title="Initiate installation">
    Click **Connect app** when prompted during installation.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-1.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=abb4331c5b2a38448b53c67b7f6e6b31" alt="HubSpot app installation prompt" width="711" height="844" data-path="images/hubspot/screenshot-1.png" />
    </Frame>
  </Step>

  <Step title="Complete external integration form">
    You will be redirected to an external integration form.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-2.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=60146aa7f2c5e5a606c210fe6c65d0cf" alt="External integration form" width="561" height="596" data-path="images/hubspot/screenshot-2.png" />
    </Frame>
  </Step>

  <Step title="Sign up for Retell AI">
    Sign up on the Retell AI website to access your dashboard if you don't already have an account.
  </Step>

  <Step title="Get your API key">
    Navigate to **Settings → API Keys** in the Retell AI Dashboard.

    Copy the **Secret Key (Webhook)** and paste it into the **Retell API Key** field on the installation form.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-3.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=d99d420008b2660543881b892bfe8706" alt="Retell AI API Keys settings" width="1263" height="405" data-path="images/hubspot/screenshot-3.png" />
    </Frame>
  </Step>

  <Step title="Configure webhook URL">
    Copy the **Webhook URL** provided in the form and paste it into the Webhooks section of your Retell Dashboard (**Settings → Webhooks**).

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-4.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=eec6e4b6ea6b6084202f6aab4b2292e0" alt="Retell AI Webhooks settings" width="1263" height="405" data-path="images/hubspot/screenshot-4.png" />
    </Frame>
  </Step>

  <Step title="Save and return to HubSpot">
    Click **Save** to submit the form, then close the page and return to HubSpot.
  </Step>
</Steps>

## Using the Application

HubSpot workflows allow you to automatically trigger outbound calls based on various events. Common use cases include:

* **New lead qualification**: Call leads immediately after they submit a form to qualify interest
* **New contact created**: Reach out to new contacts added to your CRM
* **Deal stage changes**: Follow up when a deal moves to a specific stage
* **Re-engagement**: Call contacts who haven't been active for a set period
* **Appointment reminders**: Confirm upcoming meetings or demos
* **Post-purchase follow-up**: Check in with customers after a purchase

<Note>
  You must have a Retell AI account and an agent with a connected phone number.
</Note>

### Step 1: Creating the HubSpot Workflow

<Steps>
  <Step title="Create a new workflow">
    Navigate to **Automation → Workflows** in HubSpot.

    Create a new workflow and choose your trigger based on your use case:

    * **Form submission**: Trigger when a lead fills out a specific form
    * **Record created**: Trigger when a new contact is added to your CRM
    * **Property value change**: Trigger when a deal stage or lead status changes
    * **Date-based**: Trigger based on a specific date property (e.g., appointment date)

    For this example, set the trigger to **Data Values → Record Created**.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-5.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=5a3a41151702c5ed972f9a95c2f74d7d" alt="Create workflow with Record Created trigger" width="1064" height="625" data-path="images/hubspot/screenshot-5.png" />
    </Frame>
  </Step>

  <Step title="Add phone number condition">
    Add a condition for **Phone number is known**. This ensures the workflow only triggers for contacts with valid phone numbers, preventing failed call attempts.

    You can also add additional conditions to further qualify which contacts receive calls:

    * **Lead status**: Only call contacts with a specific lead status
    * **Lifecycle stage**: Target contacts at a particular stage (e.g., "Lead" or "Marketing Qualified Lead")
    * **Contact owner**: Route calls based on the assigned sales rep
    * **Custom properties**: Filter based on your business-specific criteria

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-6.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=8131c0db66a5767eeb1c1d82a20029eb" alt="Add phone number condition" width="1064" height="625" data-path="images/hubspot/screenshot-6.png" />
    </Frame>

    The final trigger should look like the following:

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-7.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=e69ae4738ab35cc92be78828f3022c00" alt="Phone number condition configuration" width="1064" height="782" data-path="images/hubspot/screenshot-7.png" />
    </Frame>
  </Step>

  <Step title="Add Retell AI action">
    Click the **(+)** button to add an action. Select **Retell AI → Make a Phone Call** under "Integrated apps".

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-8.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=f1b9e51bdaf3523c9d72b8454786ccba" alt="Add Retell AI Make a Phone Call action" width="399" height="193" data-path="images/hubspot/screenshot-8.png" />
    </Frame>
  </Step>

  <Step title="Configure the call form">
    Configure the call form with the following settings:

    * **From**: Select the Retell AI agent/phone number
    * **To**: Select the contact's phone number token
    * **Dynamic Variables** (Optional): Pass data like the contact's name using JSON format. Ensure all values are surrounded by quotes.

    <Frame caption="The call form configuration dialog showing all available fields">
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-9.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=011d1de0f206f6a1efc17641b619d6a3" alt="Call form configuration" width="411" height="464" data-path="images/hubspot/screenshot-9.png" />
    </Frame>

    <br />

    <Frame caption="Selecting the Retell AI agent/phone number from the From dropdown">
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-10.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=b675344808a66560c010654be40adf76" alt="From field selection" width="411" height="464" data-path="images/hubspot/screenshot-10.png" />
    </Frame>

    <br />

    <Frame caption="Selecting the contact's phone number token for the To field">
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-11.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=65fa530df479e2543e4ad21d03cc8081" alt="To field selection" width="735" height="464" data-path="images/hubspot/screenshot-11.png" />
    </Frame>

    <br />

    <Frame caption="The Dynamic Variables field where you can pass JSON data to your agent">
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-12.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=8ca35ef82564cfd4f2b56dbd6c942f23" alt="Dynamic variables configuration" width="735" height="464" data-path="images/hubspot/screenshot-12.png" />
    </Frame>

    Click **Save**.
  </Step>

  <Step title="Add subsequent actions based on call outcome">
    After the call completes, you can branch your workflow based on the call outcome to automate follow-up actions.

    Use the **Call Success** output from the Retell AI action to create branches:

    **If call was successful:**

    * Send a follow-up email with next steps
    * Create a task for the sales rep to review the call
    * Update the contact's lifecycle stage
    * Add the contact to a nurture sequence

    **If call was unsuccessful** (no answer, voicemail, etc.):

    * Schedule a retry call for a later time
    * Send an SMS or email as an alternative touchpoint
    * Add to a "needs follow-up" list

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-14.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=57353adb745cecf37c2ce397c11bbec9" alt="Branching workflow based on call outcome" width="2698" height="1536" data-path="images/hubspot/screenshot-14.png" />
    </Frame>

    <Tip>
      You can also use other call outputs like **User Sentiment** or **Call Outcome** to create more granular branching logic.
    </Tip>
  </Step>

  <Step title="Publish the workflow">
    Review and click **Review and publish** to activate.
  </Step>
</Steps>

### Step 2: Viewing Call Results in HubSpot

After a contact is enrolled in the workflow and the call completes, you can view the results directly in HubSpot.

<Steps>
  <Step title="Open the contact record">
    Navigate to **CRM → Contacts** and open the contact that was enrolled in the workflow.

    <Tip>
      You can also find recently called contacts by filtering the contact list by the workflow enrollment date or checking the workflow history.
    </Tip>

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-15.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=9f000437e847f5767d292338b12596f8" alt="Activity tab with filters" width="1265" height="647" data-path="images/hubspot/screenshot-15.png" />
    </Frame>
  </Step>

  <Step title="View call analysis">
    Check the contact's **Activity** tab to view the **Call Analysis**.

    <Note>
      Ensure your activity filters include "Retell AI" as shown below:
    </Note>

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-16.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=c99b3cf14f514d0f981aae7cd902cf2a" alt="Activity filters including Retell AI" width="1265" height="816" data-path="images/hubspot/screenshot-16.png" />
    </Frame>

    Each call displays two types of analysis data:

    **Default Call Results** — Automatically generated for every call:

    * Summary
    * Duration
    * Voicemail detection
    * User Sentiment
    * Call Outcome

    **Custom Analysis** — Additional insights you configure in Retell AI using [Post-Call Analysis](/features/post-call-analysis-overview):

    * Lead qualification status
    * Custom scoring metrics
    * Business-specific data extraction
    * Any other fields you define

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-17.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=ff083d484c507ccd15bb92f0bb43f222" alt="Call Analysis in Activity tab" width="765" height="466" data-path="images/hubspot/screenshot-17.png" />
    </Frame>
  </Step>

  <Step title="View call log and recording">
    Check the **Calls** tab to view the full **Call Log** and recording.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-18.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=93671edd7a38cc92f9a8d3671ea65722" alt="Call Log with recording" width="781" height="369" data-path="images/hubspot/screenshot-18.png" />
    </Frame>
  </Step>
</Steps>

## Uninstalling the Application

<Steps>
  <Step title="Navigate to Connected Apps">
    Go to **Connected Apps** and select **Retell AI**.
  </Step>

  <Step title="Access General Settings">
    Navigate to the **General Settings** tab.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-19.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=1512539bdda8965b043e62a9bc74e81a" alt="Retell AI General Settings tab" width="1137" height="539" data-path="images/hubspot/screenshot-19.png" />
    </Frame>
  </Step>

  <Step title="Uninstall the application">
    Click **Uninstall**.

    <Frame>
      <img src="https://mintcdn.com/retellai/gW-1SjSS0hmYoa7S/images/hubspot/screenshot-20.png?fit=max&auto=format&n=gW-1SjSS0hmYoa7S&q=85&s=7b58a7a700e13baa0a46d873aabc3e26" alt="Uninstall Retell AI application" width="1137" height="539" data-path="images/hubspot/screenshot-20.png" />
    </Frame>

    <Warning>
      Your data will be deleted from Retell AI records and the app will be removed from HubSpot.
    </Warning>
  </Step>
</Steps>
