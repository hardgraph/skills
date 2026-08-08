> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/code-in-n8n.md).

# Code in n8n

n8n is a low-code tool. This means you can do a lot without code, then add code when needed.

## Code in your workflows <a href="#code-in-your-workflows" id="code-in-your-workflows"></a>

There are two places in your workflows where you can use code:

* **Expressions**

  Use expressions[^1] to transform [data](/build/work-with-data/overview.md) in your nodes. You can use JavaScript in expressions, as well as n8n's [Built-in methods and variables](/build/code-in-n8n/use-built-in-shortcuts.md).

  [→ Expressions](/build/work-with-data/expressions-versus-data-nodes.md)
* **Code node**

  Use the Code node to add JavaScript or Python to your workflow.

  [→ Code node](/build/code-in-n8n/using-the-code-node.md)

## Other technical resources <a href="#other-technical-resources" id="other-technical-resources"></a>

These are features that are relevant to technical users.

### Technical nodes <a href="#technical-nodes" id="technical-nodes"></a>

n8n provides core nodes, which simplify adding key functionality such as API requests, webhooks, scheduling, and file handling.

* **Write a backend**

  The [HTTP Request](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest.md), [Webhook](/integrations/builtin/core-nodes/n8n-nodes-base.webhook.md), and [Code](/build/code-in-n8n/using-the-code-node.md) nodes help you make API calls, respond to webhooks, and write any JavaScript in your workflow.

  Use this do things like [Create an API endpoint](https://n8n.io/workflows/1750-creating-an-api-endpoint/).

  [→ Core nodes](/integrations/builtin/core-nodes.md)
* **Represent complex logic**

  You can build complex flows, using nodes like [If](/integrations/builtin/core-nodes/n8n-nodes-base.if.md), [Switch](/integrations/builtin/core-nodes/n8n-nodes-base.switch.md), and [Merge](/integrations/builtin/core-nodes/n8n-nodes-base.merge.md) nodes.

  [→ Flow logic](/build/flow-logic.md)

### Other developer resources <a href="#other-developer-resources" id="other-developer-resources"></a>

* **The n8n API**

  n8n provides an API, where you can programmatically perform many of the same tasks as you can in the GUI. There's an [n8n API node](/integrations/builtin/core-nodes/n8n-nodes-base.n8n.md) to access the API in your workflows.

  [→ API](/connect/n8n-api.md)
* **Self-host**

  You can self-host n8n. This keeps your data on your own infrastructure.

  [→ Hosting](/deploy/host-n8n.md)
* **Build your own nodes**

  You can build custom nodes, install them on your n8n instance, and publish them to [npm](https://www.npmjs.com/).

  [→ Creating nodes](/connect/create-nodes/overview.md)

[^1]: In n8n, expressions allow you to populate node parameters dynamically by executing JavaScript code. Instead of providing a static value, you can use the n8n expression syntax to define the value using data from previous nodes, other workflows, or your n8n environment.
