> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference/customdata.md).

# Customdata

## `$execution.customData`.**`get()`** <a href="#dollarexecutioncustomdataget" id="dollarexecutioncustomdataget"></a>

**Description:** Returns the custom execution data stored under the given key. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)

**Syntax:** `$execution.customData`.get(key)

**Returns:** String

**Source:** Custom n8n functionality

**Parameters:**

* `key` (String) - The key (identifier) under which the data is stored

**Examples:**

```javascript
// Get the user's email (which was previously stored)
$execution.customData.get("user_email") //=> "me@example.com"
```

## `$execution.customData`.**`getAll()`** <a href="#dollarexecutioncustomdatagetall" id="dollarexecutioncustomdatagetall"></a>

**Description:** Returns all the key-value pairs of custom data that have been set in the current execution. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)

**Syntax:** `$execution.customData`.getAll()

**Returns:** Object

**Source:** Custom n8n functionality

**Examples:**

```javascript
$execution.customData.getAll() //=> {"user_email": "me@example.com", "id": 1234}
```

## `$execution.customData`.**`set()`** <a href="#dollarexecutioncustomdataset" id="dollarexecutioncustomdataset"></a>

**Description:** Stores custom execution data under the key specified. Use this to easily filter executions by this data. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)

**Syntax:** `$execution.customData`.set(key, value)

**Source:** Custom n8n functionality

**Parameters:**

* `key` (String) - The key (identifier) under which the data is stored
* `value` (String) - The data to store

**Examples:**

```javascript
// Store the user's email, to easily retrieve all execs related to that user later
$execution.customData.set("user_email", "me@example.com")
```

## `$execution.customData`.**`setAll()`** <a href="#dollarexecutioncustomdatasetall" id="dollarexecutioncustomdatasetall"></a>

**Description:** Sets multiple key-value pairs of custom data for the execution. Use this to easily filter executions by this data. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)

**Syntax:** `$execution.customData`.setAll(obj)

**Source:** Custom n8n functionality

**Parameters:**

* `obj` (Object) - A JavaScript object containing key-value pairs of the data to set

**Examples:**

```javascript
$execution.customData.setAll({"user_email": "me@example.com", "id": 1234})
```
