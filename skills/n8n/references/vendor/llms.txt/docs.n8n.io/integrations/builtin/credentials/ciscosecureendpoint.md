> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/ciscosecureendpoint.md).

# Cisco Secure Endpoint credentials

You can use these credentials to authenticate when using the [HTTP Request node](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md) to make a [Custom API call](/integrations/builtin/custom-api-actions-for-existing-nodes.md).

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* Create a [Cisco DevNet developer account](https://developer.cisco.com).
* Access to a [Cisco Secure Endpoint license](https://www.cisco.com/site/us/en/products/security/endpoint-security/secure-endpoint/index.html).

## Authentication methods <a href="#authentication-methods" id="authentication-methods"></a>

* OAuth2

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Cisco Secure Endpoint's documentation](https://developer.cisco.com/docs/secure-endpoint/introduction/) for more information about the service.

This is a credential-only node. Refer to [Custom API operations](/integrations/builtin/custom-api-actions-for-existing-nodes.md) to learn more. View [example workflows and related content](https://n8n.io/integrations/cisco-secure-endpoint/) on n8n's website.

## Using OAuth2 <a href="#using-oauth2" id="using-oauth2"></a>

To configure this credential, you'll need:

* The **Region** for your Cisco Secure Endpoint. Options are:
  * Asia Pacific, Japan, and China
  * Europe
  * North America
* A **Client ID**: Provided when you register a SecureX API Client
* A **Client Secret**: Provided when you register a SecureX API Client

To get a Client ID and Client Secret, you'll need to Register a SecureX API Client. Refer to [Cisco Secure Endpoint's authentication documentation](https://developer.cisco.com/docs/secure-endpoint/authentication/#authentication) for detailed instructions. Use the SecureX **Client Password** as the **Client Secret** within the n8n credential.
