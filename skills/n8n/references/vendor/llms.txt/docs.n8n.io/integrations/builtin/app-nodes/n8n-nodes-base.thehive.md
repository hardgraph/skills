> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.thehive.md).

# TheHive

Use the TheHive node to automate work in TheHive, and integrate TheHive with other applications. n8n has built-in support for a wide range of TheHive features, including creating alerts, counting tasks logs, cases, and observables.

On this page, you'll find a list of operations the TheHive node supports and links to more resources.

{% hint style="info" %}
**TheHive and TheHive 5**

n8n provides two nodes for TheHive. Use this node (TheHive) if you want to use TheHive's version 3 or 4 API. If you want to use version 5, use [TheHive 5](/integrations/builtin/app-nodes/n8n-nodes-base.thehive5.md).
{% endhint %}

{% hint style="info" %}
**Credentials**

Refer to [TheHive credentials](/integrations/builtin/credentials/thehive.md) for guidance on setting up authentication.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

The available operations depend on your API version. To see the operations list, create your credentials, including selecting your API version. Then return to the node, select the resource you want to use, and n8n displays the available operations for your API version.

* Alert
* Case
* Log
* Observable
* Task

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse TheHive node documentation integration templates](https://n8n.io/integrations/thehive) or [search all templates](https://n8n.io/workflows/)

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.

## Related resources <a href="#related-resources" id="related-resources"></a>

n8n provides a trigger node for TheHive. You can find the trigger node docs [here](/integrations/builtin/trigger-nodes/n8n-nodes-base.thehivetrigger.md).

Refer to TheHive's documentation for more information about the service:

* [Version 3](https://docs.thehive-project.org/thehive/legacy/thehive3/api/)
* [Version 4](https://docs.thehive-project.org/cortex/api/api-guide/)
