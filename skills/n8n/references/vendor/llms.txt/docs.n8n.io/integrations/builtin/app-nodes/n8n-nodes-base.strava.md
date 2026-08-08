> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.strava.md).

# Strava

Use the Strava node to automate work in Strava, and integrate Strava with other applications. n8n has built-in support for a wide range of Strava features, including creating new activities, and getting activity information.

On this page, you'll find a list of operations the Strava node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Strava credentials](/integrations/builtin/credentials/strava.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Activity
  * Create a new activity
  * Get an activity
  * Get all activities
  * Get all activity comments
  * Get all activity kudos
  * Get all activity laps
  * Get all activity zones
  * Update an activity

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Strava node documentation integration templates](https://n8n.io/integrations/strava) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
