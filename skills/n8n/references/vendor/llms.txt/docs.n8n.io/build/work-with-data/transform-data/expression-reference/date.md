> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference/date.md).

# Date

## *`Date`*.**`toDateTime()`** <a href="#datetodatetime" id="datetodatetime"></a>

**Description:** Converts a JavaScript Date to a Luxon DateTime. The DateTime contains the same information, but is easier to manipulate.

**Syntax:** *`Date`*.toDateTime()

**Returns:** DateTime

**Source:** Custom n8n functionality

**Examples:**

```javascript
// date = new Date("2024-03-30T18:49")
date.toDateTime().plus(5, 'days') //=> 2024-04-04T18:49
```
