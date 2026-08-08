> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.hubspot.md).

# HubSpot

Use the HubSpot node to automate work in HubSpot, and integrate HubSpot with other applications. n8n has built-in support for a wide range of HubSpot features, including creating, updating, deleting, and getting contacts, deals, lists, engagements and companies.

On this page, you'll find a list of operations the HubSpot node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [HubSpot credentials](/integrations/builtin/credentials/hubspot.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Contact
  * Create/Update a contact
  * Delete a contact
  * Get a contact
  * Get all contacts
  * Get recently created/updated contacts
  * Search contacts
* Contact List
  * Add contact to a list
  * Remove a contact from a list
* Company
  * Create a company
  * Delete a company
  * Get a company
  * Get all companies
  * Get recently created companies
  * Get recently modified companies
  * Search companies by domain
  * Update a company
* Deal
  * Create a deal
  * Delete a deal
  * Get a deal
  * Get all deals
  * Get recently created deals
  * Get recently modified deals
  * Search deals
  * Update a deal
* Engagement
  * Create an engagement
  * Delete an engagement
  * Get an engagement
  * Get all engagements
* Form
  * Get all fields from a form
  * Submit data to a form
* Ticket
  * Create a ticket
  * Delete a ticket
  * Get a ticket
  * Get all tickets
  * Update a ticket

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse HubSpot node documentation integration templates](https://n8n.io/integrations/hubspot) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
