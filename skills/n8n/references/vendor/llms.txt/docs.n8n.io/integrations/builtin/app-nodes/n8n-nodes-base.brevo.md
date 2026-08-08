> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.brevo.md).

# Brevo

Use the Brevo node to automate work in Brevo, and integrate Brevo with other applications. n8n has built-in support for a wide range of Brevo features, including creating, updating, deleting, and getting contacts, attributes, as well as sending emails.

On this page, you'll find a list of operations the Brevo node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Brevo credentials](/integrations/builtin/credentials/brevo.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Contact
  * Create
  * Create or Update
  * Delete
  * Get
  * Get All
  * Update
* Contact Attribute
  * Create
  * Delete
  * Get All
  * Update
* Email
  * Send
  * Send Template
* Sender
  * Create
  * Delete
  * Get All

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Brevo node documentation integration templates](https://n8n.io/integrations/brevo) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
