> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/webhook.md).

# Webhook credentials

You can use these credentials to authenticate the following nodes:

* [Webhook](/integrations/builtin/core-nodes/n8n-nodes-base.webhook.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

You must use the authentication method required by the app or service you want to query.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* Basic auth
* Header auth
* JWT auth
* None

Use this generic authentication if your app or service supports basic authentication.

To configure this credential, enter:

* The **Username** you use to access the app or service your HTTP Request is targeting
* The **Password** that goes with that username

Use this generic authentication if your app or service supports header authentication.

To configure this credential, enter:

* The header **Name** you need to pass to the app or service your HTTP request is targeting
* The **Value** for the header

Read more about [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#authentication)

## Using JWT auth <a href="#using-jwt-auth" id="using-jwt-auth"></a>

[**JWT Auth**](https://jwt.io/introduction/) is a method of authentication that uses JSON Web Tokens (JWT) to digitally sign data. This authentication method uses the **JWT credential** and can use either a **Passphrase** or **PEM Key** as key type. Refer to [JWT credential](/integrations/builtin/credentials/jwt.md) for more information.
