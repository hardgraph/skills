> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.misp.md).

# MISP

Use the MISP node to automate work in MISP, and integrate MISP with other applications. n8n has built-in support for a wide range of MISP features, including creating, updating, deleting and getting events, feeds, and organizations.

On this page, you'll find a list of operations the MISP node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [MISP credentials](/integrations/builtin/credentials/misp.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Attribute
  * Create
  * Delete
  * Get
  * Get All
  * Search
  * Update
* Event
  * Create
  * Delete
  * Get
  * Get All
  * Publish
  * Search
  * Unpublish
  * Update
* Event Tag
  * Add
  * Remove
* Feed
  * Create
  * Disable
  * Enable
  * Get
  * Get All
  * Update
* Galaxy
  * Delete
  * Get
  * Get All
* Noticelist
  * Get
  * Get All
* Object
  * Search
* Organisation
  * Create
  * Delete
  * Get
  * Get All
  * Update
* Tag
  * Create
  * Delete
  * Get All
  * Update
* User
  * Create
  * Delete
  * Get
  * Get All
  * Update
* Warninglist
  * Get
  * Get All

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse MISP node documentation integration templates](https://n8n.io/integrations/misp) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
