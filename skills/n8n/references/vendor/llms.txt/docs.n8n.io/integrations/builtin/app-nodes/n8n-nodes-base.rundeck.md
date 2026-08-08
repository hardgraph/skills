> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.rundeck.md).

# Rundeck

Use the Rundeck node to automate work in Rundeck, and integrate Rundeck with other applications. n8n has built-in support for executing jobs and getting metadata.

On this page, you'll find a list of operations the Rundeck node supports and links to more resources.

{% hint style="info" %}
**Credentials**

Refer to [Rundeck credentials](/integrations/builtin/credentials/rundeck.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* **Job**
  * Execute a job
  * Get metadata of a job

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Rundeck node documentation integration templates](https://n8n.io/integrations/rundeck) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.

## Find the job ID <a href="#find-the-job-id" id="find-the-job-id"></a>

1. Access your Rundeck dashboard.
2. Open the project that contains the job you want to use with n8n.
3. In the sidebar, select **JOBS**.
4. Under **All Jobs**, select the name of the job you want to use with n8n.
5. In the top left corner, under the name of the job, copy the string that's displayed in smaller font below the job name. This is your job ID.
6. Paste this job ID in the **Job Id** field in n8n.
