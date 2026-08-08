> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/mailchimp.md).

# Mailchimp credentials

You can use these credentials to authenticate the following nodes:

* [Mailchimp](/integrations/builtin/app-nodes/n8n-nodes-base.mailchimp.md)
* [Mailchimp Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.mailchimptrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Mailchimp](https://www.mailchimp.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API key
* OAuth2

Refer to [Selecting an authentication method](#selecting-an-authentication-method) for guidance on which method to use.

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Mailchimp's API documentation](https://mailchimp.com/developer/marketing/api/) for more information about the service.

## Using API key <a href="#using-api-key" id="using-api-key"></a>

To configure this credential, you'll need:

* An **API Key**: Generate an API key in the [API keys section](https://us1.admin.mailchimp.com/account/api/) of your Mailchimp account. Refer to [Mailchimp's Generate your API key documentation](https://mailchimp.com/developer/marketing/guides/quick-start/#generate-your-api-key) for more detailed instructions.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you need to configure OAuth2 from scratch, [register an application](https://mailchimp.com/developer/marketing/guides/access-user-data-oauth-2/#register-your-application). Refer to the [Mailchimp OAuth2 documentation](https://mailchimp.com/developer/marketing/guides/access-user-data-oauth-2/) for more information.

## Selecting an authentication method <a href="#selecting-an-authentication-method" id="selecting-an-authentication-method"></a>

Mailchimp suggests using an API key if you're only accessing your own Mailchimp account's data:

> Use an API key if you're writing code that tightly couples *your* application's data to *your* Mailchimp account's data. If you ever need to access *someone else's* Mailchimp account's data, you should be using OAuth 2 ([source](https://mailchimp.com/developer/marketing/guides/access-user-data-oauth-2/#when-not-to-use-oauth-2))
