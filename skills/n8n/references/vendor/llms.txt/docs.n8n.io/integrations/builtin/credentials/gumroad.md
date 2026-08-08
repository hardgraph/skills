> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/gumroad.md).

# Gumroad credentials

You can use these credentials to authenticate the following nodes:

* [Gumroad Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.gumroadtrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Gumroad](https://gumroad.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API access token
* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Gumroad's API documentation](https://app.gumroad.com/api) for more information about the service.

## Using API access token <a href="#using-api-access-token" id="using-api-access-token"></a>

To configure this credential, you'll need:

* An API **Access Token**: Create an application to generate an access token. Refer to the [Gumroad Create an application for the API documentation](https://gumroad.com/help/article/280-create-application-api) for detailed instructions on creating a new application and generating an access token.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you're [self-hosting n8n](/deploy/host-n8n.md), you'll need:

* An **OAuth Redirect URL**
* A **Client ID**
* A **Client Secret**

To get this information, create a Gumroad application:

1. In Gumroad, go to **Settings > Advanced**. Refer to the [Gumroad Create an application for the API documentation](https://gumroad.com/help/article/280-create-application-api) for detailed instructions.
2. Copy the **OAuth Redirect URL** from your n8n credential and enter it as the **Redirect URI** when you create the application in Gumroad.
3. Create the application. Gumroad generates an **Application ID** and **Application Secret**.
4. Copy the **Application ID** and paste it into the **Client ID** in your n8n credential.
5. Copy the **Application Secret** and paste it into the **Client Secret** in your n8n credential.
6. To request scopes beyond the default `view_sales`, enable **Custom Scopes** in your n8n credential and enter the scopes you need. Otherwise, leave it off.
