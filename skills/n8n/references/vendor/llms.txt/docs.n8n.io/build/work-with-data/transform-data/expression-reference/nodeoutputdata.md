> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md).

# Nodeoutputdata

## `$()`.**`all()`** <a href="#dollarall" id="dollarall"></a>

**Description:** Returns an array of the node’s output items

**Syntax:** `$()`.all(branchIndex?, runIndex?)

**Returns:** Array

**Source:** Custom n8n functionality

**Parameters:**

* `branchIndex` (Number) - optional - The output branch of the node to use. Defaults to the first branch (index 0)
* `runIndex` (Number) - optional - The run of the node to use. Defaults to the first run (index 0)

## `$()`.**`first()`** <a href="#dollarfirst" id="dollarfirst"></a>

**Description:** Returns the first item output by the node

**Syntax:** `$()`.first(branchIndex?, runIndex?)

**Returns:** Item

**Source:** Custom n8n functionality

**Parameters:**

* `branchIndex` (Number) - optional - The output branch of the node to use. Defaults to the first branch (index 0)
* `runIndex` (Number) - optional - The run of the node to use. Defaults to the first run (index 0)

## `$()`.**`isExecuted`** <a href="#dollarisexecuted" id="dollarisexecuted"></a>

**Description:** Is `true` if the node has executed, `false` otherwise

**Syntax:** `$()`.`$()`.**`isExecuted`**

**Returns:** Boolean

**Source:** Custom n8n functionality

## `$()`.**`item`** <a href="#dollaritem" id="dollaritem"></a>

**Description:** Returns the matching item, i.e. the one used to produce the current item in the current node. [More info](/build/work-with-data/reference-data/link-data-items.md)

**Syntax:** `$()`.`$()`.**`item`**

**Returns:** Item

**Source:** Custom n8n functionality

## `$()`.**`itemMatching()`** <a href="#dollaritemmatching" id="dollaritemmatching"></a>

**Description:** Returns the matching item, i.e. the one used to produce the item in the current node at the specified index. [More info](/build/work-with-data/reference-data/link-data-items.md)

**Syntax:** `$()`.itemMatching(currentItemIndex?)

**Returns:** Item

**Source:** Custom n8n functionality

**Parameters:**

* `currentItemIndex` (Number) - The index of the item in the current node to be matched with.

## `$()`.**`last()`** <a href="#dollarlast" id="dollarlast"></a>

**Description:** Returns the last item output by the node

**Syntax:** `$()`.last(branchIndex?, runIndex?)

**Returns:** Item

**Source:** Custom n8n functionality

**Parameters:**

* `branchIndex` (Number) - optional - The output branch of the node to use. Defaults to the first branch (index 0)
* `runIndex` (Number) - optional - The run of the node to use. Defaults to the first run (index 0)

## `$()`.**`params`** <a href="#dollarparams" id="dollarparams"></a>

**Description:** The configuration settings of the given node. These are the parameters you fill out within the node’s UI (e.g. its operation).

**Syntax:** `$()`.`$()`.**`params`**

**Returns:** NodeParams

**Source:** Custom n8n functionality
