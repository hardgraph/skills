> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/trigger-nodes/n8n-nodes-base.stripetrigger.md).

# Stripe Trigger

[Stripe](https://stripe.com/) is a suite of payment APIs that powers commerce for online businesses.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/stripe.md).
{% endhint %}

## Webhook authentication <a href="#webhook-authentication" id="webhook-authentication"></a>

From n8n version 2.25.7 and 2.26.2, the Stripe Trigger node can verify that incoming requests genuinely come from Stripe. When you set a **Signature Secret** on your [Stripe credential](/integrations/builtin/credentials/stripe.md), the node checks the `Stripe-Signature` header on each request and rejects any request that's unsigned, forged, or more than five minutes old with a `401 Unauthorized` response. n8n doesn't run your workflow for rejected requests.

Without a **Signature Secret**, the node doesn't verify incoming requests, so anyone who knows your webhook URL could send forged events. n8n strongly recommends setting one. For setup steps, refer to [Verify incoming requests](/integrations/builtin/credentials/stripe.md#verify-incoming-requests).

{% hint style="info" %}
**Examples and templates**

For usage examples and templates to help you get started, refer to n8n's [Stripe Trigger integrations](https://n8n.io/integrations/stripe-trigger/) page.
{% endhint %}
