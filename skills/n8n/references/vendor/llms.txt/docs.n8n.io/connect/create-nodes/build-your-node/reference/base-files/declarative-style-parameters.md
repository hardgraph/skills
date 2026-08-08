> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/connect/create-nodes/build-your-node/reference/base-files/declarative-style-parameters.md).

# Declarative-style parameters

These are the parameters available for [node base file](/connect/create-nodes/build-your-node/reference/base-files.md) of declarative-style nodes.

This document gives short code snippets to help understand the code structure and concepts. For a full walk-through of building a node, including real-world code examples, refer to [Build a declarative-style node](/connect/create-nodes/build-your-node/tutorial-build-a-declarative-style-node.md).

Refer to [Standard parameters](/connect/create-nodes/build-your-node/reference/base-files/standard-parameters.md) for parameters available to all nodes.

## `methods` and `loadOptions` <a href="#methods-and-loadoptions" id="methods-and-loadoptions"></a>

*Object* | *Optional*

`methods` contains the `loadOptions` object. You can use `loadOptions` to query the service to get user-specific settings, then return them and render them in the GUI so the user can include them in subsequent queries. The object must include routing information for how to query the service, and output settings that define how to handle the returned options. For example:

```js
methods : {
	loadOptions: {
		routing: {
			request: {
				url: '/webhook/example-option-parameters',
				method: 'GET',
			},
			output: {
				postReceive: [
					{
						// When the returned data is nested under another property
						// Specify that property key
						type: 'rootProperty',
						properties: {
							property: 'responseData',
						},
					},
					{
						type: 'setKeyValue',
						properties: {
							name: '={{$responseItem.key}} ({{$responseItem.value}})',
							value: '={{$responseItem.value}}',
						},
					},
					{
						// If incoming data is an array of objects, sort alphabetically by key
						type: 'sort',
						properties: {
							key: 'name',
						},
					},
				],
			},
		},
	}
},
```

## `routing` <a href="#routing" id="routing"></a>

*Object* | *Required*

`routing` is an object used within an `options` array in operations and input field objects. It contains the details of an API call.

The code example below comes from the [Declarative-style tutorial](/connect/create-nodes/build-your-node/tutorial-build-a-declarative-style-node.md). It sets up an integration with a NASA API. It shows how to use `requestDefaults` to set up the basic API call details, and `routing` to add information for each operation.

```js
description: INodeTypeDescription = {
  // Other node info here
  requestDefaults: {
			baseURL: 'https://api.nasa.gov',
			url: '',
			headers: {
				Accept: 'application/json',
				'Content-Type': 'application/json',
			},
		},
    properties: [
      // Resources here
      {
        displayName: 'Operation'
        // Other operation details
        options: [
          {
            name: 'Get'
            value: 'get',
            description: '',
            routing: {
              request: {
                method: 'GET',
                url: '/planetary/apod'
              }
            }
          }
        ]
      }
    ]
}
```

## `version` <a href="#version" id="version"></a>

*Number* or *Array* | *Optional*

If you have one version of your node, this can be a number. If you want to support more than one version, turn this into an array, containing numbers for each node version.

n8n supports two methods of node versioning, but declarative-style nodes must use the light versioning approach. Refer to [Node versioning](/connect/create-nodes/build-your-node/reference/versioning.md) for more information.

## `features` <a href="#features" id="features"></a>

*Object* | *Optional*

Define named feature flags evaluated against the node version. Use features to control parameter visibility with `@feature` in `displayOptions`.

Refer to [Feature-based versioning](/connect/create-nodes/build-your-node/reference/versioning.md#feature-based-versioning) for more information.
