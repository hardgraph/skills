---
title: Integrate Actors via API
url: https://docs.apify.com/integrations/actors/integrating-actors-via-api.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Integrations](https://docs.apify.com/integrations.md)
  - [Actor-to-Actor](https://docs.apify.com/integrations/actors.md)
previous: [Actor-to-Actor](https://docs.apify.com/integrations/actors.md)
next: [Develop integration-ready Actors](https://docs.apify.com/integrations/actors/integration-ready-actors.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Integrate Actors via API

***

You can integrate Actors via API using the [Create webhook](https://docs.apify.com/api/v2/webhooks-post.md) endpoint. It's the same as any other webhook, but to make sure you see it in Apify Console, you need to make sure of a few things.

* The `requestUrl` field needs to point to the **Run Actor** or **Run task** endpoints and needs to use their IDs as identifiers (i.e. not their technical names).
* The `payloadTemplate` field should be valid JSON - i.e. it should only use variables enclosed in strings. You will also need to make sure that it contains a `payload` field.
* The `shouldInterpolateStrings` field needs to be set to `true`, otherwise the variables won't work.
* Add `isApifyIntegration` field with the value `true`. This is a helper that turns on the Actor integration UI, if the above conditions are met.

Not meeting the conditions does not mean that the webhook won't work; it will just be displayed as a regular HTTP webhook in Apify Console.

The webhook should look something like this:


```json5
{

    "requestUrl": "https://api.apify.com/v2/actors/<integration-actor-id>/runs",

    "eventTypes": ["ACTOR.RUN.SUCCEEDED"],

    "condition": {

        "actorId": "<actor-id>",

    },

    "shouldInterpolateStrings": true,

    "isApifyIntegration": true,

    "payloadTemplate": "{\"field\":\"value\",\"payload\":{\"resource\":\"{{resource}}\"}}",

}
```


It's usually enough to just include the `resource` field in the payload template, but some Actors might also need other fields. Keep in mind that the `payloadTemplate` is a string, not an object.
