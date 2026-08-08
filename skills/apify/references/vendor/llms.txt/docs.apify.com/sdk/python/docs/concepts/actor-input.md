# Actor input

Copy for LLM

The Actor gets its [input](https://docs.apify.com/platform/actors/running/input) from the input record in its default [key-value store](https://docs.apify.com/platform/storage/key-value-store).

To access it, instead of reading the record manually, you can use the [`Actor.get_input`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_input) convenience method. It gets the input record key from the Actor configuration, reads the record from the default key-value store, and decrypts any [secret input fields](https://docs.apify.com/platform/actors/development/secret-input).

For example, if an Actor received a JSON input with two fields, `{ "firstNumber": 1, "secondNumber": 2 }`, this is how you might process it:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBhY3Rvcl9pbnB1dCA9IGF3YWl0IEFjdG9yLmdldF9pbnB1dCgpIG9yIHt9XFxuICAgICAgICBmaXJzdF9udW1iZXIgPSBhY3Rvcl9pbnB1dC5nZXQoJ2ZpcnN0TnVtYmVyJywgMClcXG4gICAgICAgIHNlY29uZF9udW1iZXIgPSBhY3Rvcl9pbnB1dC5nZXQoJ3NlY29uZE51bWJlcicsIDApXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbygnU3VtOiAlcycsIGZpcnN0X251bWJlciArIHNlY29uZF9udW1iZXIpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.CF_KyX6EAMxLr_mlIpVnOD9pKv7wI53qxf1HWTha69g\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        actor_input = await Actor.get_input() or {}

        first_number = actor_input.get('firstNumber', 0)

        second_number = actor_input.get('secondNumber', 0)

        Actor.log.info('Sum: %s', first_number + second_number)





if __name__ == '__main__':

    asyncio.run(main())
```

## Validating input[](#validating-input)

Reading values straight out of the raw input dictionary works for simple cases, but it gives you no type guarantees, no constraint checks, and no clear error when the input is malformed. For anything beyond a couple of fields, validate the input with [Pydantic](https://docs.pydantic.dev/). Your code then works with a typed, guaranteed-valid object instead. For the recommended approach, see [Validate Actor input with Pydantic](https://docs.apify.com/sdk/python/sdk/python/docs/guides/input-validation.md).

## Loading URLs from Actor input[](#loading-urls-from-actor-input)

Actors commonly receive a list of URLs to process via their input. The [`ApifyRequestList`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ApifyRequestList.md) class (from `apify.request_loaders`) can parse the standard Apify input format for URL sources. It supports both direct URL objects (`{"url": "https://example.com"}`) and remote URL lists (`{"requestsFromUrl": "https://example.com/urls.txt"}`), where the remote file contains one URL per line.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5mcm9tIGFwaWZ5LnJlcXVlc3RfbG9hZGVycyBpbXBvcnQgQXBpZnlSZXF1ZXN0TGlzdFxcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgYXN5bmMgd2l0aCBBY3RvcjpcXG4gICAgICAgIGFjdG9yX2lucHV0ID0gYXdhaXQgQWN0b3IuZ2V0X2lucHV0KCkgb3Ige31cXG5cXG4gICAgICAgICMgVGhlIGlucHV0IG1heSBjb250YWluIGEgbGlzdCBvZiBVUkwgc291cmNlcyBpbiB0aGUgc3RhbmRhcmQgQXBpZnkgZm9ybWF0XFxuICAgICAgICByZXF1ZXN0X2xpc3Rfc291cmNlcyA9IGFjdG9yX2lucHV0LmdldCgncmVxdWVzdExpc3RTb3VyY2VzJywgW10pXFxuXFxuICAgICAgICAjIENyZWF0ZSBhIHJlcXVlc3QgbGlzdCBmcm9tIHRoZSBpbnB1dCBzb3VyY2VzLlxcbiAgICAgICAgIyBTdXBwb3J0cyBkaXJlY3QgVVJMcyBhbmQgcmVtb3RlIFVSTCBsaXN0cy5cXG4gICAgICAgIHJlcXVlc3RfbGlzdCA9IGF3YWl0IEFwaWZ5UmVxdWVzdExpc3Qub3BlbihcXG4gICAgICAgICAgICByZXF1ZXN0X2xpc3Rfc291cmNlc19pbnB1dD1yZXF1ZXN0X2xpc3Rfc291cmNlcyxcXG4gICAgICAgIClcXG5cXG4gICAgICAgIHRvdGFsID0gYXdhaXQgcmVxdWVzdF9saXN0LmdldF90b3RhbF9jb3VudCgpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0xvYWRlZCB7dG90YWx9IHJlcXVlc3RzIGZyb20gaW5wdXQnKVxcblxcbiAgICAgICAgIyBQcm9jZXNzIHJlcXVlc3RzIGZyb20gdGhlIGxpc3RcXG4gICAgICAgIHdoaWxlIHJlcXVlc3QgOj0gYXdhaXQgcmVxdWVzdF9saXN0LmZldGNoX25leHRfcmVxdWVzdCgpOlxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnUHJvY2Vzc2luZyB7cmVxdWVzdC51cmx9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.FB4Iil1Mno3Akr67GCOHo4SQPllagefTiQ_MJcK76xE\&asrc=run_on_apify)

```
import asyncio



from apify import Actor

from apify.request_loaders import ApifyRequestList





async def main() -> None:

    async with Actor:

        actor_input = await Actor.get_input() or {}



        # The input may contain a list of URL sources in the standard Apify format

        request_list_sources = actor_input.get('requestListSources', [])



        # Create a request list from the input sources.

        # Supports direct URLs and remote URL lists.

        request_list = await ApifyRequestList.open(

            request_list_sources_input=request_list_sources,

        )



        total = await request_list.get_total_count()

        Actor.log.info(f'Loaded {total} requests from input')



        # Process requests from the list

        while request := await request_list.fetch_next_request():

            Actor.log.info(f'Processing {request.url}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Secret input fields[](#secret-input-fields)

The Apify platform supports [secret input fields](https://docs.apify.com/platform/actors/development/secret-input) that are encrypted before being stored. When you mark an input field as `"isSecret": true` in your Actor's [input schema](https://docs.apify.com/platform/actors/development/input-schema), the platform encrypts the value with the Actor's public key.

No special handling is needed in your code — when you call [`Actor.get_input`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_input), encrypted fields are automatically decrypted using the Actor's private key, which is provided by the platform via environment variables. You receive the plaintext values directly.
