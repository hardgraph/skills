> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.mautic.md).

# Mautic

Use the Mautic node to automate work in Mautic, and integrate Mautic with other applications. n8n has built-in support for a wide range of Mautic features, including creating, updating, deleting, and getting companies, and contacts, as well as adding and removing campaign contacts.

On this page, you'll find a list of operations the Mautic node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Mautic credentials](/integrations/builtin/credentials/mautic.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Campaign Contact
  * Add contact to a campaign
  * Remove contact from a campaign
* Company
  * Create a new company
  * Delete a company
  * Get data of a company
  * Get data of all companies
  * Update a company
* Company Contact
  * Add contact to a company
  * Remove a contact from a company
* Contact
  * Create a new contact
  * Delete a contact
  * Edit contact's points
  * Add/remove contacts from/to the don't contact list
  * Get data of a contact
  * Get data of all contacts
  * Send email to contact
  * Update a contact
* Contact Segment
  * Add contact to a segment
  * Remove contact from a segment
* Segment Email
  * Send

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Mautic node documentation integration templates](https://n8n.io/integrations/mautic) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
