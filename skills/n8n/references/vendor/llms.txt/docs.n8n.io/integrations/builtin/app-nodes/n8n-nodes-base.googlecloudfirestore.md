> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlecloudfirestore.md).

# Google Cloud Firestore

Use the Google Cloud Firestore node to automate work in Google Cloud Firestore, and integrate Google Cloud Firestore with other applications. n8n has built-in support for a wide range of Google Cloud Firestore features, including creating, deleting, and getting documents.

On this page, you'll find a list of operations the Google Cloud Firestore node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Google credentials](/integrations/builtin/credentials/google.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Document
  * Create a document
  * Create/Update a document
  * Delete a document
  * Get a document
  * Get all documents from a collection
  * Runs a query against your documents
* Collection
  * Get all root collections

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Google Cloud Firestore node documentation integration templates](https://n8n.io/integrations/google-cloud-firestore) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
