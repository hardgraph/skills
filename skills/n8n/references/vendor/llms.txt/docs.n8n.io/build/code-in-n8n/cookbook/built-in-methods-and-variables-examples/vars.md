> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/code-in-n8n/cookbook/built-in-methods-and-variables-examples/vars.md).

# vars

{% hint style="info" %}
**Feature availability**

* Available on Self-hosted Enterprise and Pro and Enterprise Cloud plans.
* You need access to the n8n instance owner account to create variables.
  {% endhint %}

`vars` contains all [Variables](/build/code-in-n8n/define-custom-variables.md) for the active environment. It's read-only: you can access variables using `vars`, but must set them using the UI.

{% tabs %}
{% tab title="JavaScript" %}

```js
// Access a variable
$vars.<variable-name>
```

{% endtab %}

{% tab title="Python" %}

```python
# Access a variable
_vars.<variable-name>
```

{% endtab %}
{% endtabs %}

{% hint style="info" %}
**`vars` and `env`**

`vars` gives access to user-created variables. It's part of the [Environments](/administer/use-source-control-and-environments.md) feature. `env` gives access to the [configuration environment variables](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables.md) for your n8n instance.
{% endhint %}
