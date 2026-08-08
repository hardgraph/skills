> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/transform-data/expression-reference.md).

# Expression reference

These are some commonly used expressions. A more exhaustive list appears below.

| Category                       | Expression                        | Description                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------ | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Access current input item data | `$json`                           | JSON data of the current item                                                                                                                                                                                                                                                                                                                                                     |
|                                | `$json.fieldName`                 | Field of the current item                                                                                                                                                                                                                                                                                                                                                         |
|                                | `$binary`                         | Binary data of current item                                                                                                                                                                                                                                                                                                                                                       |
| Access previous node data      | `$("NodeName").first()`           | First item in a node                                                                                                                                                                                                                                                                                                                                                              |
|                                | `$("NodeName").item`              | Linked item of a node. See [Item linking](/build/work-with-data/reference-data/link-data-items.md) for more information.                                                                                                                                                                                                                                                          |
|                                | `$("NodeName").all()`             | All items of a node                                                                                                                                                                                                                                                                                                                                                               |
|                                | `$("NodeName").last()`            | Last item of a node                                                                                                                                                                                                                                                                                                                                                               |
| Date/Time                      | `$now`                            | Current date and time                                                                                                                                                                                                                                                                                                                                                             |
|                                | `$today`                          | Today's date                                                                                                                                                                                                                                                                                                                                                                      |
|                                | `$now.toFormat("yyyy-MM-dd")`     | Format current date as a string                                                                                                                                                                                                                                                                                                                                                   |
| Conditionals                   | `$if(condition, "true", "false")` | Helper function that returns a value when a condition is true or false                                                                                                                                                                                                                                                                                                            |
|                                | `condition ? true : false`        | Ternary operator: returns one value if a condition is true, another if false                                                                                                                                                                                                                                                                                                      |
|                                | `$ifEmpty(value, defaultValue)`   | Helper function takes two parameters and tests the first to check if it's empty, then returns either the parameter (if not empty) or the second parameter (if the first is empty). The first parameter is empty if it's `undefined`, `null`, an empty string `''`, an array where `value.length` returns `false` , or an object where `Object.keys(value).length` returns `false` |
| String Methods                 | `text.toUpperCase()`              | Convert to uppercase                                                                                                                                                                                                                                                                                                                                                              |
|                                | `text.toLowerCase()`              | Convert to lowercase                                                                                                                                                                                                                                                                                                                                                              |
|                                | `text.includes("foo")`            | Check if text contains search term                                                                                                                                                                                                                                                                                                                                                |
|                                | `text.extractEmail()`             | Extract email from text                                                                                                                                                                                                                                                                                                                                                           |
| Array Methods                  | `array.length`                    | Get array length                                                                                                                                                                                                                                                                                                                                                                  |
|                                | `array.join(", ")`                | Join array elements using a comma a separator                                                                                                                                                                                                                                                                                                                                     |
|                                | `array.filter(x => x <= 20)`      | Filter items of array based on the filtering condition                                                                                                                                                                                                                                                                                                                            |
|                                | `array.map(x => x.id)`            | Transform items of an array                                                                                                                                                                                                                                                                                                                                                       |

Browse the tables below to find methods by the data type on which they act. Click a method name to read detailed documentation for it.

## Array <a href="#array" id="array"></a>

* [*`Array`*.**`append(elem1, elem2?, ..., elemN?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayappend)

  Adds new elements to the end of the array. Similar to `push()`, but returns the modified array. Consider using spread syntax instead (see examples).
* [*`Array`*.**`average()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayaverage)

  Returns the average of the numbers in the array. Throws an error if there are any non-numbers.
* [*`Array`*.**`chunk(length)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraychunk)

  Splits the array into an array of sub-arrays, each with the given length
* [*`Array`*.**`compact()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraycompact)

  Removes any empty values from the array. `null`, `""` and `undefined` count as empty.
* [*`Array`*.**`concat(array2, array3?, ... arrayN?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayconcat)

  Joins one or more arrays onto the end of the base array
* [*`Array`*.**`difference(otherArray)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraydifference)

  Compares two arrays. Returns all elements in the base array that aren't present in `otherArray`.
* [*`Array`*.**`filter(function(element, index?, array?), thisValue?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayfilter)

  Returns an array with only the elements satisfying a condition. The condition is a function that returns `true` or `false`.
* [*`Array`*.**`find(function(element, index?, array?), thisValue?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayfind)

  Returns the first element from the array that satisfies the provided condition. The condition is a function that returns `true` or `false`. Returns `undefined` if no matches are found.

If you need all matching elements, use `filter()`.

* [*`Array`*.**`first()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayfirst)

  Returns the first element of the array
* [*`Array`*.**`includes(element, start?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayincludes)

  Returns `true` if the array contains the specified element
* [*`Array`*.**`indexOf(element, start?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayindexof)

  Returns the position of the first matching element in the array, or -1 if the element isn’t found. Positions start at 0.
* [*`Array`*.**`intersection(otherArray)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayintersection)

  Compares two arrays. Returns all elements in the base array that are also present in the other array.
* [*`Array`*.**`isEmpty()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayisempty)

  Returns `true` if the array has no elements or is `null`
* [*`Array`*.**`isNotEmpty()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayisnotempty)

  Returns `true` if the array has at least one element
* [*`Array`*.**`join(separator?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayjoin)

  Merges all elements of the array into a single string, with an optional separator between each element.

The opposite of `split()`.

* [*`Array`*.**`last()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraylast)

  Returns the last element of the array
* [*`Array`*.**`length`**](/build/work-with-data/transform-data/expression-reference/array.md#arraylength)

  The number of elements in the array
* [*`Array`*.**`map(function(element, index?, array?), thisValue?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraymap)

  Creates a new array by applying a function to each element of the original array
* [*`Array`*.**`max()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraymax)

  Returns the largest number in the array. Throws an error if there are any non-numbers.
* [*`Array`*.**`min()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraymin)

  Returns the smallest number in the array. Throws an error if there are any non-numbers.
* [*`Array`*.**`pluck(fieldName1?, fieldName2?, …)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraypluck)

  Returns an array containing the values of the given field(s) in each Object of the array. Ignores any array elements that aren’t Objects or don’t have a key matching the field name(s) provided.
* [*`Array`*.**`randomItem()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayrandomitem)

  Returns a randomly-chosen element from the array
* [*`Array`*.**`reduce(function(prevResult, currentElem, currentIndex?, array?), initResult)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayreduce)

  Reduces an array to a single value by applying a function to each element. The function combines the current element with the result of reducing the previous elements, producing a new result.
* [*`Array`*.**`removeDuplicates(keys?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayremoveduplicates)

  Removes any re-occurring elements from the array
* [*`Array`*.**`renameKeys(from, to)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayrenamekeys)

  Changes all matching keys (field names) of any Objects in the array. Rename more than one key by adding extra arguments, i.e. `from1, to1, from2, to2, ...`.
* [*`Array`*.**`reverse()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayreverse)

  Reverses the order of the elements in the array
* [*`Array`*.**`slice(start, end)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayslice)

  Returns a portion of the array, from the `start` index up to (but not including) the `end` index. Indexes start at 0.
* [*`Array`*.**`smartJoin(keyField, nameField)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraysmartjoin)

  Creates a single Object from an array of Objects. Each Object in the array provides one field for the returned Object. Each Object in the array must contain a field with the key name and a field with the value.
* [*`Array`*.**`sort(compareFunction(a, b)?)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraysort)

  Reorders the elements of the array. For sorting strings alphabetically, no parameter is required. For sorting numbers or Objects, see examples.
* [*`Array`*.**`sum()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraysum)

  Returns the total of all the numbers in the array. Throws an error if there are any non-numbers.
* [*`Array`*.**`toJsonString()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraytojsonstring)

  Converts the array to a JSON string. The same as JavaScript’s `JSON.stringify()`.
* [*`Array`*.**`toSpliced(start, deleteCount, elem1, ....., elemN)`**](/build/work-with-data/transform-data/expression-reference/array.md#arraytospliced)

  Adds and/or removes array elements at a given position.

See also `slice()` and `append()`.

* [*`Array`*.**`toString()`**](/build/work-with-data/transform-data/expression-reference/array.md#arraytostring)

  Converts the array to a string, with values separated by commas. To use a different separator, use `join()` instead.
* [*`Array`*.**`union(otherArray)`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayunion)

  Concatenates two arrays and then removes any duplicates
* [*`Array`*.**`unique()`**](/build/work-with-data/transform-data/expression-reference/array.md#arrayunique)

  Removes any duplicate elements from the array

## BinaryFile <a href="#binaryfile" id="binaryfile"></a>

* [`binaryFile`.**`directory`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfiledirectory)

  The path to the directory that the file is stored in. Useful for distinguishing between files with the same name in different directories. Not set if n8n is configured to store files in its database.
* [`binaryFile`.**`fileExtension`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfilefileextension)

  The suffix attached to the filename (e.g. `txt`)
* [`binaryFile`.**`fileName`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfilefilename)

  The name of the file, including extension
* [`binaryFile`.**`fileSize`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfilefilesize)

  A string representing the size of the file
* [`binaryFile`.**`fileType`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfilefiletype)

  A string representing the type of the file, e.g. `image`. Corresponds to the first part of the MIME type.
* [`binaryFile`.**`id`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfileid)

  The unique ID of the file. Used to identify the file when it is stored on disk or in a storage service such as S3.
* [`binaryFile`.**`mimeType`**](/build/work-with-data/transform-data/expression-reference/binaryfile.md#binaryfilemimetype)

  A string representing the format of the file’s contents, e.g. `image/jpeg`

## Boolean <a href="#boolean" id="boolean"></a>

* [*`Boolean`*.**`isEmpty()`**](/build/work-with-data/transform-data/expression-reference/boolean.md#booleanisempty)

  Returns `false` for all booleans. Returns `true` for `null`.
* [*`Boolean`*.**`toNumber()`**](/build/work-with-data/transform-data/expression-reference/boolean.md#booleantonumber)

  Converts `true` to 1 and `false` to 0
* [*`Boolean`*.**`toString()`**](/build/work-with-data/transform-data/expression-reference/boolean.md#booleantostring)

  Converts `true` to the string ‘true’ and `false` to the string ‘false’

## CustomData <a href="#customdata" id="customdata"></a>

* [`$execution.customData`.**`get(key)`**](/build/work-with-data/transform-data/expression-reference/customdata.md#executioncustomdataget)

  Returns the custom execution data stored under the given key. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)
* [`$execution.customData`.**`getAll()`**](/build/work-with-data/transform-data/expression-reference/customdata.md#executioncustomdatagetall)

  Returns all the key-value pairs of custom data that have been set in the current execution. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)
* [`$execution.customData`.**`set(key, value)`**](/build/work-with-data/transform-data/expression-reference/customdata.md#executioncustomdataset)

  Stores custom execution data under the key specified. Use this to easily filter executions by this data. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)
* [`$execution.customData`.**`setAll(obj)`**](/build/work-with-data/transform-data/expression-reference/customdata.md#executioncustomdatasetall)

  Sets multiple key-value pairs of custom data for the execution. Use this to easily filter executions by this data. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)

## Date <a href="#date" id="date"></a>

* [*`Date`*.**`toDateTime()`**](/build/work-with-data/transform-data/expression-reference/date.md#datetodatetime)

  Converts a JavaScript Date to a Luxon DateTime. The DateTime contains the same information, but is easier to manipulate.

## DateTime <a href="#datetime" id="datetime"></a>

* [*`DateTime`*.**`day`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeday)

  The day of the month (1-31)
* [*`DateTime`*.**`diffTo(otherDateTime, unit)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimediffto)

  Returns the difference between two DateTimes, in the given unit(s)
* [*`DateTime`*.**`diffToNow(unit)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimedifftonow)

  Returns the difference between the current moment and the DateTime, in the given unit(s). For a textual representation, use `toRelative()` instead.
* [*`DateTime`*.**`endOf(unit, opts)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeendof)

  Rounds the DateTime up to the end of one of its units, e.g. the end of the month
* [*`DateTime`*.**`equals(other)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeequals)

  Returns `true` if the two DateTimes represent exactly the same moment and are in the same time zone. For a less strict comparison, use `hasSame()`.
* [*`DateTime`*.**`extract(unit?)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeextract)

  Extracts a part of the date or time, e.g. the month, as a number. To extract textual names instead, see `format()`.
* [*`DateTime`*.**`format(fmt)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeformat)

  Converts the DateTime to a string, using the format specified. [Formatting guide](https://moment.github.io/luxon/#/formatting?id=table-of-tokens). For common formats, `toLocaleString()` may be easier.
* [*`DateTime`*.**`hasSame(otherDateTime, unit)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimehassame)

  Returns `true` if the two DateTimes are the same, down to the unit specified. Time zones are ignored (only local times are compared), so use `toUTC()` first if needed.
* [*`DateTime`*.**`hour`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimehour)

  The hour of the day (0-23)
* [*`DateTime`*.**`isBetween(date1, date2)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeisbetween)

  Returns `true` if the DateTime lies between the two moments specified
* [*`DateTime`*.**`isInDST`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeisindst)

  Whether the DateTime is in daylight saving time
* [*`DateTime`*.**`locale`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimelocale)

  The locale of a DateTime, such 'en-GB'. The locale is used when formatting the DateTime.
* [*`DateTime`*.**`millisecond`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimemillisecond)

  The millisecond of the second (0-999)
* [*`DateTime`*.**`minus(n, unit?)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeminus)

  Subtracts a given period of time from the DateTime
* [*`DateTime`*.**`minute`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeminute)

  The minute of the hour (0-59)
* [*`DateTime`*.**`month`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimemonth)

  The month (1-12)
* [*`DateTime`*.**`monthLong`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimemonthlong)

  The textual long month name, e.g. 'October'. Defaults to the system's locale if no locale has been specified.
* [*`DateTime`*.**`monthShort`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimemonthshort)

  The textual abbreviated month name, e.g. 'Oct'. Defaults to the system's locale if no locale has been specified.
* [*`DateTime`*.**`plus(n, unit?)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeplus)

  Adds a given period of time to the DateTime
* [*`DateTime`*.**`quarter`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimequarter)

  The quarter of the year (1-4)
* [*`DateTime`*.**`second`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimesecond)

  The second of the minute (0-59)
* [*`DateTime`*.**`set(values)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeset)

  Assigns new values to specified units of the DateTime. To round a DateTime, see also `startOf()` and `endOf()`.
* [*`DateTime`*.**`setLocale(locale)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimesetlocale)

  Sets the locale, which determines the language and formatting for the DateTime. Useful when generating a textual representation of the DateTime, e.g. with `format()` or `toLocaleString()`.
* [*`DateTime`*.**`setZone(zone, opts)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimesetzone)

  Converts the DateTime to the given time zone. The DateTime still represents the same moment unless specified in the options. See also `toLocal()` and `toUTC()`.
* [*`DateTime`*.**`startOf(unit, opts)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimestartof)

  Rounds the DateTime down to the beginning of one of its units, e.g. the start of the month
* [*`DateTime`*.**`toISO(opts)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetoiso)

  Returns an ISO 8601-compliant string representation of the DateTime
* [*`DateTime`*.**`toLocal()`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetolocal)

  Converts a DateTime to the workflow’s local time zone. The DateTime still represents the same moment unless specified in the parameters. The workflow’s time zone can be set in the workflow settings.
* [*`DateTime`*.**`toLocaleString(formatOpts)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetolocalestring)

  Returns a localised string representing the DateTime, i.e. in the language and format corresponding to its locale. Defaults to the system's locale if none specified.
* [*`DateTime`*.**`toMillis()`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetomillis)

  Returns a Unix timestamp in milliseconds (the number elapsed since 1st Jan 1970)
* [*`DateTime`*.**`toRelative(options)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetorelative)

  Returns a textual representation of the time relative to now, e.g. ‘in two days’. Rounds down by default.
* [*`DateTime`*.**`toSeconds()`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetoseconds)

  Returns a Unix timestamp in seconds (the number elapsed since 1st Jan 1970)
* [*`DateTime`*.**`toString()`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetostring)

  Returns a string representation of the DateTime. Similar to `toISO()`. For more formatting options, see `format()` or `toLocaleString()`.
* [*`DateTime`*.**`toUTC(offset, opts)`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimetoutc)

  Converts a DateTime to the UTC time zone. The DateTime still represents the same moment unless specified in the parameters. Use `setZone()` to convert to other zones.
* [*`DateTime`*.**`weekday`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeweekday)

  The day of the week. 1 is Monday and 7 is Sunday.
* [*`DateTime`*.**`weekdayLong`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeweekdaylong)

  The textual long weekday name, e.g. 'Wednesday'. Defaults to the system's locale if no locale has been specified.
* [*`DateTime`*.**`weekdayShort`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeweekdayshort)

  The textual abbreviated weekday name, e.g. 'Wed'. Defaults to the system's locale if no locale has been specified.
* [*`DateTime`*.**`weekNumber`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeweeknumber)

  The week number of the year (1-52ish)
* [*`DateTime`*.**`year`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimeyear)

  The year
* [*`DateTime`*.**`zone`**](/build/work-with-data/transform-data/expression-reference/datetime.md#datetimezone)

  The time zone associated with the DateTime

## ExecData <a href="#execdata" id="execdata"></a>

* [`$exec`.**`customData`**](/build/work-with-data/transform-data/expression-reference/execdata.md#execcustomdata)

  Set and get custom execution data (e.g. to filter executions by). You can also do this with the ‘Execution Data’ node. [More info](/build/understand-workflows/understand-executions/customize-executions-data.md)
* [`$exec`.**`id`**](/build/work-with-data/transform-data/expression-reference/execdata.md#execid)

  The ID of the current workflow execution
* [`$exec`.**`mode`**](/build/work-with-data/transform-data/expression-reference/execdata.md#execmode)

  Can be one of 3 values: either `test` (meaning the execution was triggered by clicking a button in n8n) or `production` (meaning the execution was triggered automatically). When running workflow tests, `evaluation` is used.
* [`$exec`.**`resumeFormUrl`**](/build/work-with-data/transform-data/expression-reference/execdata.md#execresumeformurl)

  The URL to access a form generated by the [’Wait’ node](/integrations/builtin/core-nodes/n8n-nodes-base.wait.md).
* [`$exec`.**`resumeUrl`**](/build/work-with-data/transform-data/expression-reference/execdata.md#execresumeurl)

  The webhook URL to call to resume a workflow waiting at a [’Wait’ node](/integrations/builtin/core-nodes/n8n-nodes-base.wait.md).

## HTTPResponse <a href="#httpresponse" id="httpresponse"></a>

* [`$response`.**`body`**](/build/work-with-data/transform-data/expression-reference/httpresponse.md#responsebody)

  The body of the response object from the last HTTP call. Only available in the ‘HTTP Request’ node
* [`$response`.**`headers`**](/build/work-with-data/transform-data/expression-reference/httpresponse.md#responseheaders)

  The headers returned by the last HTTP call. Only available in the ‘HTTP Request’ node.
* [`$response`.**`statusCode`**](/build/work-with-data/transform-data/expression-reference/httpresponse.md#responsestatuscode)

  The HTTP status code returned by the last HTTP call. Only available in the ‘HTTP Request’ node.
* [`$response`.**`statusMessage`**](/build/work-with-data/transform-data/expression-reference/httpresponse.md#responsestatusmessage)

  An optional message regarding the request status. Only available in the ‘HTTP Request’ node.

## Item <a href="#item" id="item"></a>

* [`$item`.**`binary`**](/build/work-with-data/transform-data/expression-reference/item.md#itembinary)

  Returns any binary data the item contains
* [`$item`.**`json`**](/build/work-with-data/transform-data/expression-reference/item.md#itemjson)

  Returns the JSON data the item contains. [More info](/build/work-with-data/understand-n8ns-data-structure.md)

## NodeInputData <a href="#nodeinputdata" id="nodeinputdata"></a>

* [`$input`.**`all(branchIndex?, runIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeinputdata.md#inputall)

  Returns an array of the current node’s input items
* [`$input`.**`first(branchIndex?, runIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeinputdata.md#inputfirst)

  Returns the current node’s first input item
* [`$input`.**`item`**](/build/work-with-data/transform-data/expression-reference/nodeinputdata.md#inputitem)

  Returns the input item currently being processed
* [`$input`.**`last(branchIndex?, runIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeinputdata.md#inputlast)

  Returns the current node’s last input item
* [`$input`.**`params`**](/build/work-with-data/transform-data/expression-reference/nodeinputdata.md#inputparams)

  The configuration settings of the current node. These are the parameters you fill out within the node when configuring it (e.g. its operation).

## NodeOutputData <a href="#nodeoutputdata" id="nodeoutputdata"></a>

* [`$()`.**`all(branchIndex?, runIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#all)

  Returns an array of the node’s output items
* [`$()`.**`first(branchIndex?, runIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#first)

  Returns the first item output by the node
* [`$()`.**`isExecuted`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#isexecuted)

  Is `true` if the node has executed, `false` otherwise
* [`$()`.**`item`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#item)

  Returns the matching item, i.e. the one used to produce the current item in the current node. [More info](/build/work-with-data/reference-data/link-data-items.md)
* [`$()`.**`itemMatching(currentItemIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#itemmatching)

  Returns the matching item, i.e. the one used to produce the item in the current node at the specified index. [More info](/build/work-with-data/reference-data/link-data-items.md)
* [`$()`.**`last(branchIndex?, runIndex?)`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#last)

  Returns the last item output by the node
* [`$()`.**`params`**](/build/work-with-data/transform-data/expression-reference/nodeoutputdata.md#params)

  The configuration settings of the given node. These are the parameters you fill out within the node’s UI (e.g. its operation).

## Number <a href="#number" id="number"></a>

* [*`Number`*.**`abs()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberabs)

  Returns the number’s absolute value, i.e. removes any minus sign
* [*`Number`*.**`ceil()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberceil)

  Rounds the number up to the next whole number
* [*`Number`*.**`floor()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberfloor)

  Rounds the number down to the nearest whole number
* [*`Number`*.**`format(locale?, options?)`**](/build/work-with-data/transform-data/expression-reference/number.md#numberformat)

  Returns a formatted string representing the number. Useful for formatting for a specific language or currency. The same as \<a href=”[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\\_Objects/Intl/NumberFormat/NumberFormat”>\`Intl.NumberFormat()\`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\\_Objects/Intl/NumberFormat/NumberFormat”>`Intl.NumberFormat\(\)`).
* [*`Number`*.**`isEmpty()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberisempty)

  Returns `false` for all numbers. Returns `true` for `null`.
* [*`Number`*.**`isEven()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberiseven)

  Returns `true` if the number is even. Throws an error if the number isn’t a whole number.
* [*`Number`*.**`isInteger()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberisinteger)

  Returns `true` if the number is a whole number
* [*`Number`*.**`isOdd()`**](/build/work-with-data/transform-data/expression-reference/number.md#numberisodd)

  Returns `true` if the number is odd. Throws an error if the number isn’t a whole number.
* [*`Number`*.**`round(decimalPlaces?)`**](/build/work-with-data/transform-data/expression-reference/number.md#numberround)

  Returns the number rounded to the nearest whole number (or specified number of decimal places)
* [*`Number`*.**`toBoolean()`**](/build/work-with-data/transform-data/expression-reference/number.md#numbertoboolean)

  Converts the number to a boolean value. `0` becomes `false`; everything else becomes `true`.
* [*`Number`*.**`toDateTime(format?)`**](/build/work-with-data/transform-data/expression-reference/number.md#numbertodatetime)

  Converts a numerical timestamp into a DateTime. The format of the timestamp must be specified if it’s not in milliseconds. Uses the time zone in n8n (or in the workflow’s settings).
* [*`Number`*.**`toLocaleString(locales?, options?)`**](/build/work-with-data/transform-data/expression-reference/number.md#numbertolocalestring)

  Returns a localised string representing the number, i.e. in the language and format corresponding to its locale. Defaults to the system's locale if none specified.
* [*`Number`*.**`toString(radix?)`**](/build/work-with-data/transform-data/expression-reference/number.md#numbertostring)

  Converts the number to a simple textual representation. For more formatting options, see `toLocaleString()`.

## Object <a href="#object" id="object"></a>

* [*`Object`*.**`compact()`**](/build/work-with-data/transform-data/expression-reference/object.md#objectcompact)

  Removes all fields that have empty values, i.e. are `null` or `""`
* [*`Object`*.**`hasField(name)`**](/build/work-with-data/transform-data/expression-reference/object.md#objecthasfield)

  Returns `true` if there is a field called `name`. Only checks top-level keys. Comparison is case-sensitive.
* [*`Object`*.**`isEmpty()`**](/build/work-with-data/transform-data/expression-reference/object.md#objectisempty)

  Returns `true` if the Object has no keys (fields) set or is `null`
* [*`Object`*.**`isNotEmpty()`**](/build/work-with-data/transform-data/expression-reference/object.md#objectisnotempty)

  Returns `true` if the Object has at least one key (field) set
* [*`Object`*.**`keepFieldsContaining(value)`**](/build/work-with-data/transform-data/expression-reference/object.md#objectkeepfieldscontaining)

  Removes any fields whose values don’t at least partly match the given `value`. Comparison is case-sensitive. Fields that aren’t strings will always be removed.
* [*`Object`*.**`keys()`**](/build/work-with-data/transform-data/expression-reference/object.md#objectkeys)

  Returns an array with all the field names (keys) the object contains. The same as JavaScript’s `Object.keys(obj)`.
* [*`Object`*.**`merge(otherObject)`**](/build/work-with-data/transform-data/expression-reference/object.md#objectmerge)

  Merges the two Objects into a single one. If a key (field name) exists in both Objects, the value from the first (base) Object is used.
* [*`Object`*.**`removeField(key)`**](/build/work-with-data/transform-data/expression-reference/object.md#objectremovefield)

  Removes a field from the Object. The same as JavaScript’s `delete`.
* [*`Object`*.**`removeFieldsContaining(value)`**](/build/work-with-data/transform-data/expression-reference/object.md#objectremovefieldscontaining)

  Removes keys (fields) whose values at least partly match the given `value`. Comparison is case-sensitive. Fields that aren’t strings are always kept.
* [*`Object`*.**`toJsonString()`**](/build/work-with-data/transform-data/expression-reference/object.md#objecttojsonstring)

  Converts the Object to a JSON string. Similar to JavaScript’s `JSON.stringify()`.
* [*`Object`*.**`urlEncode()`**](/build/work-with-data/transform-data/expression-reference/object.md#objecturlencode)

  Generates a URL parameter string from the Object’s keys and values. Only top-level keys are supported.
* [*`Object`*.**`values()`**](/build/work-with-data/transform-data/expression-reference/object.md#objectvalues)

  Returns an array with all the values of the fields the Object contains. The same as JavaScript’s `Object.values(obj)`.

## PrevNodeData <a href="#prevnodedata" id="prevnodedata"></a>

* [**`name`**](/build/work-with-data/transform-data/expression-reference/prevnodedata.md#name)

  The name of the node that the current input came from.

Always uses the current node’s first input connector if there is more than one (e.g. in the ‘Merge’ node).

* [**`outputIndex`**](/build/work-with-data/transform-data/expression-reference/prevnodedata.md#outputindex)

  The index of the output connector that the current input came from. Use this when the previous node had multiple outputs (such as an ‘If’ or ‘Switch’ node).

Always uses the current node’s first input connector if there is more than one (e.g. in the ‘Merge’ node).

* [**`runIndex`**](/build/work-with-data/transform-data/expression-reference/prevnodedata.md#runindex)

  The run of the previous node that generated the current input.

Always uses the current node’s first input connector if there is more than one (e.g. in the ‘Merge’ node).

## Root <a href="#root" id="root"></a>

* [**`$(nodeName)`**](/build/work-with-data/transform-data/expression-reference.md)

  Returns the data of the specified node
* [**`$binary`**](/build/work-with-data/transform-data/expression-reference/root.md#binary)

  Returns any binary input data to the current node, for the current item. Shorthand for `$input.item.binary`.
* [**`$execution`**](/build/work-with-data/transform-data/expression-reference/root.md#execution)

  Retrieve or set metadata for the current execution
* [**`$fromAI(key, description?, type?, defaultValue?)`**](/build/work-with-data/transform-data/expression-reference/root.md#fromai)

  Use when a large language model should provide the value of a node parameter. Consider providing a description for better results.
* [**`$if(condition, valueIfTrue, valueIfFalse)`**](/build/work-with-data/transform-data/expression-reference/root.md#if)

  Returns one of two values depending on the `condition`. Similar to the `?` operator in JavaScript.
* [**`$ifEmpty(value, valueIfEmpty)`**](/build/work-with-data/transform-data/expression-reference/root.md#ifempty)

  Returns the first parameter if it isn’t empty, otherwise returns the second parameter. The following count as empty: `””`, `[]`, `{}`, `null`, `undefined`
* [**`$input`**](/build/work-with-data/transform-data/expression-reference/root.md#input)

  The input data of the current node
* [**`$itemIndex`**](/build/work-with-data/transform-data/expression-reference/root.md#itemindex)

  The position of the item currently being processed in the list of input items
* [**`$jmespath(obj, expression)`**](/build/work-with-data/transform-data/expression-reference/root.md#jmespath)

  Extracts data from an object (or array of objects) using a \<a href=”/code/cookbook/jmespath/”>JMESPath expression. Useful for querying complex, nested objects. Returns `undefined` if the expression is invalid.
* [**`$json`**](/build/work-with-data/transform-data/expression-reference/root.md#json)

  Returns the JSON input data to the current node, for the current item. Shorthand for `$input.item.json`. [More info](/build/work-with-data/understand-n8ns-data-structure.md)
* [**`$max(num1, num2, …, numN)`**](/build/work-with-data/transform-data/expression-reference/root.md#max)

  Returns the highest of the given numbers
* [**`$min(num1, num2, …, numN)`**](/build/work-with-data/transform-data/expression-reference/root.md#min)

  Returns the lowest of the given numbers
* [**`$nodeVersion`**](/build/work-with-data/transform-data/expression-reference/root.md#nodeversion)

  The version of the current node (as displayed at the bottom of the nodes’s settings pane)
* [**`$now`**](/build/work-with-data/transform-data/expression-reference/root.md#now)

  A DateTime representing the current moment.

Uses the workflow’s time zone (which can be changed in the workflow settings).

* [**`$pageCount`**](/build/work-with-data/transform-data/expression-reference/root.md#pagecount)

  The number of results pages the node has fetched. Only available in the ‘HTTP Request’ node.
* [**`$parameter`**](/build/work-with-data/transform-data/expression-reference/root.md#parameter)

  The configuration settings of the current node. These are the parameters you fill out within the node’s UI (e.g. its operation).
* [**`$prevNode`**](/build/work-with-data/transform-data/expression-reference/root.md#prevnode)

  Information about the node that the current input came from.

When in a ‘Merge’ node, always uses the first input connector.

* [**`$request`**](/build/work-with-data/transform-data/expression-reference/root.md#request)

  The request object sent during the last run of the node. Only available in the ‘HTTP Request’ node.
* [**`$response`**](/build/work-with-data/transform-data/expression-reference/root.md#response)

  The response returned by the last HTTP call. Only available in the ‘HTTP Request’ node.
* [**`$runIndex`**](/build/work-with-data/transform-data/expression-reference/root.md#runindex)

  The index of the current run of the current node execution. Starts at 0.
* [**`$secrets`**](/build/work-with-data/transform-data/expression-reference/root.md#secrets)

  The secrets from an [external secrets vault](/deploy/host-n8n/configure-n8n/basic-configuration/use-environment-variables/external-secrets.md), if configured. Secret values are never displayed to the user. Only available in credential fields.
* [**`$today`**](/build/work-with-data/transform-data/expression-reference/root.md#today)

  A DateTime representing midnight at the start of the current day.

Uses the instance’s time zone (unless overridden in the workflow’s settings).

* [**`$vars`**](/build/work-with-data/transform-data/expression-reference/root.md#vars)

  The [variables](/build/code-in-n8n/define-custom-variables.md) available to the workflow
* [**`$workflow`**](/build/work-with-data/transform-data/expression-reference/root.md#workflow)

  Information about the current workflow

## String <a href="#string" id="string"></a>

* [*`String`*.**`base64Encode()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringbase64decode)

  Converts plain text to a base64-encoded string
* [*`String`*.**`base64Encode()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringbase64encode)

  Converts a base64-encoded string to plain text
* [*`String`*.**`concat(string1, string2?, ..., stringN?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringconcat)

  Joins one or more strings onto the end of the base string. Alternatively, use the `+` operator (see examples).
* [*`String`*.**`extractDomain()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringextractdomain)

  If the string is an email address or URL, returns its domain (or `undefined` if nothing found).

If the string also contains other content, try using `extractEmail()` or `extractUrl()` first.

* [*`String`*.**`extractEmail()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringextractemail)

  Extracts the first email found in the string. Returns `undefined` if none is found.
* [*`String`*.**`extractUrl()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringextracturl)

  Extracts the first URL found in the string. Returns `undefined` if none is found. Only recognizes full URLs, e.g. those starting with `http`.
* [*`String`*.**`extractUrlPath()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringextracturlpath)

  Returns the part of a URL after the domain, or `undefined` if no URL found.

If the string also contains other content, try using `extractUrl()` first.

* [*`String`*.**`hash(algo?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringhash)

  Returns the string hashed with the given algorithm. Defaults to md5 if not specified.
* [*`String`*.**`includes(searchString, start?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringincludes)

  Returns `true` if the string contains the `searchString`. Case-sensitive.
* [*`String`*.**`indexOf(searchString, start?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringindexof)

  Returns the index (position) of the first occurrence of `searchString` within the base string, or -1 if not found. Case-sensitive.
* [*`String`*.**`isDomain()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringisdomain)

  Returns `true` if the string is a domain
* [*`String`*.**`isEmail()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringisemail)

  Returns `true` if the string is an email
* [*`String`*.**`isEmpty()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringisempty)

  Returns `true` if the string has no characters or is `null`
* [*`String`*.**`isNotEmpty()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringisnotempty)

  Returns `true` if the string has at least one character
* [*`String`*.**`isNumeric()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringisnumeric)

  Returns `true` if the string represents a number
* [*`String`*.**`isUrl()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringisurl)

  Returns `true` if the string is a valid URL
* [*`String`*.**`length`**](/build/work-with-data/transform-data/expression-reference/string.md#stringlength)

  The number of characters in the string
* [*`String`*.**`match(regexp)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringmatch)

  Matches the string against a \<a href=”[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular\\\_expressions”>regular](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular\\_expressions”>regular) expression. Returns an array containing the first match, or all matches if the `g` flag is set in the regular expression. Returns `null` if no matches are found.

For checking whether text is present, consider `includes()` instead.

* [*`String`*.**`parseJson()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringparsejson)

  Returns the JavaScript Object or value represented by the string, or `undefined` if the string isn’t valid JSON. Single-quoted JSON is not supported.
* [*`String`*.**`quote(mark?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringquote)

  Wraps a string in quotation marks, and escapes any quotation marks already in the string. Useful when constructing JSON, SQL, etc.
* [*`String`*.**`removeMarkdown()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringremovemarkdown)

  Removes any Markdown formatting from the string. Also removes HTML tags.
* [*`String`*.**`removeTags()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringremovetags)

  Removes tags, such as HTML or XML, from the string
* [*`String`*.**`replace(pattern, replacement)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringreplace)

  Returns a string with the first occurrence of `pattern` replaced by `replacement`.

To replace all occurrences, use `replaceAll()` instead.

* [*`String`*.**`replaceAll(pattern, replacement)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringreplaceall)

  Returns a string with all occurrences of `pattern` replaced by `replacement`
* [*`String`*.**`replaceSpecialChars()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringreplacespecialchars)

  Replaces special characters in the string with the closest ASCII character
* [*`String`*.**`search(regexp)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringsearch)

  Returns the index (position) of the first occurrence of a pattern within the string, or -1 if not found. The pattern is specified using a \<a href=”[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular\\\_expressions”>regular](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular\\_expressions”>regular) expression. To use text instead, see `indexOf()`.
* [*`String`*.**`slice(start, end?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringslice)

  Extracts a fragment of the string at the given position. For more advanced extraction, see `match()`.
* [*`String`*.**`split(separator?, limit?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringsplit)

  Splits the string into an array of substrings. Each split is made at the `separator`, and the separator isn’t included in the output.

The opposite of using `join()` on an array.

* [*`String`*.**`startsWith(searchString, start?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringstartswith)

  Returns `true` if the string starts with `searchString`. Case-sensitive.
* [*`String`*.**`substring(start, end?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringsubstring)

  Extracts a fragment of the string at the given position. For more advanced extraction, see `match()`.
* [*`String`*.**`toBoolean()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtoboolean)

  Converts the string to a boolean value. `0`, `false` and `no` resolve to `false`, everything else to `true`. Case-insensitive.
* [*`String`*.**`toDateTime()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtodatetime)

  Converts the string to a DateTime. Useful for further transformation. Supported formats for the string are ISO 8601, HTTP, RFC2822, SQL and Unix timestamp in milliseconds.

To parse other formats, use \<a href=”[https://moment.github.io/luxon/api-docs/index.html#datetimefromformat”>](https://moment.github.io/luxon/api-docs/index.html#datetimefromformat”>) `DateTime.fromFormat()`.

* [*`String`*.**`toJsonString()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtojsonstring)

  Prepares the string to be inserted into a JSON object. Escapes any quotes and special characters (e.g. new lines), and wraps the string in quotes.

The same as JavaScript’s `JSON.stringify()`.

* [*`String`*.**`toLowerCase()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtolowercase)

  Converts all letters in the string to lower case
* [*`String`*.**`toNumber()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtonumber)

  Converts a string representing a number to a number. Throws an error if the string doesn’t start with a valid number.
* [*`String`*.**`toSentenceCase()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtosentencecase)

  Changes the capitalization of the string to sentence case. The first letter of each sentence is capitalized and all others are lowercased.
* [*`String`*.**`toSnakeCase()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtosnakecase)

  Changes the format of the string to snake case. Spaces and dashes are replaced by `_`, symbols are removed and all letters are lowercased.
* [*`String`*.**`toTitleCase()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtotitlecase)

  Changes the capitalization of the string to title case. The first letter of each word is capitalized and the others left unchanged. Short prepositions and conjunctions aren’t capitalized (e.g. ‘a’, ‘the’).
* [*`String`*.**`toUpperCase()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtouppercase)

  Converts all letters in the string to upper case (capitals)
* [*`String`*.**`trim()`**](/build/work-with-data/transform-data/expression-reference/string.md#stringtrim)

  Removes whitespace from both ends of the string. Whitespace includes new lines, tabs, spaces, etc.
* [*`String`*.**`urlDecode(allChars?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringurldecode)

  Decodes a URL-encoded string. Replaces any character codes in the form of `%XX` with their corresponding characters.
* [*`String`*.**`urlEncode(allChars?)`**](/build/work-with-data/transform-data/expression-reference/string.md#stringurlencode)

  Encodes the string so that it can be used in a URL. Spaces and special characters are replaced with codes of the form `%XX`.

## WorkflowData <a href="#workflowdata" id="workflowdata"></a>

* [`$workflow`.**`active`**](/build/work-with-data/transform-data/expression-reference/workflowdata.md#workflowactive)

  Whether the workflow is active
* [`$workflow`.**`id`**](/build/work-with-data/transform-data/expression-reference/workflowdata.md#workflowid)

  The workflow ID. Can also be found in the workflow’s URL.
* [`$workflow`.**`name`**](/build/work-with-data/transform-data/expression-reference/workflowdata.md#workflowname)

  The name of the workflow, as shown at the top of the editor
