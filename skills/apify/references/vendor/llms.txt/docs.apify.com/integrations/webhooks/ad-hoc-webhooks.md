---
title: Ad-hoc webhooks
url: https://docs.apify.com/integrations/webhooks/ad-hoc-webhooks.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Programming](https://docs.apify.com/integrations/programming.md)
  - [Webhook integration](https://docs.apify.com/integrations/webhooks.md)
previous: [Webhook actions](https://docs.apify.com/integrations/webhooks/actions.md)
next: [Events types for webhooks](https://docs.apify.com/integrations/webhooks/events.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Ad-hoc webhooks

***

An ad-hoc webhook is a single-use webhook created for a specific Actor run when starting the run using the [Apify API](https://docs.apify.com/api/v2.md). The webhook triggers once when the run transitions to the specified state. Define ad-hoc webhooks using the `webhooks` URL parameter added to the API endpoint that starts an Actor or Actor task:


```text
https://api.apify.com/v2/actors/[ACTOR_ID]/runs?token=[YOUR_API_TOKEN]&webhooks=[AD_HOC_WEBHOOKS]
```


replace `AD_HOC_WEBHOOKS` with a base64 encoded stringified JSON array of webhook definitions:


```js
[

    {

        eventTypes: ['ACTOR.RUN.FAILED'],

        requestUrl: 'https://example.com/run-failed',

    },

    {

        eventTypes: ['ACTOR.RUN.SUCCEEDED'],

        requestUrl: 'https://example.com/run-succeeded',

        payloadTemplate: '{"hello": "world", "resource":{{resource}}}',

    },

];
```


## Create an ad-hoc webhook dynamically

You can also create a webhook dynamically from your Actor's code using the Actor's add webhook method:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();

// ...

await Actor.addWebhook({

    eventTypes: ['ACTOR.RUN.FAILED'],

    requestUrl: 'https://example.com/run-failed',

});

// ...

await Actor.exit();
```


**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        await Actor.add_webhook(

            event_types=['ACTOR.RUN.FAILED'],

            request_url='https://example.com/run-failed',

        )

        # ...
```


For more information, check out the [JavaScript SDK documentation](https://docs.apify.com/sdk/js/reference/class/Actor#addWebhook) or the [Python SDK documentation](https://docs.apify.com/sdk/python/reference/class/Actor#add_webhook).

To prevent duplicate ad-hoc webhooks in case of Actor restart, use the idempotency key parameter. The idempotency key must be unique across all user webhooks to ensure only one webhook is created for a given value. For example, use the Actor run ID as an idempotency key:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();

// ...

await Actor.addWebhook({

    eventTypes: ['ACTOR.RUN.FAILED'],

    requestUrl: 'https://example.com/run-failed',

    idempotencyKey: process.env.APIFY_ACTOR_RUN_ID,

});

// ...

await Actor.exit();
```


**Python**


```python
import os

from apify import Actor



async def main():

    async with Actor:

        await Actor.add_webhook(

            event_types=['ACTOR.RUN.FAILED'],

            request_url='https://example.com/run-failed',

            idempotency_key=os.environ['APIFY_ACTOR_RUN_ID'],

        )

        # ...
```
