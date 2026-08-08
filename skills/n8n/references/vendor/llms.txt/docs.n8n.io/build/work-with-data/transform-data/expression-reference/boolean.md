> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference/boolean.md).

# Boolean

## *`Boolean`*.**`isEmpty()`** <a href="#booleanisempty" id="booleanisempty"></a>

**Description:** Returns `false` for all booleans. Returns `true` for `null`.

**Syntax:** *`Boolean`*.isEmpty()

**Returns:** Boolean

**Source:** Custom n8n functionality

**Examples:**

```javascript
// bool = true
bool.isEmpty() // => false
```

```javascript
// bool = false
bool.isEmpty() // => false
```

```javascript
// bool = null
bool.isEmpty() // => true
```

## *`Boolean`*.**`toNumber()`** <a href="#booleantonumber" id="booleantonumber"></a>

**Description:** Converts `true` to 1 and `false` to 0

**Syntax:** *`Boolean`*.toNumber()

**Returns:** Number

**Source:** Custom n8n functionality

**Examples:**

```javascript
true.toNumber() //=> 1
```

```javascript
false.toNumber() //=> 0
```

## *`Boolean`*.**`toString()`** <a href="#booleantostring" id="booleantostring"></a>

**Description:** Converts `true` to the string ‘true’ and `false` to the string ‘false’

**Syntax:** *`Boolean`*.toString()

**Returns:** String

**Source:** JavaScript function

**Examples:**

```javascript
// bool = true
bool.toString() //=> 'true'
```

```javascript
// bool = false
bool.toString() //=> 'false'
```
