> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/node-types.md).

# Node types

This section contains the node[^1] library: reference documentation for every built-in node in n8n, and their credentials.

### Node operations: Triggers and Actions

When you add a node to a workflow, n8n displays a list of available operations. An operation is something a node does, such as getting or sending data.

There are two types of operation:

* Triggers start a workflow in response to specific events or conditions in your services. When you select a Trigger, n8n adds a trigger node to your workflow, with the Trigger operation you chose pre-selected. When you search for a node in n8n, Trigger operations have a bolt icon <img src="/spaces/GixZThfitWP21x2gQFpD/files/YhXfixCZho94eabo1r6o" alt="Trigger icon" data-size="line">.
* Actions are operations that represent specific tasks within a workflow, which you can use to manipulate data, perform operations on external systems, and trigger events in other systems as part of your workflows. When you select an Action, n8n adds a node to your workflow, with the Action operation you chose pre-selected.

## Core nodes <a href="#core-nodes" id="core-nodes"></a>

Core nodes can be actions or triggers[^2]. Whereas most nodes connect to a specific external service, core nodes provide functionality such as logic, scheduling, or generic API calls.

## Cluster nodes <a href="#cluster-nodes" id="cluster-nodes"></a>

[Cluster nodes](/key-concept-glossary.md#cluster-node-n8n) are node groups that work together to provide functionality in an n8n workflow. Instead of using a single node, you use a [root node](/key-concept-glossary.md#root-node-n8n) and one or more [sub-nodes](/key-concept-glossary.md#sub-node-n8n) that extend the functionality of the node.

![Screenshot of a workflow with a root node and two sub-nodes](/spaces/GixZThfitWP21x2gQFpD/files/GbA7RoHTpD75RG5oDkF8)

## Credentials <a href="#credentials" id="credentials"></a>

External services need a way to identify and authenticate users. This data can range from an API key over an email/password combination to a long multi-line private key. You can save these in n8n as credentials[^3].

Nodes in n8n can then request that credential information. As another layer of security, only node types with specific access rights can access the credentials.

To make sure that the data is secure, it gets saved to the database encrypted. n8n uses a random personal encryption key, which it automatically generates on the first run of n8n and then saved under `~/.n8n/config`.

To learn more about creating, managing, and sharing credentials, refer to [Manage credentials](/build/understand-workflows/create-and-edit-credentials.md).

## Community nodes <a href="#community-nodes" id="community-nodes"></a>

n8n supports custom nodes built by the community. Refer to [Community nodes](/integrations/community-nodes/installation-and-management.md) for guidance on installing and using these nodes.

For help building your own custom nodes, and publish them to [npm](https://www.npmjs.com/), refer to [Creating nodes](/connect/create-nodes/overview.md) for more information.

[^1]: In n8n, nodes are individual components that you compose to create workflows. Nodes define when the workflow should run, allow you to fetch, send, and process data, can define flow control logic, and connect with external services.

[^2]: A trigger node is a special node responsible for executing the workflow in response to certain conditions. All production workflows need at least one trigger to determine when the workflow should run.

[^3]: In n8n, credentials store authentication information to connect with specific apps and services. After creating credentials with your authentication information (username and password, API key, OAuth secrets, etc.), you can use the associated app node to interact with the service.
