> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference/number.md).

# Number

## *`Number`*.**`abs()`** <a href="#numberabs" id="numberabs"></a>

**Description:** Returns the number’s absolute value, i.e. removes any minus sign

**Syntax:** *`Number`*.abs()

**Returns:** Number

**Source:** Custom n8n functionality

**Examples:**

```javascript
// x = -1.7
x.abs() //=> 1.7
```

## *`Number`*.**`ceil()`** <a href="#numberceil" id="numberceil"></a>

**Description:** Rounds the number up to the next whole number

**Syntax:** *`Number`*.ceil()

**Returns:** Number

**Source:** Custom n8n functionality

**Examples:**

```javascript
// x = 1.234
x.ceil() //=> 2
```

## *`Number`*.**`floor()`** <a href="#numberfloor" id="numberfloor"></a>

**Description:** Rounds the number down to the nearest whole number

**Syntax:** *`Number`*.floor()

**Returns:** Number

**Source:** Custom n8n functionality

**Examples:**

```javascript
// x = 1.234
x.floor() //=> 1
```

## *`Number`*.**`format()`** <a href="#numberformat" id="numberformat"></a>

**Description:** Returns a formatted string representing the number. Useful for formatting for a specific language or currency. The same as \<a href=”[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\\_Objects/Intl/NumberFormat/NumberFormat”>\`Intl.NumberFormat()\`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\_Objects/Intl/NumberFormat/NumberFormat”>`Intl.NumberFormat\(\)`).

**Syntax:** *`Number`*.format(locale?, options?)

**Returns:** String

**Source:** Custom n8n functionality

**Parameters:**

* `locale` (String) - optional - A \<a href=”[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\\_Objects/Intl#locales\\\_argument”>locale](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\_Objects/Intl#locales\\_argument”>locale) tag for formatting the number, e.g. `fr-FR`, `en-GB`, `pr-BR`
* `options` (Object) - optional - Configuration options for number formatting. [More info](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat)

**Examples:**

```javascript
// number = 123456.789;
number.format('de-DE') //=> 123.456,789
```

```javascript
// number = 123456.789;
number.format('de-DE', {'style': 'currency', 'currency': 'EUR'}) //=> 123.456,79 €
```

## *`Number`*.**`isEmpty()`** <a href="#numberisempty" id="numberisempty"></a>

**Description:** Returns `false` for all numbers. Returns `true` for `null`.

**Syntax:** *`Number`*.isEmpty()

**Returns:** Boolean

**Source:** Custom n8n functionality

**Examples:**

```javascript
// num = 10
num.isEmpty() // => false
```

```javascript
// num = 0
num.isEmpty() // => false
```

```javascript
// num = null
num.isEmpty() // => true
```

## *`Number`*.**`isEven()`** <a href="#numberiseven" id="numberiseven"></a>

**Description:** Returns `true` if the number is even. Throws an error if the number isn’t a whole number.

**Syntax:** *`Number`*.isEven()

**Returns:** Boolean

**Source:** Custom n8n functionality

**Examples:**

```javascript
// number = 33
number.isEven() //=> false
```

## *`Number`*.**`isInteger()`** <a href="#numberisinteger" id="numberisinteger"></a>

**Description:** Returns `true` if the number is a whole number

**Syntax:** *`Number`*.isInteger()

**Returns:** Boolean

**Source:** Custom n8n functionality

**Examples:**

```javascript
// number = 4
number.isInteger() //=> true
```

```javascript
// number = 4.12
number.isInteger() //=> false
```

## *`Number`*.**`isOdd()`** <a href="#numberisodd" id="numberisodd"></a>

**Description:** Returns `true` if the number is odd. Throws an error if the number isn’t a whole number.

**Syntax:** *`Number`*.isOdd()

**Returns:** Boolean

**Source:** Custom n8n functionality

**Examples:**

```javascript
// number = 33
number.isOdd() //=> true
```

## *`Number`*.**`round()`** <a href="#numberround" id="numberround"></a>

**Description:** Returns the number rounded to the nearest whole number (or specified number of decimal places)

**Syntax:** *`Number`*.round(decimalPlaces?)

**Returns:** Number

**Source:** Custom n8n functionality

**Parameters:**

* `decimalPlaces` (Number) - optional - The number of decimal places to round to

**Examples:**

```javascript
// number = 1.256
number.round() //=> 1
```

```javascript
// number = 1.256
number.round(1) //=> 1.3
number.round(2) //=> 1.26
```

## *`Number`*.**`toBoolean()`** <a href="#numbertoboolean" id="numbertoboolean"></a>

**Description:** Converts the number to a boolean value. `0` becomes `false`; everything else becomes `true`.

**Syntax:** *`Number`*.toBoolean()

**Source:** Custom n8n functionality

**Examples:**

```javascript
// number = 12
number.toBoolean() //=> true
```

```javascript
// number = 0
number.toBoolean() //=> false
```

## *`Number`*.**`toDateTime()`** <a href="#numbertodatetime" id="numbertodatetime"></a>

**Description:** Converts a numerical timestamp into a DateTime. The format of the timestamp must be specified if it’s not in milliseconds. Uses the time zone in n8n (or in the workflow’s settings).

**Syntax:** *`Number`*.toDateTime(format?)

**Returns:** DateTime

**Source:** Custom n8n functionality

**Parameters:**

* `format` (String) - optional - The type of timestamp to convert. Options are `ms` (for Unix timestamp in milliseconds), `s` (for Unix timestamp in seconds) or `excel` (for days since 1900).

**Examples:**

```javascript
// ts = 1708695471
ts.toDateTime('s') //=> 2024-02-23T14:37:51+01:00
```

```javascript
// ts = 1708695471000
ts.toDateTime('ms') //=> 2024-02-23T14:37:51+01:00
```

```javascript
// ts = 45345
ts.toDateTime('excel') //=> 2024-02-23T01:00:00+01:00
```

## *`Number`*.**`toLocaleString()`** <a href="#numbertolocalestring" id="numbertolocalestring"></a>

**Description:** Returns a localised string representing the number, i.e. in the language and format corresponding to its locale. Defaults to the system's locale if none specified.

**Syntax:** *`Number`*.toLocaleString(locales?, options?)

**Returns:** String

**Source:** JavaScript function

**Parameters:**

* `locales` (String|Array) - optional - The locale to assign, e.g. ‘en-GB’ for British English or ‘pt-BR’ for Brazilian Portuguese. See \<a href=”[https://www.localeplanet.com/icu/”>full](https://www.localeplanet.com/icu/”>full) list (unofficial). Also accepts an array of locales. Defaults to the system locale if not specified.
* `options` (Object) - optional - An object with \<a href=”[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\\_Objects/Intl/NumberFormat/NumberFormat#parameters”>formatting](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\_Objects/Intl/NumberFormat/NumberFormat#parameters”>formatting) options

**Examples:**

```javascript
// num = 500000.125
num.toLocaleString() //=> '500,000.125' (if in US English locale)
```

```javascript
// num = 500000.125
num.toLocaleString('fr-FR') //=> '500 000,125'
```

```javascript
// num = 500000.125
num.toLocaleString('fr-FR', {style:'currency', currency:'EUR'}) //=> '500 000,13 €'
```

## *`Number`*.**`toString()`** <a href="#numbertostring" id="numbertostring"></a>

**Description:** Converts the number to a simple textual representation. For more formatting options, see `toLocaleString()`.

**Syntax:** *`Number`*.toString(radix?)

**Returns:** String

**Source:** JavaScript function

**Parameters:**

* `radix` (Number) - optional - The base to use. Must be an integer between 2 and 36. E.g. base `2` is binary and base `16` is hexadecimal.

**Examples:**

```javascript
// num = 500000.125
num.toString() //=> '500000.125'
```

```javascript
// num = 500000.125
num.toString(16) //=> '7a120.2'
```
