> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/milvus.md).

# Milvus credentials

You can use these credentials to authenticate the following nodes:

* [Milvus Vector Store](/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.vectorstoremilvus.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create and run an [Milvus](https://milvus.io/) instance. Refer to the [Install Milvus](https://milvus.io/docs/install-overview.md) for more information.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* Basic auth

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Milvus's Authentication documentation](https://milvus.io/docs/authenticate.md?tab=docker#Authenticate-User-Access) for more information about setting up authentication.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

## Using basic auth <a href="#using-basic-auth" id="using-basic-auth"></a>

To configure this credential, you'll need:

* **Base URL**: The base URL of your Milvus instance. The default is `http://localhost:19530`.
* **Username**: The username to authenticate to your Milvus instance. The default value is `root`.
* **Password**: The password to authenticate to your Milvus instance. The default value is `Milvus`.
