> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.clockify.md).

# Clockify

Use the Clockify node to automate work in Clockify, and integrate Clockify with other applications. n8n has built-in support for a wide range of Clockify features, including creating, updating, getting, and deleting tasks, time entries, projects, and tags.

On this page, you'll find a list of operations the Clockify node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Clockify credentials](/integrations/builtin/credentials/clockify.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Project
  * Create a project
  * Delete a project
  * Get a project
  * Get all projects
  * Update a project
* Tag
  * Create a tag
  * Delete a tag
  * Get all tags
  * Update a tag
* Task
  * Create a task
  * Delete a task
  * Get a task
  * Get all tasks
  * Update a task
* Time Entry
  * Create a time entry
  * Delete a time entry
  * Get time entry
  * Update a time entry

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Clockify node documentation integration templates](https://n8n.io/integrations/clockify) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
