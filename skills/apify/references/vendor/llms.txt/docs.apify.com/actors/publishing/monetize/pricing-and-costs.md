---
title: Pricing and costs
url: https://docs.apify.com/actors/publishing/monetize/pricing-and-costs.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Actors](https://docs.apify.com/actors.md)
  - [Publishing and monetization](https://docs.apify.com/actors/publishing.md)
  - [Monetize your Actor](https://docs.apify.com/actors/publishing/monetize.md)
previous: [Rental pricing model](https://docs.apify.com/actors/publishing/monetize/rental.md)
next: [Automated testing](https://docs.apify.com/actors/publishing/test.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Pricing and costs

## Compute costs for PPE Actors

Profit is computed using the formula `(0.8 * revenue) - costs`. In this section, we'll explain how the `costs` component is calculated.

When paying users run your Actor, it generates platform usage in the form of compute units, data traffic, API operations etc. This usage determines the `costs` in the profit formula above.

*FREE* tier usage

Platform usage by *FREE* tier users is covered by Apify and does not contribute to your costs.

To calculate your costs for a specific run by a paying user, multiply the unit cost of each service by the quantity consumed. For example, if a *BRONZE* tier user run uses 10 compute units (CUs) at $0.2/CU, your cost would be $2.

If your Actor uses tiered pricing, the user's discount tier determines the unit costs applied to their runs. Your costs are lower for higher tiers, enabling you to offer more competitive pricing to these customers, while sustaining healthy profit margins.

The following table summarizes the platform unit costs used for your cost computation across different discount tiers.

| Service (unit)                       | *FREE*  | *BRONZE* | *SILVER* | *GOLD*   |
| ------------------------------------ | ------- | -------- | -------- | -------- |
| Compute unit (per CU)                | $0.2    | $0.2     | $0.16    | $0.13    |
| Residential proxies (per GB)         | $8      | $8       | $7.5     | $7       |
| SERPs proxy (per 1,000 SERPs)        | $2.5    | $2.5     | $2       | $1.7     |
| Data transfer - external (per GB)    | $0.2    | $0.2     | $0.19    | $0.18    |
| Data transfer - internal (per GB)    | $0.05   | $0.05    | $0.045   | $0.04    |
| Dataset - reads (per 1,000 reads)    | $0.0004 | $0.0004  | $0.00036 | $0.00032 |
| Dataset - writes (per 1,000 writes)  | $0.005  | $0.005   | $0.0045  | $0.004   |
| Key-value store - reads (per 1,000)  | $0.005  | $0.005   | $0.0045  | $0.004   |
| Key-value store - writes (per 1,000) | $0.05   | $0.05    | $0.045   | $0.04    |
| Key-value store - lists (per 1,000)  | $0.05   | $0.05    | $0.045   | $0.04    |
| Request queue - reads (per 1,000)    | $0.004  | $0.004   | $0.0036  | $0.0032  |
| Request queue - writes (per 1,000)   | $0.01   | $0.01    | $0.009   | $0.008   |

If you decide not to offer tiered discounts on your Actor, the unit prices for *FREE* tier apply. To offer enterprise level services and unlock even cheaper unit prices for enterprise customers, please reach out to us.

Cost of PPE Actors in Standby mode

When you monetize your Actor in Standby mode using pay-per-event mode only, you are not responsible for covering platform usage costs of your users' runs.

## Discount tiers and pricing strategy

Each user running your PPE Actor belongs to a discount tier:

* *FREE*
* *BRONZE*
* *SILVER*
* *GOLD*

You can define different prices for different tiers. While optional, we recommend offering progressively lower prices for higher discount tiers. This approach can significantly improve attractiveness of your Actor to large enterprise customers who may spend thousands or tens of thousands of dollars on it.

Your platform costs are also lower for these higher tier, which helps maintain healthy profit margins. This is further detailed in the Computing your costs for PPE Actors section.

## Implement discount tiers

By default, we advise against setting excessively high prices for *FREE* tier users, as this can limit the ability to evaluate your Actor thoroughly. However, in certain situations, such as protecting your Actor from fraudulent activity or excessive use of your internal APIs, a higher price for *FREE* tier users might be justified.

During an Actor run, you can identify the user's discount tier through Actor run environment variables or by querying user data via the Apify API. This capability allows you to offer premium features or differentiated service levels to users in higher discount tiers.

## Additional benefits and enterprise tiers

Actors that implement tiered pricing also receive additional benefits like enhanced visibility in Apify Store, making your Actor more discoverable to potential users.

In addition to the standard tiers, Apify provides further tiers specifically for enterprise customers, including *PLATINUM* and *DIAMOND* tiers. If you are interested in offering enterprise-level services and attracting major clients, please contact us.
