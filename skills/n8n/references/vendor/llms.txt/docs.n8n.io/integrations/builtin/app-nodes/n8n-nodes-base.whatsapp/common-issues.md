> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.whatsapp/common-issues.md).

# Common issues

Here are some common errors and issues with the [WhatsApp Business Cloud node](/integrations/builtin/app-nodes/n8n-nodes-base.whatsapp.md) and steps to resolve or troubleshoot them.

## Bad request - please check your parameters <a href="#bad-request-please-check-your-parameters" id="bad-request-please-check-your-parameters"></a>

This error occurs when WhatsApp Business Cloud rejects your request because of a problem with its parameters. It's common to see this when using the **Send Template** operation if the data you send doesn't match the format of your template.

To resolve this issue, review the parameters in your [message template](https://www.facebook.com/business/help/2055875911147364?id=2129163877102343). Pay attention to each parameter's data type and the order they're defined in the template.

Check the data that n8n is mapping to the template parameters. If you're using expressions to set parameter values, check the input data to make sure each item resolves to a valid value. You may want to use the [Edit Fields (Set) node](/integrations/builtin/core-nodes/n8n-nodes-base.set.md) or set a fallback value to ensure you send a value with the correct format.

## Working with non-text media <a href="#working-with-non-text-media" id="working-with-non-text-media"></a>

The WhatsApp Business Cloud node can work with non-text messages and media like images, audio, documents, and more.

If your operation includes a **Input Data Field Name** or **Property Name** parameter, set this to the field name itself rather than referencing the data in an expression.

For example, if you are trying to send a message with an "Image" **MessageType** and **Take Image From** set to "n8n", set **Input Data Field Name** to a field name like `data` instead of an expression like `{{ $json.input.data }}`.
