> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/manage-workflows/export-and-import.md).

# Export and import

n8n saves workflows in JSON format. You can export your workflows as JSON files or import JSON files into your n8n library. You can export and import workflows in several ways.

{% hint style="warning" %}
**Sharing credentials**

Exported workflow JSON files include [credential](/key-concept-glossary.md#credential-n8n) names and IDs. While IDs aren't sensitive, the names could be, depending on how you name your credentials. HTTP Request nodes may contain authentication headers when imported from cURL. Remove or anonymize this information from the JSON file before sharing to protect your credentials.
{% endhint %}

## Copy-Paste <a href="#copy-paste" id="copy-paste"></a>

You can copy and paste a workflow or parts of it by selecting the nodes you want to copy to the clipboard (`Ctrl + c` or `cmd +c`) and pasting it (`Ctrl + v` or `cmd + v`) into the Editor UI.

To select all nodes or a group of nodes, click and drag: ![Select a group of nodes](/files/7425mWkjYp3jepo9pyjQ)

## From the Editor UI menu <a href="#from-the-editor-ui-menu" id="from-the-editor-ui-menu"></a>

From the top navigation bar, select the three dots in the upper right <img src="/files/SR2ZhKMS3DaJPAVNcfpZ" alt="Workflow menu icon" data-size="line"> to see the following options:

* **Download**: Downloads your current workflow as a JSON file to your computer.
* **Import from URL**: Imports workflow JSON from a URL, for example, [this workflow JSON file on GitHub](https://raw.githubusercontent.com/n8n-io/self-hosted-ai-starter-kit/refs/heads/main/n8n/demo-data/workflows/srOnR8PAY3u4RSwb.json).
* **Import from File**: Imports a workflow as a JSON file from your computer.

## From the command line <a href="#from-the-command-line" id="from-the-command-line"></a>

### Using the n8n CLI

The `n8n-cli package` commands bundle workflows into a portable `.n8np` file you can import into another instance. See [n8n packages](/build/manage-workflows/n8n-packages.md), and [Export a package](/build/manage-workflows/n8n-packages/export-a-package.md) for the available options. n8n packages are in preview and may change in future releases.

{% hint style="info" %}
The n8n CLI is the method n8n recommends for moving workflows between instances from the command line. It runs from any machine with network access, carries the folders, projects, and references a workflow needs, and checks an import before writing anything.
{% endhint %}

### Using the Server CLI

* Export: See the [export commands](/deploy/host-n8n/configure-n8n/use-the-command-line.md#export-entities) for exporting workflows or credentials.
* Import: See the [import commands](/deploy/host-n8n/configure-n8n/use-the-command-line.md#import-entities) for importing workflows or credentials.

{% hint style="warning" %}
**Feature availability**

n8n plans to deprecate the Server CLI export and import commands. No deprecation or removal version is scheduled yet. Use the [n8n CLI](#using-the-n8n-cli) for new work.
{% endhint %}

## From the n8n API

The n8n API can export and import [n8n packages](/build/manage-workflows/n8n-packages.md), which carry workflows along with the folders, projects, and references they need. n8n packages are in preview and may change in future releases.

To move a single workflow as JSON instead, use the workflow endpoints in the [n8n API](/connect/n8n-api.md).
