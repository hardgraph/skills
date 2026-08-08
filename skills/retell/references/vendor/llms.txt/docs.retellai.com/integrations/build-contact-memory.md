> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Give your AI agent persistent contact memory

> Map post-call analysis fields to contact fields so your Retell AI agent remembers preferences, past issues, and commitments across every conversation.

Contact memory lets your AI agent recall information from previous conversations — preferences, past issues, commitments, and more — without any external database. By mapping [post-call analysis](/features/post-call-analysis-overview) fields to [contact](/features/contacts) fields, Retell automatically extracts and stores conversation insights after every call or chat, building a richer contact profile over time.

The next time your agent calls that contact, the accumulated memory is injected into the agent's context as [dynamic variables](/build/dynamic-variables), enabling personalized, context-aware conversations.

## How It Works

Contact memory is built through a pipeline that runs automatically after every conversation:

<Steps>
  <Step title="Post-call analysis extracts structured data">
    After each call or chat, your agent's [post-call analysis](/features/post-call-analysis-create) configuration extracts key information from the conversation transcript — things like preferences, action items, objections, or any custom fields you define.
  </Step>

  <Step title="Analysis data maps to contact fields">
    The extracted data is written to contact fields using your configured [analysis data mappings](/integrations/crm-mappings#2-analysis-data-mapping-analysis-to-contact). Each mapping specifies an **update mode** that controls how new data combines with existing data.
  </Step>

  <Step title="Contact fields are available as dynamic variables">
    On the next phone call with the same contact, all contact fields, including the accumulated memory, are automatically injected into the agent's prompt as [dynamic variables](/build/dynamic-variables). Chats write to contact fields but don't receive them as variables.
  </Step>
</Steps>

The **Merge** update mode is the key to building memory. Unlike "Overwrite" (which replaces the old value) or "Fill if empty" (which only writes once), Merge uses an LLM to intelligently combine the existing field value with new information from the latest conversation — deduplicating, reconciling conflicts, and maintaining a coherent, up-to-date record.

## Setup Guide

### Step 1: Define Post-Call Analysis Fields

Create post-call analysis fields on your agent that extract the information you want your agent to remember. Navigate to your agent's **Post-Call Analysis** tab and add fields.

**Example fields for contact memory:**

| Field Name             | Type | Description                                                                          |
| ---------------------- | ---- | ------------------------------------------------------------------------------------ |
| `customer_preferences` | Text | Extract any stated preferences, requirements, or constraints the customer mentioned. |
| `key_topics`           | Text | Summarize the main topics discussed and any decisions made.                          |
| `action_items`         | Text | List any follow-up actions, commitments, or next steps agreed upon.                  |
| `objections`           | Text | Capture any concerns, objections, or hesitations the customer raised.                |

<Tip>
  Write descriptive extraction prompts. The more specific you are about what to extract, the more useful the memory will be. For example, instead of "Summarize the call," use "Extract the customer's stated budget, timeline, and any product preferences mentioned during the conversation."
</Tip>

### Step 2: Create Contact Custom Fields

On the [Contacts](/features/contacts) page, open **Actions → Manage contact fields** and create custom fields to store the accumulated memory. These fields will hold the merged data across all conversations.

**Example contact fields:**

| Field Name                | Type   | Description (used as the LLM merge instruction)                                                                                                                                                                                                            |
| ------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `preferences`             | String | A consolidated list of this contact's preferences and requirements across all conversations. When merging, update changed preferences and keep unchanged ones. Deduplicate. Format as a bullet list.                                                       |
| `interaction_history`     | String | A running summary of all conversations with this contact. Include key topics, decisions, and outcomes. Keep the most recent interaction prominent while preserving important context from earlier conversations. Limit to the 10 most recent interactions. |
| `pending_actions`         | String | Outstanding action items and commitments. When merging, mark completed items as done if the new conversation indicates completion. Add new action items. Remove items that are no longer relevant.                                                         |
| `objections_and_concerns` | String | A deduplicated list of objections or concerns raised across all conversations. When merging, add new concerns without repeating existing ones. Note if a previously raised concern has been resolved.                                                      |

<Warning>
  The **field description is critical** when using the Merge update mode. The description acts as an instruction to the LLM that performs the merge — it determines how existing and new values are combined. See [How Merge Works](/integrations/crm-mappings#how-merge-works) for detailed guidance.
</Warning>

### Step 3: Map Analysis Fields to Contact Fields

On the same [contact fields](/features/contacts#define-contact-fields) page, configure **post-call data mappings** for each field. Map each post-call analysis field to its corresponding contact field and select the **Merge** update mode, labelled **Accumulate & summarize** in the dashboard.

| Analysis Field         | → | Contact Field             | Update Mode |
| ---------------------- | - | ------------------------- | ----------- |
| `customer_preferences` | → | `preferences`             | Merge       |
| `key_topics`           | → | `interaction_history`     | Merge       |
| `action_items`         | → | `pending_actions`         | Merge       |
| `objections`           | → | `objections_and_concerns` | Merge       |

<Note>
  Analysis data mappings are configured at the **organization level**, not per agent. As long as any agent's post-call analysis produces a field with the mapped name, the value will be written to the contact. This means memory accumulates across all agents that interact with the same contact.
</Note>

### Step 4: Reference Memory in Your Agent Prompt

Use [dynamic variables](/build/dynamic-variables) to inject the accumulated memory into your agent's prompt. When a phone call connects to a known contact (matched by phone number), all contact fields are automatically available.

## Sync Memory to Your CRM

If you have a [CRM integration](/integrations/crm-overview) connected, whether [Salesforce](/integrations/salesforce) or [HubSpot](/integrations/hubspot), you can push the accumulated contact memory back to your CRM using [outbound sync mappings](/integrations/crm-mappings#3-outbound-sync-retell-to-crm). This keeps your CRM records enriched with insights from every AI conversation.

| Retell Contact Field  | → | CRM Field                                          |
| --------------------- | - | -------------------------------------------------- |
| `preferences`         | → | `Customer_Preferences__c` / `customer_preferences` |
| `interaction_history` | → | `AI_Interaction_Notes__c` / `ai_interaction_notes` |
| `pending_actions`     | → | `Pending_Actions__c` / `pending_actions`           |

This creates a complete loop: CRM data flows into Retell, the agent uses it during conversations, post-call analysis extracts new insights, those insights merge into the contact profile, and the updated profile syncs back to your CRM.

## Best Practices

* **Keep memory fields focused.** A single field that tries to capture everything produces noisy, hard-to-use context. Create separate fields for distinct categories (preferences, history, action items) so each one stays clean and useful.

* **Write specific merge descriptions.** The field description controls how the LLM merges values. "A running summary of interactions" is too vague. "A chronological summary of the last 10 interactions — include key topics and outcomes, drop greetings and small talk" gives the LLM clear instructions.

* **Set size limits in merge descriptions.** Without limits, merged fields grow unboundedly. Include instructions like "Limit to 10 most recent interactions" or "Keep under 500 words" to prevent fields from becoming too large for the agent's context window.

* **Use "Overwrite" for status fields, "Merge" for history fields.** A field like `lead_status` should always reflect the latest value — use Overwrite. A field like `interaction_history` needs to accumulate across conversations — use Merge.

* **Use "Fill if empty" for stable facts.** Fields like email address or company name rarely change. Use "Fill if empty" so they're captured on first mention without being overwritten by later extractions.

* **Backfill from past conversations.** If you set up memory fields after conversations have already occurred, use the [batch rerun feature](/features/rerun-call-analysis#batch-rerun-from-callchat-history) to populate contact fields from historical conversation data.

* **Test with real conversations.** After configuring your memory pipeline, make a few test calls to the same contact and verify that the contact fields accumulate correctly. Check the contact detail page to confirm the merged values make sense.
