> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/handle-special-data-types/work-with-files-and-images.md).

# Work with files and images

Binary data is any file-type data, such as image files or documents.

This page collects resources relating to binary data in n8n.

## Working with binary data in your workflows <a href="#working-with-binary-data-in-your-workflows" id="working-with-binary-data-in-your-workflows"></a>

You can process binary data in n8n workflows. n8n provides nodes to help you work with binary data. You can also use code.

### Nodes <a href="#nodes" id="nodes"></a>

There are three key nodes dedicated to handling binary data files:

* [Convert to File](/integrations/builtin/core-nodes/n8n-nodes-base.converttofile.md) to take input data and output it as a file.
* [Extract From File](/integrations/builtin/core-nodes/n8n-nodes-base.extractfromfile.md) to get data from a binary format and convert it to JSON.
* [Read/Write Files from Disk](/integrations/builtin/core-nodes/n8n-nodes-base.readwritefile.md) to read and write files from/to the machine where n8n is running.

There are separate nodes for working with XML and HTML data:

* [HTML](/integrations/builtin/core-nodes/n8n-nodes-base.html.md)
* [XML](/integrations/builtin/core-nodes/n8n-nodes-base.xml.md)

And nodes for performing common tasks:

* [Compression](/integrations/builtin/core-nodes/n8n-nodes-base.compression.md)
* [Edit Image](/integrations/builtin/core-nodes/n8n-nodes-base.editimage.md)
* [FTP](/integrations/builtin/core-nodes/n8n-nodes-base.ftp.md)

You can trigger a workflow based on changes to a local file using the [Local File trigger](/integrations/builtin/core-nodes/n8n-nodes-base.localfiletrigger.md).

To split or concatenate binary data items, use the [data transformation nodes](/build/work-with-data/expressions-versus-data-nodes.md#other-data-transformation-nodes).

### Code <a href="#code" id="code"></a>

You can use the [Code node](/build/code-in-n8n/using-the-code-node.md) to manipulate binary data in your workflows. For example, [Get the binary data buffer](/build/code-in-n8n/cookbook/code-node/get-the-binary-data-buffer.md): get the binary data available in your workflow.

## Configure binary data mode when self-hosting <a href="#configure-binary-data-mode-when-self-hosting" id="configure-binary-data-mode-when-self-hosting"></a>

You can configure how your self-hosted n8n instance handles binary data using the [Binary data environment variables](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/binary-data.md). This includes tasks such as setting the storage path and choosing how to store binary data.

Your configuration affects how well n8n scales: [Scaling | Binary data filesystem mode](/deploy/host-n8n/configure-n8n/scaling/handle-binary-data.md).

Reading and writing binary files can have security implications. If you want to disable reading and writing binary data, use the `NODES_EXCLUDE` environment variable. Refer to [Environment variables | Nodes](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/nodes.md) for more information.
