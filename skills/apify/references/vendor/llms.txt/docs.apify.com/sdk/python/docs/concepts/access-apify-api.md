# Accessing Apify API

Copy for LLM

The Apify SDK provides a built-in instance of the [Apify API Client](https://docs.apify.com/api/client/python) for accessing Apify platform features beyond what the SDK covers directly.

The SDK wraps the most common operations (opening storages, reading input, charging, and starting other Actors) in high-level methods. To do things that it doesn't expose directly, such as managing schedules, listing your Actors and runs, working with builds or webhook dispatches, or reading another run's dataset, you can reach the full [Apify API](https://docs.apify.com/api/v2) through the client. It provides a dedicated sub-client for each API resource (for example `dataset`, `run`, or `schedule`), and each operation returns plain Python data structures.

## Actor client[](#actor-client)

To access the provided instance of [`ApifyClientAsync`](https://docs.apify.com/api/client/python/reference/class/ApifyClientAsync), you can use the [`Actor.apify_client`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#apify_client) property. This instance is already authenticated with the Actor's API token and points at the same API the Actor runs against.

For example, to get the details of your user, you can use this snippet:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIENyZWF0ZSBhIG5ldyB1c2VyIGNsaWVudC5cXG4gICAgICAgIHVzZXJfY2xpZW50ID0gQWN0b3IuYXBpZnlfY2xpZW50LnVzZXIoJ21lJylcXG5cXG4gICAgICAgICMgR2V0IGluZm9ybWF0aW9uIGFib3V0IHRoZSBjdXJyZW50IHVzZXIuXFxuICAgICAgICBtZSA9IGF3YWl0IHVzZXJfY2xpZW50LmdldCgpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1VzZXI6IHttZX0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.WmlLJkd9Tk8mAypqYz0clI_cBhejAvT52vw3toIUWqw\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Create a new user client.

        user_client = Actor.apify_client.user('me')



        # Get information about the current user.

        me = await user_client.get()

        Actor.log.info(f'User: {me}')





if __name__ == '__main__':

    asyncio.run(main())
```

You select a resource with the matching method (for example, `Actor.apify_client.dataset('<dataset_id>')` for a dataset or `Actor.apify_client.run('<run_id>')` for an Actor run), and then call the operation you need on the returned sub-client.

## Actor new client[](#actor-new-client)

To create a completely new instance of the client (for example, to use a different user's token or change the client configuration), use the [`Actor.new_client`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#new_client) method. It accepts an optional `token` and `api_url`, along with retry and timeout settings (`max_retries`, `min_delay_between_retries`, `timeout`), and returns a fresh `ApifyClientAsync`:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5UT0tFTiA9ICdBTk9USEVSX1VTRVJTX1RPS0VOJ1xcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgYXN5bmMgd2l0aCBBY3RvcjpcXG4gICAgICAgICMgQ3JlYXRlIGEgbmV3IHVzZXIgY2xpZW50IHdpdGggYSBjdXN0b20gdG9rZW4uXFxuICAgICAgICBhcGlmeV9jbGllbnQgPSBBY3Rvci5uZXdfY2xpZW50KHRva2VuPVRPS0VOLCBtYXhfcmV0cmllcz0yKVxcbiAgICAgICAgdXNlcl9jbGllbnQgPSBhcGlmeV9jbGllbnQudXNlcignbWUnKVxcblxcbiAgICAgICAgIyBHZXQgaW5mb3JtYXRpb24gYWJvdXQgdGhlIGFub3RoZXIgdXNlci5cXG4gICAgICAgIHRoZW0gPSBhd2FpdCB1c2VyX2NsaWVudC5nZXQoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidBbm90aGVyIHVzZXI6IHt0aGVtfScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.C875ON4a9WzTQz9QDEO3yS1N9RRCqxaRUIPmNGbsxIw\&asrc=run_on_apify)

```
import asyncio



from apify import Actor



TOKEN = 'ANOTHER_USERS_TOKEN'





async def main() -> None:

    async with Actor:

        # Create a new user client with a custom token.

        apify_client = Actor.new_client(token=TOKEN, max_retries=2)

        user_client = apify_client.user('me')



        # Get information about the another user.

        them = await user_client.get()

        Actor.log.info(f'Another user: {them}')





if __name__ == '__main__':

    asyncio.run(main())
```

This is useful when the Actor's own token is not the right one, for instance when acting on behalf of another user, or when you want different retry and timeout behavior than the shared `Actor.apify_client` instance.
