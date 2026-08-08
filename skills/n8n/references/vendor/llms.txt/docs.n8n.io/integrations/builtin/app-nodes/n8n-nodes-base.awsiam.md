> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.awsiam.md).

# AWS IAM

Use the AWS IAM node to automate work in AWS Identity and Access Management (IAM) and integrate AWS IAM with other applications. n8n has built-in support for a wide range of AWS IAM features, which includes creating, updating, getting and deleting users and groups as well as managing group membership.

On this page, you'll find a list of operations the AWS IAM node supports, and links to more resources.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/aws.md).
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

* **User**:
  * **Add to Group**: Add an existing user to a group.
  * **Create**: Create a new user.
  * **Delete**: Delete a user.
  * **Get**: Retrieve a user.
  * **Get Many**: Retrieve a list of users.
  * **Remove From Group**: Remove a user from a group.
  * **Update**: Update an existing user.
* **Group**:
  * **Create**: Create a new group.
  * **Delete**: Create a new group.
  * **Get**: Retrieve a group.
  * **Get Many**: Retrieve a list of groups.
  * **Update**: Update an existing group.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse AWS IAM node documentation integration templates](https://n8n.io/integrations/aws-iam) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/APIReference/welcome.html) for more information about the service.

If this node doesn't support the operation you want to do, you can use the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to call the service's API.

You can use the credential you created for this service in the HTTP Request node:

1. In the HTTP Request node, select **Authentication** > **Predefined Credential Type**.
2. Select the service you want to connect to.
3. Select your credential.

Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.
