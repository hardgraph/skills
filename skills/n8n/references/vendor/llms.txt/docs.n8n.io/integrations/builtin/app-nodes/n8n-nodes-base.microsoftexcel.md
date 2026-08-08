> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.microsoftexcel.md).

# Microsoft Excel (OneDrive)

Use the Microsoft Excel (OneDrive) node to automate work in Microsoft Excel, and integrate Microsoft Excel with other applications. n8n has built-in support for a wide range of Microsoft Excel features, including adding and retrieving lists of table data, and workbooks, as well as getting worksheets. The node operates on workbooks stored in OneDrive.

On this page, you'll find a list of operations the Microsoft Excel (OneDrive) node supports and links to more resources.

{% hint style="info" %}
**Credentials**

This node's **Authentication** dropdown offers three options:

* **Excel OAuth2**: the Microsoft Excel-specific OAuth2 credential (default).
* **Microsoft OAuth2 (Graph)**: a generic Microsoft Graph credential that you can reuse across other Microsoft nodes. When you select this option, make sure you grant the credential the scopes this node needs (for example, `Files.ReadWrite`, or `Files.ReadWrite.All` if that's the permission your administrator has consented).
* **Microsoft Entra Service Principal (App-Only)**: app-only access through a Microsoft Entra app registration, with no signed-in user, available from version 2 of the node. Refer to [Microsoft Entra Service Principal credentials](/integrations/builtin/credentials/microsoftentraserviceprincipal.md) for setup and required application permissions.

Refer to [Microsoft credentials](/integrations/builtin/credentials/microsoft.md) for guidance on setting up authentication.
{% endhint %}

{% hint style="info" %}
**Government Cloud Support**

If you're using a government cloud tenant (US Government, US Government DOD, or China), make sure to select the appropriate **Microsoft Graph API Base URL** in your Microsoft credentials configuration.
{% endhint %}

{% hint style="info" %}
**This node can be used as an AI tool**

This node can be used to enhance the capabilities of an AI agent. When used in this way, many parameters can be set automatically, or with information directed by AI - find out more in the [AI tool parameters documentation](/build/integrate-ai/ai-examples/use-ai-for-parameters.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* Table
  * Adds rows to the end of the table
  * Retrieve a list of table columns
  * Retrieve a list of table rows
  * Looks for a specific column value and then returns the matching row
* Workbook
  * Adds a new worksheet to the workbook.
  * Get data of all workbooks
* Worksheet
  * Get all worksheets
  * Get worksheet content

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Microsoft Excel (OneDrive) node documentation integration templates](https://n8n.io/integrations/microsoft-excel) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
