---
title: Basic commands
url: https://docs.apify.com/actors/development/programming-interface/basic-commands.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Programming interface](https://docs.apify.com/actors/development/programming-interface.md)
previous: [Programming interface](https://docs.apify.com/actors/development/programming-interface.md)
next: [Environment variables](https://docs.apify.com/actors/development/programming-interface/environment-variables.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Basic commands

***

This page covers essential commands for the Apify SDK in JavaScript & Python. These commands are designed to be used within a running Actor, either in a local environment or on the Apify platform.

## Initialize your Actor

Before using any Apify SDK methods, initialize your Actor. This step prepares the Actor to receive events from the Apify platform, sets up machine and storage configurations, and clears previous local storage states.

**JavaScript**

Use the `init()` method to initialize your Actor. Pair it with `exit()` to properly terminate the Actor. For more information on `exit()`, go to Exit Actor.


```js
import { Actor } from 'apify';



await Actor.init();

console.log('Actor starting...');

// ...

await Actor.exit();
```


Alternatively, use the `main()` function for environments that don't support top-level awaits. The `main()` function is syntax-sugar for `init()` and `exit()`. It will call `init()` before it executes its callback and `exit()` after the callback resolves.


```js
import { Actor } from 'apify';



Actor.main(async () => {

    console.log('Actor starting...');

    // ...

});
```


**Python**

In Python, use an asynchronous context manager with the `with` keyword. The `init()` method will be called before the code block is executed, and the `exit()` method will be called after the code block is finished.


```python
from apify import Actor



async def main():

    async with Actor:

        Actor.log.info('Actor starting...')

        # ...
```


## Get input

Access the Actor's input object, which is stored as a JSON file in the Actor's default key-value store. The input is an object with properties. If the Actor defines the input schema, the input object is guaranteed to conform to it.

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



const input = await Actor.getInput();

console.log(input);

// prints: {'option1': 'aaa', 'option2': 456}



await Actor.exit();
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        actor_input: dict = await Actor.get_input() or {}

        Actor.log.info(actor_input)

        # prints: {'option1': 'aaa', 'option2': 456}
```


Usually, the file is called `INPUT`, but the exact key is defined in the `ACTOR_INPUT_KEY` environment variable.

## Key-value store access

Use the [Key-value store](https://docs.apify.com/storage/key-value-store.md) to read and write arbitrary files

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



// Save object to store (stringified to JSON)

await Actor.setValue('my_state', { something: 123 });



// Save binary file to store with content type

await Actor.setValue('screenshot.png', buffer, { contentType: 'image/png' });



// Get a record from the store (automatically parsed from JSON)

const value = await Actor.getValue('my_state');



// Access another key-value store by its name

const store = await Actor.openKeyValueStore('screenshots-store');

await store.setValue('screenshot.png', buffer, { contentType: 'image/png' });



await Actor.exit();
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        # Save object to store (stringified to JSON)

        await Actor.set_value('my_state', {'something': 123})



        # Get a record from the store (automatically parsed from JSON)

        value = await Actor.get_value('my_state')



        # Log the obtained value

        Actor.log.info(f'value = {value}')

        # prints: value = {'something': 123}
```


## Push results to the dataset

Store larger results in a [Dataset](https://docs.apify.com/storage/dataset.md), an append-only object storage

Note that Datasets can optionally be equipped with the schema that ensures only certain kinds of objects are stored in them.

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



// Append result object to the default dataset associated with the run

await Actor.pushData({ someResult: 123 });



await Actor.exit();
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        # Append result object to the default dataset associated with the run

        await Actor.push_data({'some_result': 123})
```


## Exit Actor

When an Actor's main process terminates, the Actor run is considered finished. The process exit code determines Actor's final status:

* Exit code `0`: Status `SUCCEEDED`
* Exit code not equal to `0`: Status `FAILED`

By default, the platform sets a generic status message like *Actor exit with exit code 0*. However, you can provide more informative message using the SDK's exit methods.

### Basic exit

Use the `exit()` method to terminate the Actor with a custom status message:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();

// ...

// Actor will finish with 'SUCCEEDED' status

await Actor.exit('Succeeded, crawled 50 pages');
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        # Actor will finish with 'SUCCEEDED' status

        await Actor.exit(status_message='Succeeded, crawled 50 pages')

        # INFO  Exiting actor ({"exit_code": 0})

        # INFO  [Terminal status message]: Succeeded, crawled 50 pages
```


### Immediate exit

To exit immediately without calling exit handlers:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();

// ...

// Exit right away without calling `exit` handlers at all

await Actor.exit('Done right now', { timeoutSecs: 0 });
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        # Exit right away without calling `exit` handlers at all

        await Actor.exit(event_listeners_timeout_secs=0, status_message='Done right now')

        # INFO  Exiting actor ({"exit_code": 0})

        # INFO  [Terminal status message]: Done right now
```


### Failed exit

To indicate a failed run:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();

// ...

// Actor will finish with 'FAILED' status

await Actor.exit('Could not finish the crawl, try increasing memory', { exitCode: 1 });
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        # Actor will finish with 'FAILED' status

        await Actor.exit(status_message='Could not finish the crawl, try increasing memory', exit_code=1)

        # INFO  Exiting actor ({"exit_code": 1})

        # INFO  [Terminal status message]: Could not finish the crawl, try increasing memory
```


### Preferred exit methods

The SDK provides convenient methods for exiting Actors:

* Use `exit()` with custom messages to inform users about the Actor's achievements or issues.
* Use `fail()` as a shortcut for `exit()` when indicating an error. It defaults to an exit code of `1` and emits the `exit` event, allowing components to perform cleanup or state persistence.
* The `exit()` method also emits the `exit` event, enabling cleanup or state persistence.

Example of a failed exit using a shorthand method:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();

// ...

// Or nicer way using this syntactic sugar:

await Actor.fail('Could not finish the crawl, try increasing memory');
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        # ... or nicer way using this syntactic sugar:

        await Actor.fail(status_message='Could not finish the crawl. Try increasing memory')

        # INFO  Exiting actor ({"exit_code": 1})

        # INFO  [Terminal status message]: Could not finish the crawl. Try increasing memory
```


### Exit event handlers (JavaScript only)

In JavaScript, you can register handlers for the `exit` event:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



// Register a handler to be called on exit.

// Note that the handler has `timeoutSecs` to finish its job.

Actor.on('exit', ({ statusMessage, exitCode, timeoutSecs }) => {

    // Perform cleanup...

});



await Actor.exit();
```


**Python**


```python
# 😔 Custom handlers are not supported in the Python SDK yet.
```
