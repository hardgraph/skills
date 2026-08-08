---
title: Rental pricing model
url: https://docs.apify.com/actors/publishing/monetize/rental.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Publishing and monetization](https://docs.apify.com/actors/publishing.md)
  - [Monetize your Actor](https://docs.apify.com/actors/publishing/monetize.md)
previous: [Pay per event](https://docs.apify.com/actors/publishing/monetize/pay-per-event.md)
next: [Pricing and costs](https://docs.apify.com/actors/publishing/monetize/pricing-and-costs.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Rental pricing model

Rental model sunset

Apify is sunsetting the rental pricing model. The following changes are scheduled for 2026:

* April 1 - You can no longer publish new rental Actors or change pricing on existing ones.
* October 1 - Rental Actors are fully retired. All remaining Actors are migrated to pay-per-usage pricing.

For more information, visit the [#project-rentals](https://discord.gg/QfDmA7RGFu) channel on Apify Discord.

With the rental model, you can specify a free trial period and a monthly rental price. After the trial, users with an [Apify paid plan](https://apify.com/pricing) can continue using your Actor by paying the monthly fee. You can receive 80% of the total rental fees collected each month.

## How is profit computed

Your profit is calculated from the mentioned formula:

`profit = 0.8 × rental fees`

where:

* *Rental fees*: The monthly rental fee set by the developer (for example, $30/month).

Only revenue and cost for Apify customers on paid plans are taken into consideration when computing your profit. Users on free plans are not reflected there.

## Disadvantages of the rental pricing model

### User cost confusion

Users consistently report confusion about rental pricing because they pay both the *monthly rental fee and platform usage costs*. A user might see an Actor priced at $20/month, only to discover their actual costs are $35-50 depending on usage. This can create confusion about total costs and make budgeting more difficult.

### Limited revenue scalability

The rental model, while easy to set up, is less profitable because its pricing doesn't scale with usage. Your revenue is capped at the monthly fee regardless of how much value users extract from your Actor.

### AI compatibility limitations

The growing limitation is AI compatibility. [Apify's MCP server](https://docs.apify.com/integrations/mcp.md) explicitly excludes rental Actors from search results, making them invisible to AI systems that dynamically select and execute tools. This significantly reduces your Actor's discoverability in AI workflows.

## Example of a rental pricing model

You make your Actor rental with *7-day free trial* and then *$30/month*. During the first calendar month, three users start to use your Actor:

* *User 1 (paid plan)*: Starts free trial on the 15th
* *User 2 (paid plan)*: Starts free trial on the 25th
* *User 3 (free plan)*: Starts free trial on the 20th

The first user pays their first rent 7 days after the free trial, i.e., on the 22nd of the month. The second user only starts paying the rent next month. The third user is on the Apify free plan, so after the free trial ends on the 27th of the month, they are not charged and cannot use the Actor further until they get a paid plan.

Your profit is computed only from the first user, since they are the only one who paid during this month. The revenue breakdown is:

* *Total revenue*: $30 (from User 1 only)
* *Your profit*: 80% of revenue = 0.8 × $30 = $24
