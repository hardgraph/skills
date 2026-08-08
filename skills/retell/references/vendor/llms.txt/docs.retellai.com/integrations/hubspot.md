> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Connect HubSpot

> Connect HubSpot to Retell with a private app access token to sync contacts both ways, map post-call analysis to properties, and log calls to timelines.

Connecting HubSpot lets Retell import your contacts, enrich them with [post-call analysis](/features/post-call-analysis-overview) results, push property updates back, and log every call and chat on the contact's activity timeline. This page covers the HubSpot-side setup and the token Retell needs.

<Note>
  This is the setup guide for using HubSpot as a **CRM data source**. If you want to trigger outbound calls *from* HubSpot workflows instead, see the [HubSpot Marketplace app](/integrations/hubspot-marketplace). The two are independent and can be used together.
</Note>

## When to use it

Connect HubSpot when it's your system of record and you want your agents working from it without anyone copying data between tools. It's the right choice when you want to:

* **Call or text people who already exist in HubSpot.** Contacts sync into Retell automatically, so your agent greets callers by name and knows their deal stage or lifecycle stage instead of asking.
* **Keep HubSpot current without manual data entry.** Analysis results from each conversation write back to contact properties.
* **Give your team call history where they already work.** Each call and chat appears on the contact's activity timeline with its summary and duration.

## Prerequisites

* A HubSpot account with **Super Admin** permissions. Only a super admin can create a private app and grant it scopes.

## Step 1: Create a private app

<Steps>
  <Step title="Open the app list">
    In HubSpot, click **Development** at the bottom of the left sidebar, then select **Legacy Apps**. Click **Create legacy app** in the top-right corner, then choose **Private** in the dialog.

    <Frame caption="HubSpot's Development section with Legacy Apps selected and the Create legacy app button in the top-right corner.">
      <img src="https://mintcdn.com/retellai/M9dkUq1iShh05WLW/images/integration/hubspot-new-app.png?fit=max&auto=format&n=M9dkUq1iShh05WLW&q=85&s=340314861d186ce1cdbeb9151bcea4b7" alt="The HubSpot Legacy Apps page. Development is highlighted at the bottom of the left sidebar, Legacy Apps is highlighted in the Development sub-navigation, and the orange Create legacy app button is highlighted at the top right. The empty state reads No legacy apps available, with a note encouraging developers to build on the newer projects platform." width="3456" height="1828" data-path="images/integration/hubspot-new-app.png" />
    </Frame>

    <Note>
      HubSpot renamed the private apps section to **Legacy Apps** and is steering new development toward its projects platform. Private apps still work and remain the supported way to connect HubSpot to Retell. Depending on your account you may reach the same screen through **Settings > Integrations > Legacy apps**, or see **Private Apps** as its own sidebar item.
    </Note>
  </Step>

  <Step title="Fill in the basic info">
    On the **Basic Info** tab, enter:

    * **Name** — a descriptive name, for example `Retell AI Integration`.
    * **Description** — optional, for example "Syncs contacts and logs call activity for Retell AI".
  </Step>
</Steps>

## Step 2: Grant scopes

On the **Scopes** tab, click **Add new scope**, search for each scope below in **Find a scope**, check it, then click **Update**.

Retell needs three scopes:

| Scope                        | Required for                                                             |
| ---------------------------- | ------------------------------------------------------------------------ |
| `crm.objects.contacts.read`  | Importing contacts, and the connection test Retell runs when you connect |
| `crm.schemas.contacts.read`  | Reading contact property definitions so you can map fields               |
| `crm.objects.contacts.write` | Outbound sync, and logging calls and chats to the timeline               |

<Warning>
  A missing scope doesn't fail the connection test or flag the connection as broken, because Retell only treats rejected credentials as a connection error. The affected feature just silently stops working, so grant the correct scope up front.
</Warning>

## Step 3: Generate the access token

<Steps>
  <Step title="Create the app">
    Click **Create app** in the top-right corner. Review the scopes in the confirmation dialog and click **Continue creating**.
  </Step>

  <Step title="Copy the token">
    Click **Show token**, then copy it.

    <Warning>
      This is the only time HubSpot displays the full token. Store it somewhere safe. You can rotate it later, but rotating invalidates the old token immediately.
    </Warning>
  </Step>
</Steps>

## Step 4: Connect HubSpot in Retell

<Steps>
  <Step title="Add the connection">
    In the Retell Dashboard, open **Integrations**, select the **Available** tab, find **HubSpot**, and click **+ Add Account**.
  </Step>

  <Step title="Enter the token">
    Fill in the fields:

    | Field               | Value                                     |
    | ------------------- | ----------------------------------------- |
    | **Connection name** | Alias for this connection.                |
    | **Access Token**    | The private app access token from Step 3. |

    <Frame caption="The HubSpot connection dialog on the Integrations page, with fields for connection name and access token.">
      <img src="https://mintcdn.com/retellai/M9dkUq1iShh05WLW/images/integration/hubspot-config-integration.png?fit=max&auto=format&n=M9dkUq1iShh05WLW&q=85&s=6250b19fd8ade95e11418b97c31c90f1" alt="The Retell Dashboard Integrations page on the Available tab, showing the HubSpot card with an Add Account button and a Connected badge. An open dialog titled Hubspot has a Connection name field with the placeholder Alias for connection and an Access Token field with the placeholder Enter your access token, below a hint reading Need help finding access tokens? See docs." width="2878" height="1042" data-path="images/integration/hubspot-config-integration.png" />
    </Frame>
  </Step>

  <Step title="Connect and confirm">
    Click **Connect**. Retell creates the connection and immediately tests it by reading a page of contacts from HubSpot.

    On success the dialog reports the connection as verified and offers **Set up contact sync**. On failure it shows HubSpot's own error and re-enables the field so you can paste a corrected token.
  </Step>

  <Step title="Set up contact sync">
    Click **Set up contact sync** to open the field mapping dialog, then map the HubSpot properties you want to import and the Retell fields you want to write back.

    Retell pre-fills one mapping in each direction: HubSpot `phone` to Retell `phone_number`. Phone number is how contacts are matched between the two systems, so it stays mapped and can't be removed. See [CRM data mappings](/integrations/crm-mappings) for how to map the rest, create custom fields, and choose update modes.

    To log conversations to HubSpot, turn on **Log activities automatically** on the **Sync to HubSpot** tab.
  </Step>
</Steps>

## Verify it worked

* On the **Connected** tab, the HubSpot connection shows as connected.
* Open **Contacts**. After the first sync, HubSpot contacts appear with correctly formatted phone numbers and your mapped fields populated.
* The first sync is a full scan of every contact that has the mapped phone property, so a large portal takes a while. After that, Retell polls every 5 minutes and imports only contacts modified since the last run.

<Note>
  Contacts with no value in the mapped phone property are excluded from the sync entirely, as are contacts whose phone number can't be parsed into a valid E.164 number. Fix or remove malformed numbers in HubSpot before relying on two-way sync.
</Note>

## Manage the private app

### Rotate the access token

<Steps>
  <Step title="Rotate in HubSpot">
    Go to **Development > Legacy Apps** and click your Retell app's name. Next to the access token, click **Rotate**, then choose how the old token expires:

    * **Rotate and expire later** keeps the old token valid for 7 days. Pick this one. Retell keeps working while you swap the credential over.
    * **Rotate and expire now** kills the old token immediately, so contact sync and activity logging fail until Retell has the new one.
  </Step>

  <Step title="Reconnect in Retell">
    The connection settings dialog shows a saved token masked and read-only, so there's no field to paste a new one into. Delete the connection and create it again with the new token.

    Your Retell contacts survive the delete. Only the stored connection goes away, so you'll need to set up field mappings again.
  </Step>
</Steps>

### Change scopes

Go to **Development > Legacy Apps**, click your Retell app's name, then click **Edit app** in the top-right corner to change its scopes. Adding a scope takes effect without a new token. Removing one stops the corresponding Retell feature working, without flagging the connection as broken.

## Troubleshooting

<AccordionGroup>
  <Accordion title="Connecting fails with an authorization error">
    Confirm you pasted the private app **access token** and not a HubSpot API key or an OAuth client secret. Check that the app has `crm.objects.contacts.read`, which the connection test needs, and that the token hasn't been rotated since you copied it.
  </Accordion>

  <Accordion title="The connection shows an error after working for a while">
    Retell flags a connection as errored when HubSpot rejects the credentials with an HTTP 401. The usual cause is a rotated or deleted token. Delete the connection and reconnect with a current token.
  </Accordion>

  <Accordion title="Sync runs clean but one property never populates">
    A scope or permission error on a single property doesn't fail the sync or flag the connection, because Retell only treats credential rejections as connection errors. Check that the property is mapped on the right tab of the sync settings, and that the app has the write scope if you expect Retell to update it.
  </Accordion>

  <Accordion title="Calls aren't showing up on the HubSpot timeline">
    Activity logging needs three things: **Log activities automatically** enabled on the **Sync to HubSpot** tab of the sync settings, a Retell contact that was imported from this HubSpot connection, and the `crm.objects.contacts.write` scope on your private app. Retell attaches the activity to the contact it matched by phone number, so a call from a number that isn't a synced HubSpot contact is never logged.
  </Accordion>

  <Accordion title="Contacts are missing from Retell after a sync">
    Retell only imports contacts that have a value in the mapped phone property, and skips any whose number can't be parsed into E.164. A contact with a phone number stored in a different property than the one you mapped won't sync. Check which HubSpot property you mapped to `phone_number` in the sync settings.
  </Accordion>
</AccordionGroup>

## FAQ

<AccordionGroup>
  <Accordion title="Does Retell create new contacts in HubSpot?">
    No. Contact sync only updates contacts that already exist in HubSpot, and never deletes them. Contacts that Retell creates on its own, for example from an inbound call from an unknown number, stay in Retell and are not pushed to HubSpot.
  </Accordion>

  <Accordion title="Can I connect more than one HubSpot portal?">
    You can add multiple connections, but only one of them can drive contact sync for your organization at a time. When contact sync is already set up on another connection, **Set up contact sync** is disabled on the new one.
  </Accordion>

  <Accordion title="How are calls and chats represented in HubSpot?">
    A call becomes a **Call** engagement associated with the matched contact, with its status set to `COMPLETED`, plus the duration and direction. A chat becomes a **Communication** object on the SMS channel, since HubSpot has no chat engagement type. Both carry a body with the conversation ID, the from and to numbers, the disconnection reason for calls, and the summary.
  </Accordion>

  <Accordion title="Does this replace the HubSpot Marketplace app?">
    No, they do different jobs. This integration syncs contacts and logs activity. The [Marketplace app](/integrations/hubspot-marketplace) adds a **Make a Phone Call** action to HubSpot workflows so HubSpot can trigger outbound calls. You can run both.
  </Accordion>

  <Accordion title="Can I map custom properties?">
    Yes. Map any HubSpot contact property, including custom ones, to a Retell [custom field](/integrations/crm-mappings#custom-fields). Objects other than contacts, such as deals and companies, are not part of contact sync.
  </Accordion>

  <Accordion title="What happens to my Retell contacts if I disconnect?">
    They stay. Turning contact sync off stops future syncs, and deleting the connection removes the stored credentials. Contacts already imported remain in Retell either way.
  </Accordion>
</AccordionGroup>

## Next steps

<CardGroup cols={2}>
  <Card title="CRM data mappings" icon="arrows-left-right" href="/integrations/crm-mappings">
    Map HubSpot properties to Retell contacts, choose update modes, and control what syncs back.
  </Card>

  <Card title="Build contact memory" icon="brain" href="/integrations/build-contact-memory">
    Accumulate what your agents learn across conversations into the contact record.
  </Card>

  <Card title="Trigger calls from HubSpot" icon="bolt" href="/integrations/hubspot-marketplace">
    Use the Marketplace app to start outbound calls from a HubSpot workflow.
  </Card>

  <Card title="Dynamic variables" icon="code" href="/build/dynamic-variables">
    Reference synced contact fields from your agent's prompt.
  </Card>
</CardGroup>
