# Pay-per-event monetization

Copy for LLM

With the [pay-per-event pricing model](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event), users pay for specific events that are programmatically triggered from your Actor's source code. Such events might include, for example, generating a single result or calling an external API.

## Configure monetization[](#configure-monetization)

To use the pay-per-event pricing model and define the pricing, [monetize your Actor](https://docs.apify.com/platform/actors/publishing/monetize) in Apify Console.

## Charge for events[](#charge-for-events)

To charge for events, use the [`Actor.charge`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#charge) method. It records that your Actor performed a billable activity, so the Apify platform charges the user's account for it.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIGhpZ2hsaWdodC1zdGFydFxcbiAgICAgICAgIyBDaGFyZ2UgZm9yIGEgc2luZ2xlIG9jY3VycmVuY2Ugb2YgYW4gZXZlbnRcXG4gICAgICAgIGF3YWl0IEFjdG9yLmNoYXJnZShldmVudF9uYW1lPSdpbml0JylcXG4gICAgICAgICMgaGlnaGxpZ2h0LWVuZFxcblxcbiAgICAgICAgIyBQcmVwYXJlIHNvbWUgbW9jayByZXN1bHRzXFxuICAgICAgICByZXN1bHQgPSBbXFxuICAgICAgICAgICAgeyd3b3JkJzogJ0xvcmVtJ30sXFxuICAgICAgICAgICAgeyd3b3JkJzogJ0lwc3VtJ30sXFxuICAgICAgICAgICAgeyd3b3JkJzogJ0RvbG9yJ30sXFxuICAgICAgICAgICAgeyd3b3JkJzogJ1NpdCd9LFxcbiAgICAgICAgICAgIHsnd29yZCc6ICdBbWV0J30sXFxuICAgICAgICBdXFxuICAgICAgICAjIGhpZ2hsaWdodC1zdGFydFxcbiAgICAgICAgIyBTaG9ydGN1dCBmb3IgY2hhcmdpbmcgZm9yIGVhY2ggcHVzaGVkIGRhdGFzZXQgaXRlbVxcbiAgICAgICAgYXdhaXQgQWN0b3IucHVzaF9kYXRhKHJlc3VsdCwgY2hhcmdlZF9ldmVudF9uYW1lPSdyZXN1bHQtaXRlbScpXFxuICAgICAgICAjIGhpZ2hsaWdodC1lbmRcXG5cXG4gICAgICAgICMgaGlnaGxpZ2h0LXN0YXJ0XFxuICAgICAgICAjIE9yIHlvdSBjYW4gY2hhcmdlIGZvciBhIGdpdmVuIG51bWJlciBvZiBldmVudHMgbWFudWFsbHlcXG4gICAgICAgIGF3YWl0IEFjdG9yLmNoYXJnZShcXG4gICAgICAgICAgICBldmVudF9uYW1lPSdyZXN1bHQtaXRlbScsXFxuICAgICAgICAgICAgY291bnQ9bGVuKHJlc3VsdCksXFxuICAgICAgICApXFxuICAgICAgICAjIGhpZ2hsaWdodC1lbmRcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.MgHwzmRzCibi-CjnUSbT1fgbsJqXbwQ1YwPywSXseYY\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Charge for a single occurrence of an event

        await Actor.charge(event_name='init')



        # Prepare some mock results

        result = [

            {'word': 'Lorem'},

            {'word': 'Ipsum'},

            {'word': 'Dolor'},

            {'word': 'Sit'},

            {'word': 'Amet'},

        ]

        # Shortcut for charging for each pushed dataset item

        await Actor.push_data(result, charged_event_name='result-item')



        # Or you can charge for a given number of events manually

        await Actor.charge(

            event_name='result-item',

            count=len(result),

        )





if __name__ == '__main__':

    asyncio.run(main())
```

For details on how to maximize your profits, see also [Best practices](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event#best-practices).

### Prefer per-unit charging[](#prefer-per-unit-charging)

If you can split your work into individual units, for example scraping one page or calling one API endpoint, prefer issuing one `Actor.charge()` call per unit. Don't batch multiple events into a single call with the `count` parameter. This approach gives you better control over budget consumption:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICB1cmxzID0gW1xcbiAgICAgICAgICAgICdodHRwczovL2V4YW1wbGUuY29tLzEnLFxcbiAgICAgICAgICAgICdodHRwczovL2V4YW1wbGUuY29tLzInLFxcbiAgICAgICAgICAgICdodHRwczovL2V4YW1wbGUuY29tLzMnLFxcbiAgICAgICAgXVxcblxcbiAgICAgICAgZm9yIHVybCBpbiB1cmxzOlxcbiAgICAgICAgICAgICMgQ2hhcmdlIGZvciBhIHNpbmdsZSBldmVudFxcbiAgICAgICAgICAgIGNoYXJnZV9yZXN1bHQgPSBhd2FpdCBBY3Rvci5jaGFyZ2UoXFxuICAgICAgICAgICAgICAgIGV2ZW50X25hbWU9J3BhZ2Utc2NyYXBlZCcsXFxuICAgICAgICAgICAgKVxcblxcbiAgICAgICAgICAgIGlmIGNoYXJnZV9yZXN1bHQuZXZlbnRfY2hhcmdlX2xpbWl0X3JlYWNoZWQ6XFxuICAgICAgICAgICAgICAgIGJyZWFrXFxuXFxuICAgICAgICAgICAgcmVzdWx0ID0geyd1cmwnOiB1cmwsICdkYXRhJzogZidTY3JhcGVkIGRhdGEgZnJvbSB7dXJsfSd9XFxuXFxuICAgICAgICAgICAgIyBQdXNoIHRoZSByZXN1bHQgdG8gdGhlIGRhdGFzZXRcXG4gICAgICAgICAgICBhd2FpdCBBY3Rvci5wdXNoX2RhdGEocmVzdWx0KVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.IcjQ_sN4xB7pDCbueYU9HNFDMpl8mzy1u8KGXZ5G-CI\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        urls = [

            'https://example.com/1',

            'https://example.com/2',

            'https://example.com/3',

        ]



        for url in urls:

            # Charge for a single event

            charge_result = await Actor.charge(

                event_name='page-scraped',

            )



            if charge_result.event_charge_limit_reached:

                break



            result = {'url': url, 'data': f'Scraped data from {url}'}



            # Push the result to the dataset

            await Actor.push_data(result)





if __name__ == '__main__':

    asyncio.run(main())
```

If you use the `count` parameter, always check the returned `charged_count`. It tells you how many events were charged, which may be less than what you requested.

### Monitor charging[](#monitor-charging)

For both custom and synthetic events, every `Actor.charge` call returns a [`ChargeResult`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ChargeResult.md). Inspect its fields to learn how much was charged.

Instead of inspecting every `ChargeResult`, you can also use the [`ChargingManager`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ChargingManager.md). It provides methods for querying the remaining budget, total charged amount, and per-event charge counts.

This information lets you plan work based on the remaining budget rather than discovering the limit after the fact. It's particularly useful when charging happens in multiple places across your code, or when using a crawler where you don't directly control the main loop.

To access the [`ChargingManager`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ChargingManager.md), use the [`Actor.get_charging_manager()`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#get_charging_manager) method:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBjaGFyZ2luZ19tYW5hZ2VyID0gQWN0b3IuZ2V0X2NoYXJnaW5nX21hbmFnZXIoKVxcblxcbiAgICAgICAgIyBDaGVjayB0aGUgdG90YWwgYnVkZ2V0IGZvciB0aGlzIHJ1blxcbiAgICAgICAgbWF4X2NoYXJnZSA9IGNoYXJnaW5nX21hbmFnZXIuZ2V0X21heF90b3RhbF9jaGFyZ2VfdXNkKClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnTWF4IHRvdGFsIGNoYXJnZTogJHttYXhfY2hhcmdlfScpXFxuXFxuICAgICAgICAjIENoZWNrIGhvdyBtYW55IGV2ZW50cyBjYW4gc3RpbGwgYmUgY2hhcmdlZFxcbiAgICAgICAgcmVtYWluaW5nID0gY2hhcmdpbmdfbWFuYWdlci5jYWxjdWxhdGVfbWF4X2V2ZW50X2NoYXJnZV9jb3VudF93aXRoaW5fbGltaXQoXFxuICAgICAgICAgICAgJ3Jlc3VsdC1zY3JhcGVkJyxcXG4gICAgICAgIClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnUmVtYWluaW5nIGNoYXJnZWFibGUgZXZlbnRzOiB7cmVtYWluaW5nfScpXFxuXFxuICAgICAgICAjIEdldCB0aGUgdG90YWwgYW1vdW50IGNoYXJnZWQgc28gZmFyXFxuICAgICAgICB0b3RhbF9jaGFyZ2VkID0gY2hhcmdpbmdfbWFuYWdlci5jYWxjdWxhdGVfdG90YWxfY2hhcmdlZF9hbW91bnQoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidUb3RhbCBjaGFyZ2VkIHNvIGZhcjogJHt0b3RhbF9jaGFyZ2VkfScpXFxuXFxuICAgICAgICAjIENoZWNrIGFsbCBldmVudCB0eXBlcyBhbmQgdGhlaXIgcmVtYWluaW5nIGNvdW50c1xcbiAgICAgICAgY2hhcmdlYWJsZSA9IGNoYXJnaW5nX21hbmFnZXIuY29tcHV0ZV9jaGFyZ2VhYmxlKClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnQ2hhcmdlYWJsZSBldmVudHM6IHtjaGFyZ2VhYmxlfScpXFxuXFxuICAgICAgICAjIENoZWNrIGlmIGEgc3BlY2lmaWMgZXZlbnQgdHlwZSBoYXMgcmVhY2hlZCBpdHMgbGltaXRcXG4gICAgICAgIGlmIGNoYXJnaW5nX21hbmFnZXIuaXNfZXZlbnRfY2hhcmdlX2xpbWl0X3JlYWNoZWQoJ3Jlc3VsdC1zY3JhcGVkJyk6XFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oJ0J1ZGdldCBleGhhdXN0ZWQgZm9yIHJlc3VsdC1zY3JhcGVkIGV2ZW50cycpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.2EehFpnku69yLXWFzng1TEsx5iqLl7SyTqMO62dmHio\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        charging_manager = Actor.get_charging_manager()



        # Check the total budget for this run

        max_charge = charging_manager.get_max_total_charge_usd()

        Actor.log.info(f'Max total charge: ${max_charge}')



        # Check how many events can still be charged

        remaining = charging_manager.calculate_max_event_charge_count_within_limit(

            'result-scraped',

        )

        Actor.log.info(f'Remaining chargeable events: {remaining}')



        # Get the total amount charged so far

        total_charged = charging_manager.calculate_total_charged_amount()

        Actor.log.info(f'Total charged so far: ${total_charged}')



        # Check all event types and their remaining counts

        chargeable = charging_manager.compute_chargeable()

        Actor.log.info(f'Chargeable events: {chargeable}')



        # Check if a specific event type has reached its limit

        if charging_manager.is_event_charge_limit_reached('result-scraped'):

            Actor.log.info('Budget exhausted for result-scraped events')





if __name__ == '__main__':

    asyncio.run(main())
```

### Handle the charge limit[](#handle-the-charge-limit)

Your Actor users can set the maximum cost for the run. It helps users control their spending, as they won't be billed beyond the limit.

The user spending limit for the run is available in your Actor code as the `ACTOR_MAX_TOTAL_CHARGE_USD` environment variable, and [`ChargeResult`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ChargeResult.md) already accounts for it. To control the limit, inspect the fields of `ChargeResult`:

* `event_charge_limit_reached`: Checks if the user's limit allows for another charge of the event.
* `chargeable_within_limit`: Indicates how many events of each type can still be charged within the remaining budget.
* `charged_count`: Indicates how many events were billed by the call.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICB1cmxzID0gW1xcbiAgICAgICAgICAgICdodHRwczovL2V4YW1wbGUuY29tLzEnLFxcbiAgICAgICAgICAgICdodHRwczovL2V4YW1wbGUuY29tLzInLFxcbiAgICAgICAgICAgICdodHRwczovL2V4YW1wbGUuY29tLzMnLFxcbiAgICAgICAgXVxcblxcbiAgICAgICAgZm9yIHVybCBpbiB1cmxzOlxcbiAgICAgICAgICAgICMgRG8gc29tZSBleHBlbnNpdmUgd29yayAoZS5nLiBzY3JhcGluZywgQVBJIGNhbGxzKVxcbiAgICAgICAgICAgIHJlc3VsdCA9IHsndXJsJzogdXJsLCAnZGF0YSc6IGYnU2NyYXBlZCBkYXRhIGZyb20ge3VybH0nfVxcblxcbiAgICAgICAgICAgICMgaGlnaGxpZ2h0LXN0YXJ0XFxuICAgICAgICAgICAgIyBwdXNoX2RhdGEgcmV0dXJucyBhIENoYXJnZVJlc3VsdCwgY2hlY2sgaXQgdG8ga25vdyBpZiB0aGUgYnVkZ2V0IHJhbiBvdXRcXG4gICAgICAgICAgICBjaGFyZ2VfcmVzdWx0ID0gYXdhaXQgQWN0b3IucHVzaF9kYXRhKFxcbiAgICAgICAgICAgICAgICByZXN1bHQsIGNoYXJnZWRfZXZlbnRfbmFtZT0ncmVzdWx0LWl0ZW0nXFxuICAgICAgICAgICAgKVxcblxcbiAgICAgICAgICAgIGlmIGNoYXJnZV9yZXN1bHQuZXZlbnRfY2hhcmdlX2xpbWl0X3JlYWNoZWQ6XFxuICAgICAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKCdDaGFyZ2UgbGltaXQgcmVhY2hlZCwgc3RvcHBpbmcgdGhlIEFjdG9yJylcXG4gICAgICAgICAgICAgICAgYnJlYWtcXG4gICAgICAgICAgICAjIGhpZ2hsaWdodC1lbmRcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.iUR00utanOiWtcXqFN7hieQ4siqFSm0xYZ8fFVo8j_E\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        urls = [

            'https://example.com/1',

            'https://example.com/2',

            'https://example.com/3',

        ]



        for url in urls:

            # Do some expensive work (e.g. scraping, API calls)

            result = {'url': url, 'data': f'Scraped data from {url}'}



            # push_data returns a ChargeResult, check it to know if the budget ran out

            charge_result = await Actor.push_data(

                result, charged_event_name='result-item'

            )



            if charge_result.event_charge_limit_reached:

                Actor.log.info('Charge limit reached, stopping the Actor')

                break





if __name__ == '__main__':

    asyncio.run(main())
```

When the charge limit is reached, [`Actor.charge`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#charge) stops charging and [`Actor.push_data`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#push_data) stops pushing data. The platform then aborts the run automatically. However, the run keeps consuming platform resources for a short time before it stops. For details, see [Handle graceful shutdown](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event#handle-graceful-shutdown).

## Test monetization locally[](#test-monetization-locally)

Before releasing your monetization code to the public, test it locally. To make your Actor work in pay-per-event mode, pass it the `ACTOR_TEST_PAY_PER_EVENT` environment variable:

```
ACTOR_TEST_PAY_PER_EVENT=true python -m youractor
```

In this mode, nothing is billed, but every charge call is logged into a local `charging-log` dataset.

### View the log[](#view-the-log)

To inspect the results of your tests, open the `charging-log` dataset. By default, it's stored in the `storage/datasets/charging-log/` directory.

The log contains all the events charged throughout the run. Because pricing configuration is stored by the Apify platform, all events have a default price of $1.

Note that this log isn't available when running the Actor in production on the Apify platform.

## Transition from a different pricing model[](#transition-from-a-different-pricing-model)

When you plan to start using the pay-per-event pricing model for an Actor that is already monetized with a different pricing model, your source code must support both pricing models during the transition period enforced by the Apify platform. The most frequent case is the transition from the pay-per-result model which utilizes the `ACTOR_MAX_PAID_DATASET_ITEMS` environment variable to prevent returning unpaid dataset items. The following is an example how to handle such scenarios. The key part is the [`ChargingManager.get_pricing_info()`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ChargingManager.md#get_pricing_info) method which returns information about the current pricing model.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIENoZWNrIHRoZSBkYXRhc2V0IGJlY2F1c2UgdGhlcmUgbWlnaHQgYWxyZWFkeSBiZSBpdGVtc1xcbiAgICAgICAgIyBpZiB0aGUgcnVuIG1pZ3JhdGVkIG9yIHdhcyByZXN0YXJ0ZWRcXG4gICAgICAgIGRlZmF1bHRfZGF0YXNldCA9IGF3YWl0IEFjdG9yLm9wZW5fZGF0YXNldCgpXFxuICAgICAgICBtZXRhZGF0YSA9IGF3YWl0IGRlZmF1bHRfZGF0YXNldC5nZXRfbWV0YWRhdGEoKVxcbiAgICAgICAgY2hhcmdlZF9pdGVtcyA9IG1ldGFkYXRhLml0ZW1fY291bnRcXG5cXG4gICAgICAgICMgaGlnaGxpZ2h0LXN0YXJ0XFxuICAgICAgICBpZiBBY3Rvci5nZXRfY2hhcmdpbmdfbWFuYWdlcigpLmdldF9wcmljaW5nX2luZm8oKS5pc19wYXlfcGVyX2V2ZW50OlxcbiAgICAgICAgICAgICMgaGlnaGxpZ2h0LWVuZFxcbiAgICAgICAgICAgIGF3YWl0IEFjdG9yLnB1c2hfZGF0YSh7J2hlbGxvJzogJ3dvcmxkJ30sIGNoYXJnZWRfZXZlbnRfbmFtZT0nZGF0YXNldC1pdGVtJylcXG4gICAgICAgIGVsaWYgY2hhcmdlZF9pdGVtcyA8IChBY3Rvci5jb25maWd1cmF0aW9uLm1heF9wYWlkX2RhdGFzZXRfaXRlbXMgb3IgMCk6XFxuICAgICAgICAgICAgYXdhaXQgQWN0b3IucHVzaF9kYXRhKHsnaGVsbG8nOiAnd29ybGQnfSlcXG4gICAgICAgICAgICBjaGFyZ2VkX2l0ZW1zICs9IDFcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.oj63VozEjiSZXAO2TXRu3t8vGmWkOX7kFNNSAlODbv4\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        # Check the dataset because there might already be items

        # if the run migrated or was restarted

        default_dataset = await Actor.open_dataset()

        metadata = await default_dataset.get_metadata()

        charged_items = metadata.item_count



        if Actor.get_charging_manager().get_pricing_info().is_pay_per_event:

            await Actor.push_data({'hello': 'world'}, charged_event_name='dataset-item')

        elif charged_items < (Actor.configuration.max_paid_dataset_items or 0):

            await Actor.push_data({'hello': 'world'})

            charged_items += 1





if __name__ == '__main__':

    asyncio.run(main())
```
