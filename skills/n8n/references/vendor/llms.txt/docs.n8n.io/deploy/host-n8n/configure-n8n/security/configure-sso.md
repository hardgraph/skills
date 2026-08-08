> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/deploy/host-n8n/configure-n8n/security/configure-sso.md).

# Configure SSO

{% hint style="info" %}
**Feature availability**

* Available on Business and Enterprise plans.
* You need to be an instance owner or admin to enable and configure SAML or OIDC.
  {% endhint %}

n8n supports the SAML and OIDC authentication protocols for single sign-on (SSO). See [OIDC vs SAML](https://www.onelogin.com/learn/oidc-vs-saml) for more general information on the two protocols, the differences between them, and their respective benefits.

* [Set up SAML](/administer/manage-users-and-access/verify-user-identity/use-saml/set-up-saml.md): a general guide to setting up SAML in n8n, and links to resources for common identity providers (IdPs).
* [Set up OIDC](/administer/manage-users-and-access/verify-user-identity/use-oidc/set-up-oidc.md): a general guide to setting up OpenID Connect (OIDC) SSO in n8n.

## Configure SSO with environment variables <a href="#configure-sso-with-environment-variables" id="configure-sso-with-environment-variables"></a>

You can also configure SSO from environment variables instead of through the UI. Available from n8n v2.18.0. See [SSO environment variables](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/sso.md) for the full list of variables, and [Manage instance settings using environment variables](/deploy/host-n8n/configure-n8n/manage-settings-using-environment-variables.md) for how the activation pattern works.
