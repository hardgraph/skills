> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/invoiceninja.md).

# Invoice Ninja credentials

You can use these credentials to authenticate the following nodes:

* [Invoice Ninja](/integrations/builtin/app-nodes/n8n-nodes-base.invoiceninja.md)
* [Invoice Ninja Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.invoiceninjatrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create an [Invoice Ninja](https://www.invoiceninja.com/) account. Only the Pro and Enterprise plans support API integrations.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API key

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to Invoice Ninja's [v4 API documentation](https://invoice-ninja.readthedocs.io/en/latest/api.html) and [v5 API documentation](https://api-docs.invoicing.co/) for more information about the APIs.

## Using API key <a href="#using-api-key" id="using-api-key"></a>

To configure this credential, you'll need:

* A **URL**: If Invoice Ninja hosts your installation, use either of the default URLs mentioned. If you're self-hosting your installation, use the URL of your Invoice Ninja instance.
* An **API Token**: Generate an API token in **Settings > Account Management > API Tokens**.
* An optional **Secret**, available only for v5 API users
