> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googlebusinessprofile.md).

# Google Business Profile

Use the Google Business Profile node to automate work in Google Business Profile and integrate Google Business Profile with other applications. n8n has built-in support for a wide range of Google Business Profile features, which includes creating, updating, and deleting posts and reviews.

On this page, you'll find a list of operations the Google Business Profile node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/google.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Post
  * Create
  * Delete
  * Get
  * Get Many
  * Update
* Review
  * Delete Reply
  * Get
  * Get Many
  * Reply

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Google Business Profile node documentation integration templates](https://n8n.io/integrations/google-business-profile) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

n8n provides a trigger node for Google Business Profile. You can find the trigger node docs [here](/integrations/builtin/trigger-nodes/n8n-nodes-base.googlebusinessprofiletrigger.md).

Refer to [Google Business Profile's documentation](https://developers.google.com/my-business/reference/rest) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
