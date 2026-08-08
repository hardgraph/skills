> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CRM data mappings

> How data flows between your CRM, Retell contacts, and post-call analysis: inbound and outbound field mappings, sync direction, update modes, and custom fields.

This page explains the data relationships between your external CRM, Retell contacts, and post-call analysis data, and how to configure the mappings that control data flow.

## Data model overview

The CRM integration involves three main entities and four data flows between them:

<CardGroup cols={3}>
  <Card title="External CRM" icon="database">
    Your Salesforce or HubSpot contact records with their native fields.
  </Card>

  <Card title="Retell Contact" icon="address-card">
    A unified contact record in Retell with default fields and custom fields.
  </Card>

  <Card title="Post-Call Analysis" icon="chart-mixed">
    Structured data extracted from each call/chat by your [post-call analysis](/features/post-call-analysis-overview) configuration.
  </Card>
</CardGroup>

| Data Flow            | Direction                           | Description                                                            |
| -------------------- | ----------------------------------- | ---------------------------------------------------------------------- |
| **Inbound Sync**     | External CRM → Retell Contact       | Import and update contacts from your CRM on a recurring schedule       |
| **Analysis Mapping** | Post-Call Analysis → Retell Contact | Map extracted conversation data to contact fields after each call/chat |
| **Outbound Sync**    | Retell Contact → External CRM       | Push updated contact fields back to your CRM                           |
| **Activity Logging** | Retell Contact → External CRM       | Log call and chat records as activities on the CRM contact             |

## How data flows

### 1. Inbound sync: CRM to Retell

Inbound sync imports contacts from your CRM into Retell. It runs every 5 minutes and picks up only records modified since the last run. A manual sync re-scans everything.

**Matching logic:** Contacts are matched between systems using **phone number** as the primary key. When Retell finds a CRM contact with a phone number that matches an existing Retell contact, it updates the existing record. Otherwise, it creates a new contact.

**Records Retell skips:** a CRM contact with no value in the mapped phone field, or with a phone number that can't be parsed into a valid E.164 number. Skipped records don't stop the sync.

**Inbound sync mappings** control which CRM fields are imported:

| CRM Field (Example)                      | Direction     | Retell Field              |
| ---------------------------------------- | ------------- | ------------------------- |
| `Phone` (Salesforce) / `phone` (HubSpot) | CRM -> Retell | `phone_number` (required) |
| `FirstName` / `firstname`                | CRM -> Retell | `first_name`              |
| `LastName` / `lastname`                  | CRM -> Retell | `last_name`               |
| `Default Field` / `hs_field`             | CRM -> Retell | Your custom field name    |
| `Custom_Field__c` / `custom_property`    | CRM -> Retell | Your custom field name    |

<Note>
  The `phone_number` mapping is required and hardcoded for inbound sync. Without it, Retell has no way to match CRM records to Retell conversations.
</Note>

### 2. Analysis data mapping: analysis to contact

After each call or chat, [post-call analysis](/features/post-call-analysis-overview) extracts structured data from the conversation. You can map these analysis results to contact fields, building a richer contact profile over time.

For example, if your post-call analysis extracts a `lead_qualification_status` field, you can map it to a custom contact field so each contact's qualification status is automatically updated after every conversation.

**Example mappings:**

| Analysis Data Field | Direction           | Contact Field          | Update Mode   |
| ------------------- | ------------------- | ---------------------- | ------------- |
| `lead_status`       | Analysis -> Contact | `qualification_status` | Overwrite     |
| `email_address`     | Analysis -> Contact | `email`                | Fill if empty |
| `customer_notes`    | Analysis -> Contact | `interaction_summary`  | Merge         |

#### Update modes

Each analysis data mapping has an **update mode** that controls how the new value interacts with the existing value. The names in parentheses are the labels shown on the [contact fields](/features/contacts#define-contact-fields) page in the dashboard.

<CardGroup cols={3}>
  <Card title="Overwrite">
    Always replace the existing value with the new analysis result. Use this for fields where the latest value is always the most relevant (e.g., sentiment, status).
  </Card>

  <Card title="Fill if Empty (Fill only if empty)">
    Only write the new value if the field is currently empty. Use this for fields you want to capture once and preserve (e.g., email address, company name).
  </Card>

  <Card title="Merge (Accumulate & summarize)">
    Intelligently combine the existing and new values using LLM. Only available for string fields. Use this for cumulative fields where you want to preserve context from previous conversations (e.g., interaction notes, preferences).
  </Card>
</CardGroup>

#### How merge works

The **Merge** update mode uses an LLM to intelligently combine the existing contact field value with the new analysis result from the latest conversation. Instead of simply appending or replacing text, the LLM reads both values and produces a single, coherent merged result.

The **field description** you set when creating or editing the custom field plays a critical role in this process. The LLM uses the field description as its primary instruction for how to perform the merge — it tells the model what kind of data the field holds, what information to prioritize, and how the merged output should be structured.

**Example field descriptions for merge:**

| Field Name             | Good Description                                                                                                                                                                                                                                    | Why It Works                                                                        |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `interaction_notes`    | `A running summary of all conversations with this contact. Include key topics discussed, action items, and any commitments made. Keep the most recent interaction details prominent while preserving important context from earlier conversations.` | Tells the LLM to maintain a chronological summary and what details matter.          |
| `customer_preferences` | `The contact's stated preferences and requirements. Consolidate preferences across conversations — update changed preferences and keep unchanged ones. Format as a bullet list.`                                                                    | Guides the LLM to reconcile conflicting information and specifies an output format. |
| `objections_raised`    | `A deduplicated list of objections or concerns the contact has raised across all conversations. Merge new objections into the existing list without repeating ones already captured.`                                                               | Instructs the LLM to deduplicate rather than blindly concatenate.                   |

<Tip>
  Think of the field description as a prompt to the LLM. The more specific you are about what to keep, what to discard, and how to structure the output, the better the merge results will be.
</Tip>

**Limits worth knowing.** Merge only applies to `string` fields; on any other type the mapping is skipped. The merged result is capped at roughly 512 tokens, so a field set up as an ever-growing log eventually starts losing the oldest detail rather than expanding forever. Write the description to prioritize what matters most.

If the merge call fails or comes back empty, Retell falls back to keeping both values, appending the new one below the existing one separated by a blank line. You never lose data to a failed merge, but the field won't be as tidy as a successful one.

### 3. Outbound sync: Retell to CRM

When a contact's fields are updated in Retell, either manually or via analysis data mapping, the changed fields can be pushed back to your CRM through **outbound sync mappings**.

Outbound sync only triggers when all of these hold:

* The contact was **imported from your CRM** and still carries its CRM record ID.
* The contact was imported by the **connection that's currently active**. If you delete a connection and set up a new one, contacts imported by the old one stop syncing outbound until they're re-imported.
* At least one **outbound sync mapping** is configured.
* The conversation actually **changed a mapped field**. An analysis result identical to the stored value writes nothing.

Outbound sync **updates existing CRM records only**. It never creates or deletes contacts in your CRM. It also never writes back the phone number, which stays reserved as the matching key.

**Outbound sync mappings** control which Retell fields are pushed:

| Retell Field           | Direction     | CRM Field (Example)              |
| ---------------------- | ------------- | -------------------------------- |
| `qualification_status` | Retell -> CRM | `Lead_Status__c` / `lead_status` |
| `interaction_summary`  | Retell -> CRM | `Notes__c` / `notes`             |

### 4. Conversation activity logging

When enabled, Retell logs each call and chat as an activity record in your CRM, associated with the matched contact. This gives your sales team a complete conversation history directly in the CRM.

Activity records include:

* Call/chat direction (inbound or outbound)
* Duration (for calls)
* Summary
* Timestamp
* The Retell conversation ID, and the from and to phone numbers

Retell picks the number to match on from the call's direction: the caller's number on an inbound call, the dialed number on an outbound one.

<Note>
  Activity logging has the same requirement as outbound sync: the contact must have been imported from the currently active CRM connection. Conversations with numbers that don't match a synced contact, and contacts Retell created itself, produce no activity record.
</Note>

## End-to-end data flow example

Here's how a typical interaction flows through the system:

<Steps>
  <Step title="Contact imported from CRM">
    An inbound sync imports a contact from Salesforce with fields: `Phone: +1234567890`, `FirstName: Alice`, `LastName: Smith`, `Lead_Status__c: New`.

    A Retell contact is created with: `phone_number: +1234567890`, `first_name: Alice`, `last_name: Smith`, `lead_status: New`.
  </Step>

  <Step title="Agent makes a call">
    Your voice agent calls Alice. During the call, contact fields are injected as dynamic variables, so the agent knows Alice's name and lead status.
  </Step>

  <Step title="Post-call analysis runs">
    After the call, post-call analysis extracts:

    * `lead_qualification_status: Qualified`
    * `customer_notes: Interested in enterprise plan, wants demo next week`
    * `email: alice@example.com`
  </Step>

  <Step title="Analysis data maps to contact">
    The analysis results are applied to Alice's contact based on your mappings:

    * `lead_status` is **overwritten** with `Qualified`
    * `interaction_summary` is **merged** with the new notes, preserving previous conversation context
    * `email` is **filled** (was previously empty)
  </Step>

  <Step title="Outbound sync pushes to CRM">
    Updated fields are synced back to Salesforce:

    * `Lead_Status__c` updated to `Qualified`
    * `Notes__c` updated with the merged interaction summary
  </Step>

  <Step title="Activity logged in CRM">
    A Task record is created in Salesforce with the call summary, duration, and direction, associated with Alice's contact record.
  </Step>
</Steps>

## Configuring field mappings

### Custom fields

Before you can map CRM fields to non-default Retell fields, you need to create **custom fields** on your Retell contacts.

Custom fields support the following types:

| Type       | Example Values                     |
| ---------- | ---------------------------------- |
| `string`   | `"Interested in enterprise plan"`  |
| `number`   | `42`, `99.5`                       |
| `boolean`  | `true`, `false`                    |
| `date`     | `2025-03-15`                       |
| `datetime` | `2025-03-15T10:30:00Z`             |
| `enum`     | One of a predefined set of options |

<Note>
  Custom field names must be in `snake_case` and cannot conflict with the built-in field names (`phone_number`, `first_name`, `last_name`, `do_not_call`).
</Note>

### Field type compatibility

When configuring sync mappings, the CRM field type must be compatible with the Retell field type. Retell handles type coercion automatically:

* CRM date strings are parsed into Retell `date`/`datetime` fields
* CRM picklist values map to Retell `enum` fields
* CRM numeric strings are converted to Retell `number` fields
* CRM boolean-like values (`"true"`, `"false"`) are converted to Retell `boolean` fields

## Best practices

* **Start with essential mappings.** Map `phone_number`, `first_name`, and `last_name` for inbound sync first. Add custom fields incrementally as you identify what data is valuable.

* **Use "Fill if empty" for stable data.** Fields like email address or company name rarely change — use "Fill if empty" to capture them on first mention without overwriting later.

* **Use "Merge" sparingly.** The merge update mode uses AI to combine values, which works well for free-text notes but adds latency and cost. Use "Overwrite" or "Fill if empty" when possible.

* **Write descriptive field descriptions for merge fields.** The field description acts as the LLM's instruction for how to merge values. A specific, well-written description (e.g., "A running summary of interactions — keep recent details prominent, deduplicate action items") produces much better results than a vague one. See [How Merge Works](#how-merge-works) for examples.

* **Use "Overwrite" for status fields.** Fields like lead status or sentiment should always reflect the most recent conversation.

* **Enable activity logging for sales visibility.** Your sales team can see the full conversation history in the CRM without switching to the Retell dashboard.

* **Use a dedicated CRM integration user.** For Salesforce, use a dedicated service account as the Run-As user to avoid permission issues when team members change roles.
