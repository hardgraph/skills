> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/eventbrite.md).

# Eventbrite credentials

You can use these credentials to authenticate the following nodes:

* [Eventbrite Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.eventbritetrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create an [Eventbrite](https://www.eventbrite.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API private key
* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Eventbrite's API documentation](https://www.eventbrite.com/platform/api) for more information about the service.

## Using API private key <a href="#using-api-private-key" id="using-api-private-key"></a>

To configure this credential, you'll need:

* A **Private Key**: Refer to the [Eventbrite API Authentication Get a Private Token documentation](https://www.eventbrite.com/platform/api#/introduction/authentication/1.-get-a-private-token) for detailed steps to generate a Private Token. Use this private token as the **Private Key** in the n8n credential.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you need to configure OAuth2 from scratch or need more detail on what's happening in the OAuth web flow, refer to the instructions in the [Eventbrite API authentication For App Partners documentation](https://www.eventbrite.com/platform/api#/introduction/authentication/2.-\(for-app-partners\)-authorize-your-users) to set up OAuth.
