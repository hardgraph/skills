> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/code-in-n8n/cookbook/built-in-methods-and-variables-examples/execution.md).

# execution

## `execution.id` <a href="#executionid" id="executionid"></a>

Contains the unique ID of the current workflow execution.

{% tabs %}
{% tab title="JavaScript" %}

```js
let executionId = $execution.id;
```

{% endtab %}

{% tab title="Python" %}

```python
executionId = _execution.id
```

{% endtab %}
{% endtabs %}

## `execution.resumeUrl` <a href="#executionresumeurl" id="executionresumeurl"></a>

The webhook URL to call to resume a [waiting](/integrations/builtin/core-nodes/n8n-nodes-base.wait.md) workflow.

See the [Wait > On webhook call](/integrations/builtin/core-nodes/n8n-nodes-base.wait.md#on-webhook-call) documentation to learn more.

`execution.resumeUrl` is available in workflows containing a Wait node, along with a node that waits for a webhook response.

## `execution.customData` <a href="#executioncustomdata" id="executioncustomdata"></a>

This is only available in the Code node.

{% tabs %}
{% tab title="JavaScript" %}

```js
// Set a single piece of custom execution data
$execution.customData.set("key", "value");

// Set the custom execution data object
$execution.customData.setAll({"key1": "value1", "key2": "value2"})

// Access the current state of the object during the execution
var customData = $execution.customData.getAll()

// Access a specific value set during this execution
var customData = $execution.customData.get("key")
```

{% endtab %}

{% tab title="Python" %}

```python
# Set a single piece of custom execution data
_execution.customData.set("key", "value");

# Set the custom execution data object
_execution.customData.setAll({"key1": "value1", "key2": "value2"})

# Access the current state of the object during the execution
customData = _execution.customData.getAll()

# Access a specific value set during this execution
customData = _execution.customData.get("key")
```

{% endtab %}
{% endtabs %}

Refer to [Custom executions data](/build/understand-workflows/understand-executions/customize-executions-data.md) for more information.

***
