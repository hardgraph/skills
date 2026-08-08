> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/code-in-n8n/cookbook/code-node/get-number-of-items-returned-by-last-node.md).

# Get number of items returned by last node

To get the number of items returned by the previous node:

{% tabs %}
{% tab title="JavaScript" %}

```js
if (Object.keys(items[0].json).length === 0) {
return [
	{
		json: {
			results: 0,
		}
	}
]
}
return [
	{
		json: {
			results: items.length,
		}
	}
];
```

The output will be similar to the following.

```json
[
	{
		"results": 8
	}
]
```

{% endtab %}

{% tab title="Python" %}

```python
if len(items[0].json) == 0:
	return [
		{
			"json": {
				"results": 0,
			}
		}
	]
else:
	return [
		{
			"json": {
				"results": len(items),
			}
		}
	]
```

The output will be similar to the following.

```json
[
	{
		"results": 8
	}
]
```

{% endtab %}
{% endtabs %}
