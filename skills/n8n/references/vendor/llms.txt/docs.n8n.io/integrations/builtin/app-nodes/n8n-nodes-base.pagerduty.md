> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.pagerduty.md).

# PagerDuty

Use the PagerDuty node to automate work in PagerDuty, and integrate PagerDuty with other applications. n8n has built-in support for a wide range of PagerDuty features, including creating incident notes, as well as updating, and getting all log entries and users.

On this page, you'll find a list of operations the PagerDuty node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [PagerDuty credentials](/integrations/builtin/credentials/pagerduty.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Incident
  * Create an incident
  * Get an incident
  * Get all incidents
  * Update an incident
* Incident Note
  * Create an incident note
  * Get all incident's notes
* Log Entry
  * Get a log entry
  * Get all log entries
* User
  * Get a user

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse PagerDuty node documentation integration templates](https://n8n.io/integrations/pagerduty) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
