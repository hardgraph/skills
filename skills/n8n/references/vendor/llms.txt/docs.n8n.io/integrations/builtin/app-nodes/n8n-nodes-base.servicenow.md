> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.servicenow.md).

# ServiceNow

Use the ServiceNow node to automate work in ServiceNow, and integrate ServiceNow with other applications. n8n has built-in support for a wide range of ServiceNow features, including getting business services, departments, configuration items, and dictionary as well as creating, updating, and deleting incidents, users, and table records.

On this page, you'll find a list of operations the ServiceNow node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [ServiceNow credentials](/integrations/builtin/credentials/servicenow.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Business Service
  * Get All
* Configuration Items
  * Get All
* Department
  * Get All
* Dictionary
  * Get All
* Incident
  * Create
  * Delete
  * Get
  * Get All
  * Update
* Table Record
  * Create
  * Delete
  * Get
  * Get All
  * Update
* User
  * Create
  * Delete
  * Get
  * Get All
  * Update
* User Group
  * Get All
* User Role
  * Get All

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse ServiceNow node documentation integration templates](https://n8n.io/integrations/servicenow) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
