> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/signl4.md).

# SIGNL4 credentials

You can use these credentials to authenticate the following nodes:

* [SIGNL4](/integrations/builtin/app-nodes/n8n-nodes-base.signl4.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [SIGNL4](https://www.signl4.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* Webhook secret

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [SIGNL4's Inbound Webhook documentation](https://connect.signl4.com/webhook/docs/index.html) for more information about the service.

## Using webhook secret <a href="#using-webhook-secret" id="using-webhook-secret"></a>

To configure this credential, you'll need:

* A **Team Secret**: SIGNL4 includes this secret in the "✅ Sign up complete" email as the last part of the webhook URL. If your webhook URL is `https://connect.signl4.com/webhook/helloworld`, your team secret would be `helloworld`.
