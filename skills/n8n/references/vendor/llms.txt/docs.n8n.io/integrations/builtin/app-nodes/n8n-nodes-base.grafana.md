> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.grafana.md).

# Grafana

Use the Grafana node to automate work in Grafana, and integrate Grafana with other applications. n8n has built-in support for a wide range of Grafana features, including creating, updating, deleting, and getting dashboards, teams, and users.

On this page, you'll find a list of operations the Grafana node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Grafana credentials](/integrations/builtin/credentials/grafana.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Dashboard
  * Create a dashboard
  * Delete a dashboard
  * Get a dashboard
  * Get all dashboards
  * Update a dashboard
* Team
  * Create a team
  * Delete a team
  * Get a team
  * Retrieve all teams
  * Update a team
* Team Member
  * Add a member to a team
  * Retrieve all team members
  * Remove a member from a team
* User
  * Delete a user from the current organization
  * Retrieve all users in the current organization
  * Update a user in the current organization

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Grafana node documentation integration templates](https://n8n.io/integrations/grafana) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
