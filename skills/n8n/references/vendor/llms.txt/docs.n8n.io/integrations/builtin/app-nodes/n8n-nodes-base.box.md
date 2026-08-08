> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.box.md).

# Box

Use the Box node to automate work in Box, and integrate Box with other applications. n8n has built-in support for a wide range of Box features, including creating, copying, deleting, searching, uploading, and downloading files and folders.

On this page, you'll find a list of operations the Box node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Box credentials](/integrations/builtin/credentials/box.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* File
  * Copy a file
  * Delete a file
  * Download a file
  * Get a file
  * Search files
  * Share a file
  * Upload a file
* Folder
  * Create a folder
  * Get a folder
  * Delete a folder
  * Search files
  * Share a folder
  * Update folder

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Box node documentation integration templates](https://n8n.io/integrations/box) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
