> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/freshservice.md).

# Freshservice credentials

You can use these credentials to authenticate the following nodes:

* [Freshservice](/integrations/builtin/app-nodes/n8n-nodes-base.freshservice.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Freshservice](https://freshservice.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API key

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Freshservice's API documentation](https://api.freshservice.com/v2/) for more information about the service.

## Using API key <a href="#using-api-key" id="using-api-key"></a>

To configure this credential, you'll need:

* An **API Key**: Refer to the [Freshservice API authenticaton documentation](https://api.freshservice.com/v2/#authentication) for detailed instructions on getting your API key.
* Your Freshservice **Domain**: Use the subdomain of your Freshservice account. This is part of the URL, for example `https://<subdomain>.freshservice.com`. So if you access Freshservice through `https://n8n.freshservice.com`, enter `n8n` as your **Domain**.
