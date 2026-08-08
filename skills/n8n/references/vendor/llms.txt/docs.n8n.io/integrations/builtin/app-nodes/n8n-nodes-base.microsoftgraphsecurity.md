> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.microsoftgraphsecurity.md).

# Microsoft Graph Security

Use the Microsoft Graph Security node to automate work in Microsoft Graph Security, and integrate Microsoft Graph Security with other applications. n8n has built-in support for a wide range of Microsoft Graph Security features, including getting, and updating scores, and profiles.

On this page, you'll find a list of operations the Microsoft Graph Security node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Microsoft credentials](/integrations/builtin/credentials/microsoft.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**Government Cloud Support**

If you're using a government cloud tenant (US Government, US Government DOD, or China), make sure to select the appropriate **Microsoft Graph API Base URL** in your Microsoft credentials configuration.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Secure Score
  * Get
  * Get All
* Secure Score Control Profile
  * Get
  * Get All
  * Update

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Microsoft Graph Security node documentation integration templates](https://n8n.io/integrations/microsoft-graph-security) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
