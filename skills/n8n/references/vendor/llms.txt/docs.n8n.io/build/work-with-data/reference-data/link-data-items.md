> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/reference-data/link-data-items.md).

# Link data items

An item is a single piece of data. Nodes receive one or more items, operate on them, and output new items. Each item links back to the items in the previous nodes that generated it.

Usually this just works. You need to understand this behavior in detail if you're:

* Using the Code node for complex behaviors with input and output data.
* Building a programmatic-style node.

This section provides:

* A conceptual overview of [Item linking concepts](/build/work-with-data/reference-data/link-data-items/how-items-link-through-workflows.md).
* Information on [Item linking for node creators](/build/work-with-data/reference-data/link-data-items/item-linking-for-node-creators.md).
* Support for end users who need to [work with the data path](/build/work-with-data/reference-data/link-data-items/preserving-linking-in-the-code-node.md) to retrieve item data from previous nodes and link items when using the Code node.
* Guidance on troubleshooting [errors](/build/work-with-data/reference-data/link-data-items/item-linking-errors.md).
