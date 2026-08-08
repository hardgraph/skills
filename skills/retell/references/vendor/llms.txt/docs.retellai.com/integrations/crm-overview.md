> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# CRM integrations

> Connect Salesforce or HubSpot to Retell: sync contacts both ways, map post-call analysis to contact fields, and log call and chat activity to the CRM.

Retell's CRM integration syncs contacts both ways between your CRM and Retell. Import contacts, enrich them with [post-call analysis](/features/post-call-analysis-overview) results, and push conversation activity back, all automatically.

## Supported CRM platforms

<CardGroup cols={2}>
  <Card title="Salesforce" icon="salesforce" href="/integrations/salesforce">
    Sync contacts and log call and chat activity as Salesforce Tasks.
  </Card>

  <Card title="HubSpot" icon="hubspot" href="/integrations/hubspot">
    Sync contacts and log call and chat activity to the HubSpot timeline.
  </Card>
</CardGroup>

<iframe className="w-full aspect-video rounded-xl" src="https://www.youtube.com/embed/nE6y7xrPKZk" title="Connect Hubspot with Retell built-in CRM" frameBorder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />

## How it works

The integration has four parts, each configured separately.

### 1. Contact sync (CRM to Retell)

Retell imports contacts from your CRM and keeps them current. Imported records appear on the [Contacts](/features/contacts) page.

* **Phone number** is the key used to match contacts between systems. It's mapped by default and can't be unmapped.
* **Inbound sync mappings** control which CRM fields are imported.
* Default contact fields are `phone_number`, `first_name`, `last_name`, and `do_not_call`.
* **Custom fields** (string, number, boolean, date, datetime, enum) capture anything else you want from your CRM.

Sync runs **every 5 minutes** and imports only records modified since the last run. A **manual sync re-scans every contact** from scratch, ignoring that cursor, so use it after changing mappings rather than as a routine refresh.

<Note>
  Contacts are skipped when the mapped phone field is empty, or when its value can't be parsed into a valid E.164 phone number. A contact that syncs but shows no name or custom fields usually means those fields aren't mapped, not that the sync failed.
</Note>

### 2. Post-call analysis mapping (analysis to contacts)

After each call or chat, Retell can map [post-call analysis](/features/post-call-analysis-overview) results onto contact fields, building a richer profile as your agents have more conversations.

Each mapping has an update mode. The dashboard labels these differently on the [contact fields](/features/contacts#define-contact-fields) page:

| Update mode       | Dashboard label        | Behavior                                                            |
| ----------------- | ---------------------- | ------------------------------------------------------------------- |
| **Overwrite**     | Overwrite              | Always replace the existing value with the new analysis result      |
| **Fill if empty** | Fill only if empty     | Only write when the field currently has no value                    |
| **Merge**         | Accumulate & summarize | Combine the existing and new values with an LLM (\$0.005 per merge) |

Analysis data mappings are configured **per organization**, not per agent, so the same rules apply no matter which agent handled the conversation.

See [CRM data mappings](/integrations/crm-mappings) for the full data flow and how to write field descriptions that make merges behave.

### 3. Conversation activity logging (Retell to CRM)

With **Log activities automatically** enabled, Retell logs each call and chat to your CRM:

* **Salesforce** — a Task record with the call duration, direction, and summary.
* **HubSpot** — a Call engagement, or a Communication object for chats, on the contact's activity timeline.

<Note>
  Activity is only logged for contacts that were imported from the currently connected CRM. A call from a number that doesn't match a synced contact produces no activity record, and neither do contacts Retell created on its own.
</Note>

### 4. Outbound field sync (Retell to CRM)

**Outbound sync mappings** push contact field updates from Retell back to your CRM. When a contact's fields change in Retell, whether manually or through analysis mapping, the mapped fields are written to the matching CRM record.

Outbound sync **updates existing CRM records only**. It never creates or deletes contacts in your CRM.

Unlike contact sync, this isn't on a schedule. It runs right after a conversation ends, as part of applying that conversation's analysis results.

## Set up a CRM integration

<Steps>
  <Step title="Connect your CRM">
    Open **Integrations** in the Retell Dashboard, select the **Available** tab, and pick your provider. Follow the provider guide for the credentials:

    * [Salesforce setup guide](/integrations/salesforce)
    * [HubSpot setup guide](/integrations/hubspot)

    You need a role with the **CRM.Write** permission to create a connection.
  </Step>

  <Step title="Configure field mappings">
    After the connection test passes, select **Set up contact sync**. The dialog has two tabs: **Import contacts** (CRM to Retell) and **Sync to \[provider]** (Retell to CRM). Phone number is mapped for you and stays locked.
  </Step>

  <Step title="Create custom fields">
    For CRM fields that don't correspond to a default Retell field, create a custom field in Retell to hold the value. You can do this inline from the mapping dropdown.
  </Step>

  <Step title="Set up analysis data mappings (optional)">
    Map your [post-call or post-chat analysis fields](/features/post-call-analysis-overview) to contact fields, choosing an update mode for each based on how you want data to accumulate.
  </Step>

  <Step title="Enable conversation activity logging (optional)">
    Turn on **Log activities automatically** on the **Sync to \[provider]** tab to log each call and chat to your CRM.
  </Step>

  <Step title="Run the first sync">
    Trigger a manual sync to import your existing contacts. This is a full scan, so a large CRM takes a while. It picks up where it left off if it doesn't finish in one pass.
  </Step>
</Steps>

<Note>
  You can connect several CRM accounts, but only one connection drives contact sync for your organization at a time. **Set up contact sync** is disabled on a second connection while another one owns it.
</Note>

## Contact fields

Every Retell contact has four built-in fields:

| Field          | Type    | Description                                                                   |
| -------------- | ------- | ----------------------------------------------------------------------------- |
| `phone_number` | string  | Primary identifier for matching contacts across systems                       |
| `first_name`   | string  | Contact's first name                                                          |
| `last_name`    | string  | Contact's last name                                                           |
| `do_not_call`  | boolean | Used for filtering only. It does **not** block outbound calls to the contact. |

Extend contacts with **custom fields** of type `string`, `number`, `boolean`, `date`, `datetime`, or `enum`.

<Note>
  `do_not_call` isn't mapped by default in either direction. Map it explicitly on both tabs if you want it to sync.
</Note>

## Use contact data in agents

Contact fields are available as **dynamic variables** in your agent prompts. When a call matches a known contact by phone number, the mapped contact fields are injected into the agent's context, so your agent can personalize the conversation using CRM data like the contact's name, account status, or history.

See [dynamic variables](/build/dynamic-variables) for how to reference contact fields in a prompt.

## Troubleshooting

A connection is marked as errored only when your CRM **rejects the credentials**, such as an expired secret or a revoked token. Failures caused by a missing field permission or scope leave the connection reading as healthy while that one field or feature quietly stops working. If a specific field or activity type isn't syncing but the connection looks fine, check permissions and scopes on the CRM side rather than the connection itself.
