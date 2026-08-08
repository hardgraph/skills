> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.boxtrigger.md).

# Box Trigger

[Box](https://www.box.com/) is a cloud computing company which provides file sharing, collaborating, and other tools for working with files uploaded to its servers.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/box.md).
{% endhint %}

{% hint style="info" %}
**Examples and templates**

For usage examples and templates to help you get started, refer to n8n's [Box Trigger integrations](https://n8n.io/integrations/box-trigger/) page.
{% endhint %}

## Find your Box Target ID <a href="#find-your-box-target-id" id="find-your-box-target-id"></a>

To get your Target ID in Box:

1. Open the file/folder that you would like to monitor.
2. Copy the string of characters after `folder/` in your URL. This is the target ID. For example, if the URL is `https://app.box.com/folder/12345`, then `12345` is the target ID.
3. Paste it in the **Target ID** field in n8n.
