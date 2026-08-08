# Pay-per-event monetization

Copy for LLM

With the [pay-per-event pricing model](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event), users pay for specific events that are programmatically triggered from your Actor's source code. Such events might include, for example, generating a single result or calling an external API.

## Configure monetization[](#configure-monetization)

To use the pay-per-event pricing model and define the pricing, [monetize your Actor](https://docs.apify.com/platform/actors/publishing/monetize) in Apify Console.

## Charge for events[](#charge-for-events)

To charge for events, use the [`Actor.charge`](https://docs.apify.com/sdk/js/sdk/js/reference/class/Actor.md#charge) method. It records that your Actor performed a billable activity, so the Apify platform charges the user's account for it.

```
import { Actor } from 'apify';



await Actor.init();



// Charge for a single occurence of an event

await Actor.charge({ eventName: 'init' });



// Prepare some mock results

const result = [

    { word: 'Lorem' },

    { word: 'Ipsum' },

    { word: 'Dolor' },

    { word: 'Sit' },

    { word: 'Amet' },

];



// Shortcut for charging for each pushed dataset item

await Actor.pushData(result, 'result-item');



// Or you can charge for a given number of events manually

await Actor.charge({

    eventName: 'result-item',

    count: result.length,

});



await Actor.exit();
```

For details on how to maximize your profits, see also [Best practices](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event#best-practices).

### Prefer per-unit charging over batching[](#prefer-per-unit-charging-over-batching)

If you can split your work into individual units, for example scraping one page or calling one API endpoint, prefer issuing one `Actor.charge()` call per unit. Don't batch multiple events into a single call with the `count` parameter. This approach gives you better control over budget consumption:

```
import { Actor } from 'apify';



await Actor.init();



const urls = [

    'https://example.com/1',

    'https://example.com/2',

    'https://example.com/3',

];



// ✅ Preferred: charge one event at a time

for (const url of urls) {

    const { eventChargeLimitReached } = await Actor.charge({

        eventName: 'page-scraped',

    });



    if (eventChargeLimitReached) break;



    const result = { url, data: `Scraped data from ${url}` };

    await Actor.pushData(result);

}



// ⚠️ Avoid: batching charges and then doing the work

const { chargedCount } = await Actor.charge({

    eventName: 'page-scraped',

    count: urls.length,

});

// chargedCount may be less than urls.length if the budget is insufficient —

// make sure to only process chargedCount items, not all of them!

for (let i = 0; i < chargedCount; i++) {

    const result = { url: urls[i], data: `Scraped data from ${urls[i]}` };

    await Actor.pushData(result);

}



await Actor.exit();
```

If you use the `count` parameter, always check the returned `chargedCount`. It tells you how many events were charged, which may be less than what you requested.

### Monitor charging[](#monitor-charging)

For custom events, every `Actor.charge` call returns [`ChargeResult`](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ChargeResult.md). Inspect its fields to learn how much was charged.

Instead of inspecting every `ChargeResult`, you can also use the [`ChargingManager`](https://docs.apify.com/sdk/js/sdk/js/reference/class/ChargingManager.md). It provides methods for querying the remaining budget, total charged amount, and per-event charge counts.

This information lets you plan work based on the remaining budget rather than discovering the limit after the fact. It's particularly useful when charging happens in multiple places across your code, or when using a crawler where you don't directly control the main loop.

To access the [`ChargingManager`](https://docs.apify.com/sdk/js/sdk/js/reference/class/ChargingManager.md), use the [`Actor.getChargingManager()`](https://docs.apify.com/sdk/js/sdk/js/reference/class/Actor.md#getChargingManager) method:

```
import { Actor } from 'apify';



await Actor.init();



const chargingManager = Actor.getChargingManager();



// Check the total budget for this run

const maxCharge = chargingManager.getMaxTotalChargeUsd();

console.log(`Max charge: ${maxCharge}`);



// Check how many events can still be charged before reaching the limit

const remainingCharge = chargingManager.calculateMaxEventChargeCountWithinLimit(

    'result-item',

);

console.log(`Remaining number of events: ${remainingCharge}`);



await Actor.exit();
```

### Handle the charge limit[](#handle-the-charge-limit)

Your Actor users can set the maximum cost for the run. It helps users control their spending, as they won't be billed beyond the limit.

The user spending limit for the run is available in your Actor code as the `ACTOR_MAX_TOTAL_CHARGE_USD` environment variable, and [`ChargeResult`](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ChargeResult.md) already accounts for it. To control the limit, inspect the fields of `ChargeResult`:

* `eventChargeLimitReached`: Checks if the user's limit allows for another charge of the event.
* `chargeableWithinLimit`: Indicates how many events of each type can still be charged within the remaining budget.
* `chargedCount`: Indicates how many events were billed by the call.

```
import { Actor } from 'apify';



await Actor.init();



const urls = [

    'https://example.com/1',

    'https://example.com/2',

    'https://example.com/3',

];



for (const url of urls) {

    // Do some expensive work (e.g. scraping, API calls)

    const result = { url, data: `Scraped data from ${url}` };



    // pushData returns a ChargeResult - check it to know if the budget ran out

    const { eventChargeLimitReached } = await Actor.pushData(

        result,

        'result-item',

    );



    if (eventChargeLimitReached) {

        console.log('Charge limit reached, stopping the Actor');

        break;

    }

}



await Actor.exit();
```

When the charge limit is reached, [`Actor.charge`](https://docs.apify.com/sdk/js/sdk/js/reference/class/Actor.md#charge) stops charging and [`Actor.push_data`](https://docs.apify.com/sdk/js/sdk/js/reference/class/Actor.md#pushData) stops pushing data. The platform then aborts the run automatically. However, the run keeps consuming platform resources for a short time before it stops. For details, see [Handle graceful shutdown](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event#handle-graceful-shutdown).

## Test monetization locally[](#test-monetization-locally)

Before releasing your monetization code to the public, test it locally. To make your Actor work in pay-per-event mode, pass it the `ACTOR_TEST_PAY_PER_EVENT` environment variable:

```
ACTOR_TEST_PAY_PER_EVENT=true npm start
```

### View the log[](#view-the-log)

To inspect the results of your tests, open the `charging-log` dataset. By default, it's stored in the `storage/datasets/charging-log/` directory.

The log contains all the events charged throughout the run. Because pricing configuration is stored by the Apify platform, all events have a default price of $1.

Note that this log isn't available when running the Actor in production on the Apify platform.

## Transitioning from a different pricing model[](#transitioning-from-a-different-pricing-model)

When you plan to start using the pay-per-event pricing model for an Actor that is already monetized with a different pricing model, your source code will need support both pricing models during the transition period enforced by the Apify platform. Arguably the most frequent case is the transition from the pay-per-result model which utilizes the `ACTOR_MAX_PAID_DATASET_ITEMS` environment variable to prevent returning unpaid dataset items. The following is an example how to handle such scenarios. The key part is the [`ChargingManager.getPricingInfo`](https://docs.apify.com/sdk/js/sdk/js/reference/class/ChargingManager.md#getPricingInfo) method which returns information about the current pricing model.

```
import { Actor } from 'apify';



await Actor.init();



// Check the dataset because there might already be items if the run migrated or was restarted

const defaultDataset = await Actor.openDataset();

let chargedItems = (await defaultDataset.getInfo())!.itemCount;



if (Actor.getChargingManager().getPricingInfo().isPayPerEvent) {

    await Actor.pushData({ hello: 'world' }, 'dataset-item');

} else if (chargedItems < Number(process.env.ACTOR_MAX_PAID_DATASET_ITEMS)) {

    await Actor.pushData({ hello: 'world' });

    chargedItems += 1;

}



await Actor.exit();
```
