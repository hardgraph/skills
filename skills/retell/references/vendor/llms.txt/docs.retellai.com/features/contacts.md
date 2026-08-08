> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Contacts

> Manage the people your Retell agents call and text: contact records keyed by phone number, custom contact fields, post-call data mappings, and CRM sync.

Contacts is your workspace's record of the people your agents talk to. Each contact is keyed by phone number and collects the conversations you've had with that person, along with any fields you choose to store about them. Those fields reach the agent as [dynamic variables](/build/dynamic-variables) on the next phone call, so it can open with what it already knows instead of asking again.

Open it from the **Contacts** tab under **Data** in the dashboard.

<Frame caption="The Contacts page, showing each contact's conversation count and last conversation.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/contacts-list.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=28bf12cfc0e8f089b81e62a819da38d2" alt="Contacts page in the Retell dashboard showing a table of contacts with columns for phone number, first name, last name, related conversations, latest conversation, do not call, external ID, and a custom email field. Custom field columns continue off to the right." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/contacts/contacts-list.png" />
  </div>
</Frame>

## When to use it

* **Your CRM is the source of truth.** Sync contacts in from [Salesforce or HubSpot](/integrations/crm-overview) and map their fields to agent variables once, instead of passing the same values on every API call.
* **Conversations span more than one call.** When a long call drops or the person calls back, the agent picks up with the context from last time rather than restarting discovery.
* **You run follow-up or chase sequences.** The next call can act on what the last one produced.
* **You want one set of fields across agents.** Post-call data mappings are set per workspace, not per agent, so every agent writes to the same contact fields.

### Example

A tutoring company runs 20-minute enrollment calls. They store `program_interest`, `budget_range`, and `last_topic_discussed` as contact fields, filled from [post-call analysis](/features/post-call-analysis-overview). When a prospect calls back after a dropped call, the agent already has all three and resumes at the pricing conversation instead of starting over.

## How contacts are created

Contacts arrive three ways, and all of them match on phone number:

* **Automatically, after a conversation.** When a phone call or SMS chat ends, Retell looks up the number (the caller's number on inbound, the number you dialed on outbound). If no contact matches, it creates one, then updates the contact's conversation count and last-conversation time. Web calls and web chats have no phone number, so they don't create contacts.
* **Manually**, from **Actions → Add contact**.
* **From your CRM**, if you've connected [Salesforce or HubSpot](/integrations/crm-overview) and configured inbound sync.

A phone number identifies exactly one contact. Adding a contact whose number already exists fails with "A contact with phone number +14155551000 already exists."

## Add a contact

<Frame caption="Everything on the Contacts page that changes data lives in the Actions menu.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/actions-menu.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=d20e484a499949d1d27d706908b5c584" alt="Actions menu open on the Contacts page listing Manage contact fields, Manage CRM sync, Run full sync, Backfill from Post-Call Data, and Add contact." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/contacts/actions-menu.png" />
  </div>
</Frame>

<Steps>
  <Step title="Open the form">
    Select **Actions → Add contact**.

    <Frame caption="The Add Contact form, with the workspace's custom fields below the built-in ones.">
      <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/add-contact.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=45724227a9183109c65793cc3310fcf1" alt="Add Contact dialog with a required phone number field showing the placeholder +14155552671, first name, last name, a Do not call dropdown set to False, and a Custom fields section listing the workspace's own fields: Company, Email, Preferred language, an Account tier selector, and a Renewal date picker." width="1202" height="1010" data-path="images/contacts/add-contact.png" />
    </Frame>
  </Step>

  <Step title="Enter the phone number">
    Required, and it has to be a valid number. Use E.164 format, for example `+14155552671`. This is the key everything else matches on.
  </Step>

  <Step title="Fill in the rest">
    First name, last name, and **Do not call** are optional, as are any custom fields you've defined. Custom field values are checked against the field's type, so a number field rejects text and an enum field rejects values outside its options.
  </Step>

  <Step title="Create">
    Select **Create**. The contact appears in the table right away, with no conversations attached until one happens.
  </Step>
</Steps>

## Work with the contacts table

The table lists every contact in the workspace with these built-in columns, plus one for each custom field you've defined:

| Column                 | What it shows                                                            |
| ---------------------- | ------------------------------------------------------------------------ |
| Phone Number           | The contact's number, and the key used to match conversations.           |
| First Name / Last Name | Set manually, synced from your CRM, or written by post-call analysis.    |
| Contact ID             | The record's identifier, for example `contact_9f2c41ab77de05c3aa61e480`. |
| Related Conversations  | How many calls and chats are attached to this contact.                   |
| Latest Conversation    | When you last spoke to them.                                             |
| Do Not Call            | A flag you can set, filter on, and sync with your CRM.                   |
| External ID            | The record ID in the connected CRM, when the contact came from one.      |

The settings icon above the table, labelled **Manage table**, controls which columns appear and in what order. The arrangement is saved for the workspace, so everyone sees the same table.

**Search** matches phone number, first name, last name, external ID, and custom field values.

**Filters** narrow by phone number, external ID (including whether one exists at all), do-not-call, last conversation time, and any custom field.

## Open a contact

Selecting a row opens a panel with two halves:

* **Contact information** lists every field on the record and lets you edit them in place. Saving an edit on a CRM-linked contact also pushes the change back to your CRM, if outbound sync is configured.
* **Conversations** lists the calls and chats attached to that number, newest first, with total time and a split of inbound calls, outbound calls, and messages. Selecting one opens the full session.

Use the up and down arrow keys to step through contacts without closing the panel.

<Frame caption="A contact's fields on the left, every call and chat with that number on the right.">
  <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/contact-detail.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=12cf2855335a386bf3c6e08babc7c5b3" alt="Contact detail panel for Ada Lovelace showing Contact information with first and last name, related conversations, latest conversation, do not call, external ID, contact ID, and custom fields such as Company, Account tier, Renewal date, and Situation summary, beside a Conversations panel with total time, average call length, an inbound and outbound split bar, and a list of call summaries tagged successful or unsuccessful with sentiment and disconnection reason." width="1812" height="1520" data-path="images/contacts/contact-detail.png" />
</Frame>

## Define contact fields

Beyond the built-in fields, you can define custom fields to store anything else you want to keep about a person. Open **Actions → Manage contact fields**.

The fields table shows, for each field, its type, the CRM field it syncs with, the post-call analysis field that writes to it, and when it last changed.

<Frame caption="Contact fields, with CRM sync and post-call mappings shown per field.">
  <div style={{ aspectRatio: '16 / 9', display: 'flex', alignItems: 'center', justifyContent: 'center', width: '100%' }}>
    <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/contact-fields.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=3d484b76b4738a21a7ed8506eb354f68" alt="Contact Fields page listing the built-in Phone Number, First Name, Last Name, and Do Not Call fields above custom fields such as Company, Email, Account tier, and Renewal date. Each row shows the HubSpot field it syncs with, the post-call analysis field that writes to it, an update mode badge reading Overwrite, Fill only if empty, or Accumulate and summarize, and when it last changed." style={{ maxWidth: '100%', maxHeight: '100%', objectFit: 'contain' }} width="1600" height="900" data-path="images/contacts/contact-fields.png" />
  </div>
</Frame>

<Steps>
  <Step title="Add the field">
    Select **Add Contact Field**, then pick a **type**: Text, Number, Boolean, Selector, Date, or Datetime. Type is fixed after creation, so choose deliberately.

    <Frame caption="Adding a contact field. The type can't be changed once the field exists.">
      <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/add-contact-field.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=a60e15f5304b61bc1ef00110b00ddade" alt="Add contact field dialog with a Type dropdown set to Text, a Field name input with the placeholder 'e.g. Customer need', a note that the field type can't be changed once created, a Description box reading 'What this field captures', and a Map with Post-Call Data toggle." width="1300" height="844" data-path="images/contacts/add-contact-field.png" />
    </Frame>
  </Step>

  <Step title="Name it">
    Field names are snake\_case: lowercase letters, numbers, and underscores, starting with a letter. Names can't collide with the built-in fields, can't start with `contact` or `external` (both reserved), and can't repeat an existing field.
  </Step>

  <Step title="Describe what it holds">
    The description tells you what the field captures. It does more than document: for fields filled by post-call analysis with **Accumulate & summarize**, the description is the instruction the model follows when combining an old value with a new one.
  </Step>

  <Step title="Map it to post-call data (optional)">
    Turn on the post-call mapping, choose the analysis field that feeds it, and pick how new values combine with existing ones:

    * **Overwrite** replaces the stored value every time.
    * **Fill only if empty** writes once and leaves it alone after that.
    * **Accumulate & summarize** merges the old and new values with a model, following your field description.

    Mappings apply to the whole workspace, so any agent producing that analysis field writes to this contact field. See [CRM data mappings](/integrations/crm-mappings) for the full picture, and [contact memory](/integrations/build-contact-memory) for building running context this way.
  </Step>
</Steps>

Built-in fields behave differently from custom ones. First name, last name, and do-not-call can be edited and mapped, but not deleted. Phone number is fixed. Custom fields can be edited or deleted.

## Use contact data in a conversation

When a phone call starts, Retell looks up the contact by number and passes its fields to the agent as dynamic variables: `first_name`, `last_name`, `do_not_call`, and one per custom field, each named after the field. Reference them in the prompt the same way as any other [dynamic variable](/build/dynamic-variables), for example `{{first_name}}`.

This applies to inbound calls, outbound calls, and [batch calls](/deploy/make-batch-call). Chats update contacts after they end, but don't receive contact fields as variables. If no contact matches the number, the variables are simply absent, so write prompts that read naturally without them.

## Backfill fields from past conversations

New mappings only apply going forward. To apply one to conversations that already happened, select **Actions → Backfill from Post-Call Data**, choose which fields to fill, and scope the run by agent and call time.

<Frame caption="Backfilling contact fields from conversations that already happened.">
  <img src="https://mintcdn.com/retellai/sRyxHQPc2FNpy8QJ/images/contacts/backfill.png?fit=max&auto=format&n=sRyxHQPc2FNpy8QJ&q=85&s=96b9eba1caab047ce200be18752dfab9" alt="Backfill from Post-Call Data dialog listing three selected fields, Situation summary, Objections, and Next steps, each labelled with the post-call analysis field it draws from, above a Filter on calls section with a Type dropdown set to Call time and a date range value." width="1220" height="1146" data-path="images/contacts/backfill.png" />
</Frame>

A few things to know before you start:

* You need at least one post-call data mapping configured, or the run is rejected.
* One backfill runs at a time per workspace. Starting a second returns "A backfill job is already running."
* Progress shows as a banner on the Contacts page while the job runs.

## Keep contacts in sync with your CRM

With [Salesforce or HubSpot connected](/integrations/crm-overview), contacts sync both directions: CRM records flow in as contacts, and edits flow back out. Two entries in the **Actions** menu control it, both disabled until a CRM is connected:

* **Manage CRM sync** opens the field mapping settings. See [CRM data mappings](/integrations/crm-mappings).
* **Run full sync** re-imports the full contact set instead of waiting for the next scheduled sync.

While two-way sync is active, contacts that came from the CRM are locked against deletion, so the CRM stays the source of truth for those records.

## Who can use it

Contacts needs CRM view permission to open, and CRM edit permission for anything that changes data: adding contacts, editing fields, running a backfill, or triggering a full sync. Without edit permission the controls are visible but disabled. See [Access Control](/accounts/access-control).

<Note>
  Contacts are managed in the dashboard. There's no public API for creating or listing them at the moment.
</Note>

## FAQ

<AccordionGroup>
  <Accordion title="Can I import contacts from a CSV?">
    No. Contacts come from CRM sync, from adding them in the dashboard, or automatically after a conversation. If your contacts live in Salesforce or HubSpot, [connect the CRM](/integrations/crm-overview) and let inbound sync bring them in.
  </Accordion>

  <Accordion title="Why do contacts appear that I never added?">
    Ending a phone call or SMS chat with a number that has no contact creates one. That's what links a conversation to a person and lets the next call reuse what the last one learned.
  </Accordion>

  <Accordion title="Can two contacts share a phone number?">
    No. The number is the identifier, and creating a duplicate is rejected.
  </Accordion>

  <Accordion title="Why doesn't a contact show a conversation I know happened?">
    Conversations attach by phone number, so check that the number on the call matches the contact exactly. Web calls and web chats carry no phone number and never attach to a contact.
  </Accordion>

  <Accordion title="Can I delete a contact?">
    The dashboard doesn't offer contact deletion today. You can clear a contact's field values by editing it, and you can delete custom contact fields from the contact fields page. Contacts synced from a CRM are locked against deletion while two-way sync is active.
  </Accordion>

  <Accordion title="What happens to my contacts if I disconnect the CRM?">
    The contacts stay. They keep their fields and their conversation history, and the External ID showing where they came from. Syncing stops, so later changes on either side no longer cross over.
  </Accordion>
</AccordionGroup>
