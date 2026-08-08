> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudstorage.md).

# Google Cloud Storage

Use the Google Cloud Storage node to automate work in Google Cloud Storage, and integrate Google Cloud Storage with other applications. n8n has built-in support for a wide range of Google Cloud Storage features, including creating, updating, deleting, and getting buckets and objects.

On this page, you'll find a list of operations the Google Cloud Storage node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Google Cloud Storage credentials](/integrations/builtin/credentials/google.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Bucket
  * Create
  * Delete
  * Get
  * Get Many
  * Update
* Object
  * Create
  * Delete
  * Get
  * Get Many
  * Update

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Google Cloud Storage node documentation integration templates](https://n8n.io/integrations/google-cloud-storage) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to Google's [Cloud Storage API documentation](https://cloud.google.com/storage/docs/apis) for detailed information about the API that this node integrates with.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
