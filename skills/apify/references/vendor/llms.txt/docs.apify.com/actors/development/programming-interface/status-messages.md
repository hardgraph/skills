---
title: Status messages
url: https://docs.apify.com/actors/development/programming-interface/status-messages.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Development](https://docs.apify.com/actors/development.md)
  - [Programming interface](https://docs.apify.com/actors/development/programming-interface.md)
previous: [Environment variables](https://docs.apify.com/actors/development/programming-interface/environment-variables.md)
next: [System events](https://docs.apify.com/actors/development/programming-interface/system-events.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Status messages

***

Each Actor run has a status, represented by the `status` field. The following table describes the possible values:

| Status       | Type         | Description                                 |
| ------------ | ------------ | ------------------------------------------- |
| `READY`      | initial      | Started but not allocated to any worker yet |
| `RUNNING`    | transitional | Executing on a worker                       |
| `SUCCEEDED`  | terminal     | Finished successfully                       |
| `FAILED`     | terminal     | Run failed                                  |
| `TIMING-OUT` | transitional | Timing out now                              |
| `TIMED-OUT`  | terminal     | Timed out                                   |
| `ABORTING`   | transitional | Being aborted by user                       |
| `ABORTED`    | terminal     | Aborted by user                             |

## Status messages

In addition to the status, each Actor run has a status message (the `statusMessage` field). This message informs users about the Actor's current activity, enhancing the user experience.

![Status message](/assets/images/status-message-5a087d1952b6d9050f089ca946bffba5.png)

## Exit status message

When an Actor exits, the status message is set to either:

* A default text (e.g., *Actor finished with exit code 1*)
* A custom message (see the [exit](https://docs.apify.com/actors/development/programming-interface/basic-commands.md#exit-actor) method for details)

## Update status message

To keep users informed during the Actor's execution, update the status message periodically. Use the following code to set a status message:

**JavaScript**


```js
import { Actor } from 'apify';



await Actor.init();



// ...

await Actor.setStatusMessage('Crawled 45 of 100 pages');



await Actor.exit();
```


Update frequency

You can call the `setStatusMessage` function as often as needed. The SDK only invokes the API if the status changes, simplifying usage.

**Python**


```python
from apify import Actor



async def main():

    async with Actor:

        await Actor.set_status_message('Crawled 45 of 100 pages')

        # INFO  [Status message]: Crawled 45 of 100 pages
```


## Communicate limitations

If your Actor has specific limitations for users on the Apify free plan (e.g., restricted features, limited results), communicate these clearly to avoid confusion.

* Status messages: Use `Actor.setStatusMessage` or `Actor.exit` message to explain why a run finished early or failed (e.g., "This Actor has a special daily limit for free plan users. This was set by the Actor developer, not Apify. Upgrade to continue.").

* Provide clear error messages: Don't return generic system errors or fail the run in a way that looks like a platform issue. This frustrates users and makes troubleshooting difficult.

  <!-- -->

  * Wrong: API usage is limited to 10 results
  * Right: This Actor only allows up to 10 results for free users. Upgrade to a paid plan to receive unlimited results.

* Documentation: Clearly state any limitations in your Actor's `README` and input schema descriptions so users know what to expect before running the Actor.

  <!-- -->

  * General restrictions (like limiting the number of results) must be explained in the top-level input schema description that renders above the input editor UI.
  * Feature-specific limitations must be included in the title of an input field. The title must include explanation in parenthesis such as `(paying users only)` or `(limited for free users)`. E.g. `Max comments (paying users only)`.
