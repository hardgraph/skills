> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes.md).

# Cluster nodes

[Cluster nodes](/key-concept-glossary.md#cluster-node-n8n) are node groups that work together to provide functionality in an n8n workflow. Instead of using a single node, you use a [root node](/key-concept-glossary.md#root-node-n8n) and one or more [sub-nodes](/key-concept-glossary.md#sub-node-n8n) that extend the functionality of the node.

![Screenshot of a workflow with a root node and two sub-nodes](/spaces/GixZThfitWP21x2gQFpD/files/GbA7RoHTpD75RG5oDkF8)

## Root nodes <a href="#root-nodes" id="root-nodes"></a>

Each cluster starts with one [root node](#user-content-fn-1)[^1].

## Sub-nodes <a href="#sub-nodes" id="sub-nodes"></a>

Each root node can have one or more sub-nodes[^2] attached to it.

[^1]: Each n8n cluster node contains a single root nodes that defines the main functionality of the cluster. One or more sub nodes attach to the root node to extend its functionality.

[^2]: n8n cluster nodes consist of one or more sub nodes connected to a root node. Sub nodes extend the functionality of the root node, providing access to specific services or resources or offering specific types of dedicated processing, like calculator functionality, for example.
