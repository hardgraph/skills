> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.azurestorage.md).

# Azure Storage

The Azure Storage node has built-in support for a wide range of features, which includes creating, getting, and deleting blobs and containers. Use this node to automate work within the Azure Storage service or integrate it with other services in your workflow.

On this page, you'll find a list of operations the Azure Storage node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/azurestorage.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* **Blob**
  * **Create blob**: Create a new blob or replace an existing one.
  * **Delete blob**: Delete an existing blob.
  * **Get blob**: Retrieve data for a specific blob.
  * **Get many blobs**: Retrieve a list of blobs.
* **Container**
  * **Create container**: Create a new container.
  * **Delete container**: Delete an existing container.
  * **Get container**: Retrieve data for a specific container.
  * **Get many containers**: Retrieve a list of containers.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Azure Storage node documentation integration templates](https://n8n.io/integrations/azure-storage) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Microsoft's Azure Storage documentation](https://learn.microsoft.com/en-us/rest/api/storageservices/) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
