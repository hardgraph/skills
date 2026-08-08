> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.drift.md).

# Drift

Use the Drift node to automate work in Drift, and integrate Drift with other applications. n8n has built-in support for a wide range of Drift features, including creating, updating, deleting, and getting contacts.

On this page, you'll find a list of operations the Drift node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Drift credentials](/integrations/builtin/credentials/drift.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Contact
  * Create a contact
  * Get custom attributes
  * Delete a contact
  * Get a contact
  * Update a contact

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Drift node documentation integration templates](https://n8n.io/integrations/drift) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
