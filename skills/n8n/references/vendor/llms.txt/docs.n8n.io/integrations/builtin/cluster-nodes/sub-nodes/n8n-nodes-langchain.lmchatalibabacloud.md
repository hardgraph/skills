> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/integrations/builtin/cluster-nodes/sub-nodes/n8n-nodes-langchain.lmchatalibabacloud.md).

# Qwen Cloud Chat Model

The Qwen Cloud Chat Model node sends chat prompts to conversational models available on Qwen Cloud, for advanced AI chains and LangChain integrations. Use it to generate conversational responses, integrate model outputs into workflows, or run prompts with custom sampling, retry, and timeout settings.

{% hint style="info" %}
**Credentials**

You can find authentication information for this node [here](/integrations/builtin/credentials/alibaba.md).
{% endhint %}

{% hint style="info" %}
**Parameter resolution in sub-nodes**

Sub-nodes behave differently to other nodes when processing multiple items using an expression.

Most nodes, including root nodes, take any number of items as input, process these items, and output the results. You can use expressions to refer to input items, and the node resolves the expression for each item in turn. For example, given an input of five `name` values, the expression `{{ $json.name }}` resolves to each name in turn.

In sub-nodes, the expression always resolves to the first item. For example, given an input of five `name` values, the expression `{{ $json.name }}` always resolves to the first name.
{% endhint %}

## Operations <a href="#operations" id="operations"></a>

### Generate chat response <a href="#generate-chat-response" id="generate-chat-response"></a>

Generate a chat-style response from the selected Qwen Cloud model.

**Parameters**

* **Model** (type: *options*, field: `model`): The model that generates the completion. Learn more about available models on Qwen Cloud: [Choose models](https://docs.qwencloud.com/developer-guides/getting-started/model-selection).

**Options**

* **Frequency Penalty** (type: *number*, field: `frequencyPenalty`): Positive values penalize new tokens based on how often they appear so far, decreasing the model's likelihood to repeat the same line verbatim. Default: `0`.
* **Maximum Number of Tokens** (type: *number*, field: `maxTokens`): The maximum number of tokens to generate in the completion. The limit depends on the selected model. A value of minus one uses the model's default limit. Default: `-1`.
* **Response Format** (type: *options*, field: `responseFormat`): The output format returned by the node, for example plain text or structured formats. Default: text.
* **Presence Penalty** (type: *number*, field: `presencePenalty`): Positive values penalize new tokens based on whether they appear in the text so far, increasing the model's likelihood to discuss new topics. Default: `0`.
* **Sampling Temperature** (type: *number*, field: `temperature`): Control randomness. Lower values make output less random, near zero is deterministic. Default: `0.7`.
* **Timeout** (type: *number*, field: `timeout`): Maximum time (in milliseconds) allowed for a request before it's aborted. Default: `360000`.
* **Max Retries** (type: *number*, field: `maxRetries`): Maximum number of retry attempts for failed requests. Default: `2`.
* **Top P** (type: *number*, field: `topP`): Nucleus sampling parameter that controls diversity. 0.5 means half of the probability mass is considered. Adjust **Top P** or **Sampling Temperature**, but not both. Default: `1`.

## Templates and examples <a href="#templates-and-examples" id="templates-and-examples"></a>

[Browse Qwen Cloud Chat Model node documentation integration templates](https://n8n.io/integrations/alibaba-cloud-chat-model) or [search all templates](https://n8n.io/workflows/)

## Related resources <a href="#related-resources" id="related-resources"></a>

Refer to [Choose models](https://docs.qwencloud.com/developer-guides/getting-started/model-selection) for more information about available models and their capabilities.
