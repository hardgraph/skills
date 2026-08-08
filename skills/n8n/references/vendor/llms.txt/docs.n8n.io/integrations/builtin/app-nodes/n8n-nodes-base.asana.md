> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.asana.md).

# Asana

Use the Asana node to automate work in Asana, and integrate Asana with other applications. n8n has built-in support for a wide range of Asana features, including creating, updating, deleting, and getting users, tasks, projects, and subtasks.

On this page, you'll find a list of operations the Asana node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Asana credentials](/integrations/builtin/credentials/asana.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**Update to 1.22.2 or above**

Due to changes in Asana's API, some operations in this node stopped working on 17th January 2023. Upgrade to n8n 1.22.2 or above.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Project
  * Create a new project
  * Delete a project
  * Get a project
  * Get all projects
  * Update a project
* Subtask
  * Create a subtask
  * Get all subtasks
* Task
  * Create a task
  * Delete a task
  * Get a task
  * Get all tasks
  * Move a task
  * Search for tasks
  * Update a task
* Task Comment
  * Add a comment to a task
  * Remove a comment from a task
* Task Tag
  * Add a tag to a task
  * Remove a tag from a task
* Task Project
  * Add a task to a project
  * Remove a task from a project
* User
  * Get a user
  * Get all users

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Asana node documentation integration templates](https://n8n.io/integrations/asana) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
