> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/credentials/alibaba.md).

# Qwen Cloud credentials

You can use these credentials to authenticate the following nodes:

* [Qwen Cloud](/integrations/builtin/app-nodes/n8n-nodes-langchain.alibabacloud.md)
* [Qwen Cloud Chat Model](/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatalibabacloud.md)

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Create a [Qwen Cloud](https://qwencloud.com/) account.

## Supported authentication methods <a href="#supported-authentication-methods" id="supported-authentication-methods"></a>

* API key

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Qwen Cloud API key documentation](https://docs.qwencloud.com/developer-guides/administration/api-keys) for more information about the service.

View n8n's [Advanced AI](/build/integrate-ai.md) documentation.

## Using API key <a href="#using-api-key" id="using-api-key"></a>

To configure this credential, you'll need:

* An **API Key**

1. Sign in to [Qwen Cloud](https://qwencloud.com/).
2. Go to **API Keys**.
3. Use the workspace switcher at the bottom of the sidebar to select the workspace where you want to create the key.
4. Select **Create API key**.
5. Enter a description, then select **Generate Key**.
6. Copy the API key. It displays only once.
7. Enter the API key in your n8n credential.
8. For Qwen Cloud accounts, set **Region** to **Singapore**. Select another region only if you're using Alibaba Cloud Model Studio.

Refer to [Qwen Cloud API key documentation](https://docs.qwencloud.com/developer-guides/administration/api-keys) for more information.
