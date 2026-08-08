> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.kobotoolboxtrigger.md).

# KoboToolbox Trigger

[KoboToolbox](https://www.kobotoolbox.org/) is a field survey and data collection tool to design interactive forms to be completed offline from mobile devices. It's available both as a free cloud solution or as a self-hosted version.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/kobotoolbox.md).
{% endhint %}

{% hint style="info" %}
**Examples and templates**

For usage examples and templates to help you get started, refer to n8n's [KoboToolbox Trigger integrations](https://n8n.io/integrations/kobotoolbox-trigger/) page.
{% endhint %}

This node starts a workflow upon new submissions of a specified form. The trigger node handles the creation/deletion of the hook, so you don't need to do any setup in KoboToolbox.

It works the same way as the Get Submission operation in the [KoboToolbox](/integrations/builtin/app-nodes/n8n-nodes-base.kobotoolbox.md) node, including supporting the same reformatting options.
