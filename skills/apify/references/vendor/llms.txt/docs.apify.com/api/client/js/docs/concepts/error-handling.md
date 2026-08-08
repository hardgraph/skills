# Error handling and retries

Copy for LLM

Based on the endpoint, the client automatically extracts the relevant data and returns it in the expected format. Date strings are automatically converted to `Date` objects. For exceptions, the client throws an [`ApifyApiError`](https://docs.apify.com/api/client/js/api/client/js/reference/class/ApifyApiError.md), which wraps the plain JSON errors returned by API and enriches them with other contexts for easier debugging.

```
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({ token: 'MY-APIFY-TOKEN' });



try {

    const { items } = await client.dataset('non-existing-dataset-id').listItems();

} catch (error) {

    // The error is an instance of ApifyApiError

    const { message, type, statusCode, clientMethod, path } = error;

    // Log error for easier debugging

    console.log({ message, statusCode, clientMethod, type });

}
```

## Retries with exponential backoff[](#retries-with-exponential-backoff)

The client automatically retries requests that fail due to network errors, Apify API internal errors (HTTP 500+), or rate limit errors (HTTP 429). By default, the client retries up to 8 times with exponential backoff starting at 500ms.

To configure retry behavior, set the `maxRetries` and `minDelayBetweenRetriesMillis` options in the `ApifyClient` constructor:

```
import { ApifyClient } from 'apify-client';



const client = new ApifyClient({

    token: 'MY-APIFY-TOKEN',

    maxRetries: 8,

    minDelayBetweenRetriesMillis: 500, // 0.5s

    timeoutSecs: 360, // 6 mins

});
```
