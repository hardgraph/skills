> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/readme.md).

# Nodes

n8n calls integrations nodes.

Nodes are the building blocks of workflows in n8n. They're an entry point for retrieving data, a function to process data, or an exit for sending data. The data process includes filtering, recomposing, and changing data. There can be one or several nodes for your API, service or app. You can connect multiple nodes, which allows you to create complex workflows.

## Built-in nodes <a href="#built-in-nodes" id="built-in-nodes"></a>

n8n includes a collection of built-in integrations. Refer to [Built-in nodes](/integrations/builtin/node-types.md) for documentation on all n8n's built-in nodes.

## Community nodes <a href="#community-nodes" id="community-nodes"></a>

As well as using the built-in nodes, you can also install community-built nodes. Refer to [Community nodes](/integrations/community-nodes/installation-and-management.md) for more information.

## Credential-only nodes and custom operations <a href="#credential-only-nodes-and-custom-operations" id="credential-only-nodes-and-custom-operations"></a>

One of the most complex parts of setting up [API](/key-concept-glossary.md#api) calls is managing authentication. n8n provides [credentials](/key-concept-glossary.md#credential-n8n) support for operations and services beyond those supported by built-in nodes.

* Custom operations for existing nodes: n8n supplies hundreds of nodes to create workflows that link multiple products. However, some nodes don't include all the possible operations supported by a product's API. You can work around this by making a custom API call using the [HTTP Request](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) node.
* Credential-only nodes: n8n includes credential-only nodes. These are integrations where n8n supports setting up credentials for use in the HTTP Request node, but doesn't provide a standalone node. You can find a credential-only node in the nodes panel, as you would for any other integration.

Refer to [Custom operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) for more information.

## Generic integrations <a href="#generic-integrations" id="generic-integrations"></a>

If you need to connect to a service where n8n doesn't have a node, or a credential-only node, you can still use the [HTTP Request](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) node. Refer to the node page for details on how to set up authentication and create your API call.

## Where to go next <a href="#where-to-go-next" id="where-to-go-next"></a>

* If you want to create your own node, head over to the [Creating Nodes](/connect/create-nodes/overview.md) section.
* Check out [Community nodes](/integrations/community-nodes/using-community-nodes.md) to learn about installing and managing community-built nodes.
* If you'd like to learn more about the different nodes in n8n, their functionalities and example usage, check out n8n's node libraries: [Core nodes](/integrations/builtin/core-nodes.md), [Actions](/integrations/builtin/app-nodes.md), and [Triggers](/integrations/builtin/trigger-nodes.md).
* If you'd like to learn how to add the credentials for the different nodes, head over to the [Credentials](/integrations/builtin/credentials.md) section.
