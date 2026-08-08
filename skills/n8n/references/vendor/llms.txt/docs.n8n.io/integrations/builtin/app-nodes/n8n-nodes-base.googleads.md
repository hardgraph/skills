> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.googleads.md).

# Google Ads

Use the Google Ads node to automate work in Google Ads, and integrate Google Ads with other applications. n8n has built-in support for a wide range of Google Ads features, including getting campaigns.

On this page, you'll find a list of operations the Google Ads node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Google Ads credentials](/integrations/builtin/credentials/google.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Campaign
  * Get all campaigns
  * Get a campaign

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Google Ads node documentation integration templates](https://n8n.io/integrations/google-ads) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Google Ads' documentation](https://developers.google.com/google-ads/api/docs/start) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
