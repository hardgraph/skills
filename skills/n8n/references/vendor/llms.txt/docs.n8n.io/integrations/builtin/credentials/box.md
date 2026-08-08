> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/box.md).

# Box credentials

You can use these credentials to authenticate the following nodes:

* [Box](/integrations/builtin/app-nodes/n8n-nodes-base.box.md)
* [Box Trigger](/integrations/builtin/trigger-nodes/n8n-nodes-base.boxtrigger.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Box](https://www.box.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Box's API documentation](https://developer.box.com/reference/) for more information about the service.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

{% hint style="info" %}
**Note for n8n Cloud users**

Cloud users don't need to provide connection details. Select **Connect my account** to connect through your browser.
{% endhint %}

If you need to configure OAuth2 from scratch or need more detail on what's happening in the OAuth web flow, you'll need to create a Custom App. Refer to the [Box OAuth2 Setup documentation](https://developer.box.com/guides/authentication/oauth2/oauth2-setup/) for more information.
