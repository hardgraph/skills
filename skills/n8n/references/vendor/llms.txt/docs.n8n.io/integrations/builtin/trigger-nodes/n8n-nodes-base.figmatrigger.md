> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.figmatrigger.md).

# Figma Trigger (Beta)

[Figma](https://www.figma.com/) is a prototyping tool which is primarily web-based, with more offline features enabled by desktop applications for macOS and Windows.

{% hint style="warning" %}
**Supported Figma Plans**

Figma doesn't support webhooks on the free "Starter" plan. Your team needs to be on the "Professional" plan to use this node.
{% endhint %}

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/figma.md).
{% endhint %}

{% hint style="info" %}
**Examples and templates**

For usage examples and templates to help you get started, refer to n8n's [Figma Trigger integrations](https://n8n.io/integrations/figma-trigger-beta/) page.
{% endhint %}

## Events <a href="#events" id="events"></a>

* **File Commented**: Triggers when someone comments on a file.
* **File Deleted**: Triggers when someone deletes an individual file, but not when someone deletes an entire folder with all files.
* **File Updated**: Triggers when someone saves or deletes a file. A save occurs when someone closes a file within 30 seconds after making changes.
* **File Version Updated**: Triggers when someone creates a named version in the version history of a file.
* **Library Publish**: Triggers when someone publishes a library file.
