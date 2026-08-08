# Creating webhooks

Copy for LLM

[Webhooks](https://docs.apify.com/platform/integrations/webhooks) allow you to configure the Apify platform to perform an action when a certain event occurs. For example, you can use them to start another Actor when the current run finishes or fails.

## Creating an ad-hoc webhook dynamically[](#creating-an-ad-hoc-webhook-dynamically)

Besides creating webhooks manually in [Apify Console](https://docs.apify.com/platform/console), or through the Apify API, you can also create [ad-hoc webhooks](https://docs.apify.com/platform/integrations/webhooks/ad-hoc-webhooks) dynamically from the code of your Actor using the [`Actor.add_webhook`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#add_webhook) method:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFdlYmhvb2tcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIENyZWF0ZSBhIHdlYmhvb2sgdGhhdCB3aWxsIGJlIHRyaWdnZXJlZCB3aGVuIHRoZSBBY3RvciBydW4gZmFpbHMuXFxuICAgICAgICB3ZWJob29rID0gV2ViaG9vayhcXG4gICAgICAgICAgICBldmVudF90eXBlcz1bJ0FDVE9SLlJVTi5GQUlMRUQnXSxcXG4gICAgICAgICAgICByZXF1ZXN0X3VybD0naHR0cHM6Ly9leGFtcGxlLmNvbS9ydW4tZmFpbGVkJyxcXG4gICAgICAgIClcXG5cXG4gICAgICAgICMgQWRkIHRoZSB3ZWJob29rIHRvIHRoZSBBY3Rvci5cXG4gICAgICAgIGF3YWl0IEFjdG9yLmFkZF93ZWJob29rKHdlYmhvb2spXFxuXFxuICAgICAgICAjIFJhaXNlIGFuIGVycm9yIHRvIHNpbXVsYXRlIGEgZmFpbGVkIHJ1bi5cXG4gICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignSSBhbSBhbiBlcnJvciBhbmQgSSBrbm93IGl0IScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.9eaaxbIvUZHqsglxnm_A_gsA3KZm0bBVhm778Yi_2j4\&asrc=run_on_apify)

```
import asyncio



from apify import Actor, Webhook





async def main() -> None:

    async with Actor:

        # Create a webhook that will be triggered when the Actor run fails.

        webhook = Webhook(

            event_types=['ACTOR.RUN.FAILED'],

            request_url='https://example.com/run-failed',

        )



        # Add the webhook to the Actor.

        await Actor.add_webhook(webhook)



        # Raise an error to simulate a failed run.

        raise RuntimeError('I am an error and I know it!')





if __name__ == '__main__':

    asyncio.run(main())
```

Note that webhooks are only supported when running on the Apify platform. When running the Actor locally, the method will print a warning and have no effect.

## Preventing duplicate webhooks[](#preventing-duplicate-webhooks)

To ensure that duplicate ad-hoc webhooks won't get created in a case of Actor restart, you can use the `idempotency_key` parameter. The idempotency key must be unique across all the webhooks of a user so that only one webhook gets created for a given value. You can use, for example, the Actor run ID as the idempotency key:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFdlYmhvb2tcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIENyZWF0ZSBhIHdlYmhvb2sgd2l0aCBhbiBpZGVtcG90ZW5jeSBrZXkgdG8gcHJldmVudCBkdXBsaWNhdGVzIG9uIHJldHJpZXMuXFxuICAgICAgICB3ZWJob29rID0gV2ViaG9vayhcXG4gICAgICAgICAgICBldmVudF90eXBlcz1bJ0FDVE9SLlJVTi5GQUlMRUQnXSxcXG4gICAgICAgICAgICByZXF1ZXN0X3VybD0naHR0cHM6Ly9leGFtcGxlLmNvbS9ydW4tZmFpbGVkJyxcXG4gICAgICAgICAgICBpZGVtcG90ZW5jeV9rZXk9QWN0b3IuY29uZmlndXJhdGlvbi5hY3Rvcl9ydW5faWQsXFxuICAgICAgICApXFxuXFxuICAgICAgICAjIEFkZCB0aGUgd2ViaG9vayB0byB0aGUgQWN0b3IuXFxuICAgICAgICBhd2FpdCBBY3Rvci5hZGRfd2ViaG9vayh3ZWJob29rKVxcblxcbiAgICAgICAgIyBSYWlzZSBhbiBlcnJvciB0byBzaW11bGF0ZSBhIGZhaWxlZCBydW4uXFxuICAgICAgICByYWlzZSBSdW50aW1lRXJyb3IoJ0kgYW0gYW4gZXJyb3IgYW5kIEkga25vdyBpdCEnKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.Omr7uLYb6LdWBBclwGsR9mF836mLnNYkqWHBc2KUIIQ\&asrc=run_on_apify)

```
import asyncio



from apify import Actor, Webhook





async def main() -> None:

    async with Actor:

        # Create a webhook with an idempotency key to prevent duplicates on retries.

        webhook = Webhook(

            event_types=['ACTOR.RUN.FAILED'],

            request_url='https://example.com/run-failed',

            idempotency_key=Actor.configuration.actor_run_id,

        )



        # Add the webhook to the Actor.

        await Actor.add_webhook(webhook)



        # Raise an error to simulate a failed run.

        raise RuntimeError('I am an error and I know it!')





if __name__ == '__main__':

    asyncio.run(main())
```
