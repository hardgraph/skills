---
title: Metamorph
url: https://docs.apify.com/actors/development/programming-interface/metamorph.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Programming interface](https://docs.apify.com/actors/development/programming-interface.md)
previous: [Container web server](https://docs.apify.com/actors/development/programming-interface/container-web-server.md)
next: [Standby mode](https://docs.apify.com/actors/development/programming-interface/standby.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Metamorph

***

## Transform Actor runs

Metamorph is a powerful operation that transforms an Actor run into the run of another Actor with a new input. This feature enables you to leverage existing Actors and create more efficient workflows.

## Understand metamorph

The metamorph process involves several key steps. It stops the current Actor's Docker container, then starts a new container using a different Docker image. During this transition, all default storages are preserved. The new input is stored under the `INPUT-METAMORPH-1` key in the default key-value store, ensuring seamless data transfer between Actor runs.

## Benefits of metamorph

Metamorph offers several benefits for developers:

* Seamless transition between Actors without starting a new run
* Building new Actors on top of existing ones
* Providing users with an improved input structure and interface
* Maintaining transparency for end-users

These benefits make metamorph a valuable tool for creating complex, efficient workflows.

## Implementation guidelines

To make your Actor compatible with metamorph, use `Actor.getInput()` instead of `Actor.getValue('INPUT')`. This method fetches the input using the correct key (*INPUT-METAMORPH-1*) for metamorphed runs, ensuring proper data retrieval in transformed Actor runs.

Runtime limits

There's a limit to how many times you can metamorph a single run. Refer to the [Actor runtime limits](https://docs.apify.com/account/limits.md#actor-limits) for more details.

Actor permissions

Actors running under limited permissions can only metamorph into Actors that also run with limited permissions. See [how Actor permissions work](https://docs.apify.com/actors/running/permissions.md#how-actor-permissions-work) for details.

## Example

Let's walk through an example of using metamorph to create a hotel review scraper:

1. Create an Actor that accepts a hotel URL as input.

2. Use the [apify/web-scraper](https://apify.com/apify/web-scraper) Actor to scrape reviews.

3. Use the metamorph operation to transform into a run of apify/web-scraper.

**JavaScript**

Here's the JavaScript code to achieve this:


```js
import { Actor } from 'apify';



await Actor.init();



// Get input of your Actor.

const { hotelUrl } = await Actor.getInput();



// Create input for apify/web-scraper

const newInput = {

    startUrls: [{ url: hotelUrl }],

    pageFunction: () => {

        // Here you pass the page function that

        // scrapes all the reviews ...

    },

    // ... and here would be all the additional

    // input parameters.

};



// Transform the Actor run to apify/web-scraper

// with the new input.

await Actor.metamorph('apify/web-scraper', newInput);



// The line here will never be reached, because the

// Actor run will be interrupted.

await Actor.exit();
```


**Python**

Here's the Python code to achieve this:


```python
from apify import Actor



async def main():

    async with Actor:

        # Get input of your Actor

        actor_input = await Actor.get_input() or {}



        # Create input for apify/web-scraper

        new_input = {

            'startUrls': [{'url': actor_input['url']}],

            'pageFunction': """

                # Here you pass the page function that

                # scrapes all the reviews ...

            """,

            # ... and here would be all the additional input parameters

        }



        # Transform the Actor run to apify/web-scraper with the new input

        await Actor.metamorph('apify/web-scraper', new_input)



        # The line here will never be reached, because the Actor run will be interrupted

        Actor.log.info('This should not be printed')
```


By following these steps, you can create a powerful hotel review scraper that leverages the capabilities of existing Actors through the metamorph operation.
