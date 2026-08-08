> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.gsuiteadmin.md).

# Google Workspace Admin

Use the Google Workspace Admin node to automate work in Google Workspace Admin, and integrate Google Workspace Admin with other applications. n8n has built-in support for a wide range of Google Workspace Admin features, including creating, updating, deleting, and getting users, groups, and ChromeOS devices.

On this page, you'll find a list of operations the Google Workspace Admin node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Google credentials](/integrations/builtin/credentials/google.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* ChromeOS Device
  * Get a ChromeOS device
  * Get many ChromeOS devices
  * Update a ChromeOS device
  * Change the status of a ChromeOS device
* Group
  * Create a group
  * Delete a group
  * Get a group
  * Get many groups
  * Update a group
* User
  * Add an existing user to a group
  * Create a user
  * Delete a user
  * Get a user
  * Get many users
  * Remove a user from a group
  * Update a user

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Google Workspace Admin node documentation integration templates](https://n8n.io/integrations/google-workspace-admin) or [search all templates](https://n8n.io/workflows/)

## How to control which custom fields to fetch for a user <a href="#how-to-control-which-custom-fields-to-fetch-for-a-user" id="how-to-control-which-custom-fields-to-fetch-for-a-user"></a>

There are three different ways to control which custom fields to retrieve when getting a user's information. Use the **Custom Fields** parameter to select one of the following:

* **Don't Include**: Doesn't include any custom fields.
* **Custom**: Includes the custom fields from schemas in **Custom Schema Names or IDs**.
* **Include All**: Include all the fields associated with the user.

To include custom fields, follow these steps:

1. Select **Custom** from the **Custom Fields** dropdown list.
2. Select the schema names you want to include in the **Custom Schema Names or IDs** dropdown list.
