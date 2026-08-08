---
id: "integrations/scheduled-data-exports"
title: "Scheduled Data Exports"
description: "Scheduled Data Exports are available to all users signed up after September '23, the legacy Grow and Pro plans, and Enterprise plans. If you're on a legacy Free or Starter plan and want to access this integration, migrate to our new pricing via your billing settings."
permalink: "/docs/integrations/scheduled-data-exports"
slug: "scheduled-data-exports"
version: "current"
original_source: "docs/integrations/scheduled-data-exports.mdx"
---

> **AI agents:** This is the Markdown version of a RevenueCat documentation page. For the complete documentation index, see [llms.txt](https://www.revenuecat.com/docs/llms.txt).

:::success\[Pro integration]
Scheduled Data Exports are available to all users signed up after September '23, the legacy Grow and Pro plans, and Enterprise plans. If you're on a legacy Free or Starter plan and want to access this integration, migrate to our new pricing via your [billing settings](https://app.revenuecat.com/settings/billing).
:::

RevenueCat delivers your apps' data to a cloud storage provider on a schedule you control. Each Scheduled Data Export integration is made up of one or more **feeds** — datasets, each with its own column catalog. You choose which feeds to enable, which columns to include, what delivery cadence to run on, and whether to receive CSV or Parquet files. See [Feeds](#feeds) for the datasets you can export today.

## Setup instructions

- [Amazon S3](https://www.revenuecat.com/docs/integrations/scheduled-data-exports/scheduled-data-exports-s3)
- [Google Cloud Storage](https://www.revenuecat.com/docs/integrations/scheduled-data-exports/scheduled-data-exports-gcp)
- [Azure Blob Storage](https://www.revenuecat.com/docs/integrations/scheduled-data-exports/scheduled-data-exports-azure)

## Feeds

A **feed** is a dataset that RevenueCat exports on a schedule. Each feed has its own column catalog in the dashboard and lands in its own location in your storage destination.

The feeds available today are:

| Feed                 | Description                                                                | Output path                                                         |
| -------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Transactions**     | Per-transaction subscriber and revenue data.                               | `<base>/<date>/transactions_<timestamp>.<ext>`                      |
| **Virtual Currency** | Per-row virtual currency ledger: balance adjustments with running balance. | `<base>/virtual_currency/<date>/virtual_currency_<timestamp>.<ext>` |

`<base>` is the optional path prefix you configure on the integration (empty by default). Files are organized into dated subfolders such as `2023-01-01/`. Each feed writes to its own subfolder so feeds never collide in the same destination — the exception is Transactions, which writes directly under `<base>` to keep existing integrations' paths stable. Parquet and multi-file deliveries use folder-based names instead of the single-file `_<timestamp>` suffix.

Enable feeds from the integration settings in the dashboard. You can enable any combination of the available feeds on a single integration.

:::info\[Feeds share one storage connection]
All feeds on an integration use the same storage connection. You configure the bucket and credentials once, and every feed you enable delivers to that same bucket — what changes per feed is only the path within the bucket (see the output paths above). Enabling an additional feed doesn't create a second connection or point to a different bucket; its files simply arrive under their own path alongside your other feeds.
:::

## Selecting columns

Each feed has a **column catalog** that lists every available column for that feed. You choose which columns belong in that feed's export, save, and future deliveries reflect that selection — past exports aren't rewritten. The default selection covers the most commonly used fields; you can expand or trim it at any time.

For step-by-step instructions, see [Selecting columns for an export](https://www.revenuecat.com/docs/integrations/scheduled-data-exports/selecting-columns).

## Export frequency

By default, exports run once per day. How much data each delivery contains depends on the feed:

- **Incremental feeds** can send only new and updated rows when **Receive new and updated transactions only** is enabled, which reduces the volume you process per delivery. On Enterprise plans, these feeds can be delivered every 4, 6, 8, 12, or 24 hours. For any cadence outside those options, contact your Customer Success Manager or visit our [pricing page](https://www.revenuecat.com/pricing/).
- **Full-snapshot feeds** re-export the entire dataset on each run and don't support incremental delivery.

Each feed's delivery behavior is noted in its section under [Available feeds](#available-feeds).

:::info
The date and time set in **Next export start time** is when the next export should start being generated — not when it will be delivered.
:::

## Output format

Scheduled Data Exports can deliver CSV or Parquet files. You can change the format from the integration settings in the dashboard.

CSV exports are gzip compressed, comma-delimited, and by default produced as a single file per delivery. For CSV exports larger than approximately 1 GB, enable **Split export into multiple files** to break each delivery into smaller files.

Parquet exports use Snappy compression and always produce multiple files per delivery; RevenueCat does not merge them.

## Available feeds

The tables below list every column available for each feed. New integrations start with a default selection of the most commonly used columns; you can adjust each feed's selection from the column catalog in the dashboard. For step-by-step instructions, see [Selecting columns for an export](https://www.revenuecat.com/docs/integrations/scheduled-data-exports/selecting-columns).

### Transactions

Per-transaction subscriber and revenue data. Delivered to `<base>/<date>/transactions_<timestamp>.<ext>`. This is an incremental feed: with **Receive new and updated transactions only** enabled, each delivery includes just the rows that changed since the last one.

:::info
All dates and times are provided in UTC.
:::

| Header                                 | Description                                                                                                                                                                                                                                                                                                                                                                          | Type         | Example value                                                                                                                                                                                                                                | Can be null |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `rc_original_app_user_id`              | Can be used as a unique user identifier to find all of a user's transactions.                                                                                                                                                                                                                                                                                                        | string       | `$RCAnonymousID:87c6049c58069238dce29853916d624c`                                                                                                                                                                                            |             |
| `rc_last_seen_app_user_id_alias`       | Can be used together with `rc_original_app_user_id` to match transactions with user identifiers in your systems.                                                                                                                                                                                                                                                                     | string       | `$RCAnonymousID:87c6049c58069238dce29853916d624c`                                                                                                                                                                                            |             |
| `country`                              | Store country of a transaction when known, or an IP-based estimate of a subscriber's country when not known.                                                                                                                                                                                                                                                                         | string       | `GB`                                                                                                                                                                                                                                         | ✅          |
| `country_source`                       | `from_sdk` when the store country of a transaction is known, or `estimated` when `country` is sourced from an IP-based estimate.                                                                                                                                                                                                                                                     | string       | `from_sdk`                                                                                                                                                                                                                                   | ✅          |
| `product_identifier`                   | The product identifier that was purchased.                                                                                                                                                                                                                                                                                                                                           | string       | `rc_subscription_monthly`                                                                                                                                                                                                                    |             |
| `product_display_name`                 | The display name of the product identifier if one has been set                                                                                                                                                                                                                                                                                                                       | string       | `Monthly $9.99`                                                                                                                                                                                                                              | ✅          |
| `product_duration`                     | The standard duration of the product if one is known by RevenueCat. May be null if RevenueCat does not know the authoritative duration. <br> <br>`product_duration` does not represent the trial or introductory period length of a transaction, it only represents the standard duration of the product that's been subscribed to.                                              | string       | `P1M`                                                                                                                                                                                                                                        | ✅          |
| `start_time`                           | Purchase time of transaction.                                                                                                                                                                                                                                                                                                                                                        | datetime     | `2023-01-01 08:27:06`                                                                                                                                                                                                                        |             |
| `end_time`                             | Expected expiration time of subscription. Null when `is_auto_renewable = false` <br>For Google Play, `end_time` can be before `start_time` to indicate an invalid transaction (e.g. billing issue).                                                                                                                                                                                | datetime     | `2023-02-01 08:27:06`                                                                                                                                                                                                                        | ✅          |
| `grace_period_end_time`                | Expiration time of a grace period (if applicable) for a subscription. Will remain set while a subscription is in its grace period, or if it exited its grace period without renewing. Null when a subscription is not in a grace period or expiration was not due to a grace period.                                                                                                 | datetime     | `2023-02-17 08:27:06`                                                                                                                                                                                                                        | ✅          |
| `effective_end_time`                   | Single reference point of a subscriber’s expiration and entitlement revocation; inclusive of each store’s logic for refunds, grace periods, etc.                                                                                                                                                                                                                                     | datetime     | `2023-02-17 08:27:06`                                                                                                                                                                                                                        | ✅          |
| `store`                                | The source of the transaction. Can be `app_store`, `play_store`, `stripe`, or [`promotional`](https://www.revenuecat.com/docs/dashboard-and-metrics/customer-profile#entitlements).                                                                                                                                                                                                                                 | string       | `play_store`                                                                                                                                                                                                                                 |             |
| `is_auto_renewable`                    | `true` for auto-renewable subscriptions, `false` otherwise.                                                                                                                                                                                                                                                                                                                          | boolean      | `true`                                                                                                                                                                                                                                       |             |
| `is_trial_period`                      | `true` if the transaction was a trial.                                                                                                                                                                                                                                                                                                                                               | boolean      | `false`                                                                                                                                                                                                                                      |             |
| `is_in_intro_offer_period`             | `true` if the transaction is in an introductory offer period.                                                                                                                                                                                                                                                                                                                        | boolean      | `false`                                                                                                                                                                                                                                      |             |
| `is_sandbox`                           | `true` for transactions made in a [sandbox environment](https://www.revenuecat.com/docs/test-and-launch/sandbox).                                                                                                                                                                                                                                                                                                   | boolean      | `false`                                                                                                                                                                                                                                      |             |
| `price_in_usd`                         | The revenue (converted to USD) generated from the transaction after accounting for full and partial refunds. Can be null if product prices haven't been collected from the user's device.                                                                                                                                                                                            | float        | `0`                                                                                                                                                                                                                                          | ✅          |
| `purchase_price_in_usd`                | The gross revenue (converted to USD) generated from the transaction. Remains set for refunded transactions. Can be null if product prices haven't been collected from the user's device.                                                                                                                                                                                             | float        | `9.99`                                                                                                                                                                                                                                       | ✅          |
| `takehome_percentage`                  | \[DEPRECATED] The estimated percentage of the transaction price that will be paid out to developers after commissions, but before VAT and DST taxes are taken into account. (will be either 0.7 or 0.85) <br> <br>We recommend using `tax_percentage` and `commission_percentage` to calculate proceeds instead. [Learn more here](https://www.revenuecat.com/docs/dashboard-and-metrics/taxes-and-commissions). | float        | `0.7`                                                                                                                                                                                                                                        |             |
| `tax_percentage`                       | The portion of a transaction’s price that will be deducted by the store for taxes. VAT & Digital Services Taxes may be withheld by stores depending on the store and country. To learn more about how RevenueCat estimates taxes, [click here](https://www.revenuecat.com/docs/dashboard-and-metrics/taxes-and-commissions).                                                                                        | float        | `0.1442`                                                                                                                                                                                                                                     |             |
| `commission_percentage`                | The portion of a transaction's price that will be deducted by the store for commission. In stores where taxes are deducted before commission, this value will not equal the published commission from a store, because that commission is calculated on the post-tax revenue.                                                                                                        | float        | `0.15`                                                                                                                                                                                                                                       |             |
| `store_transaction_id`                 | orderId or transaction\_identifier.                                                                                                                                                                                                                                                                                                                                                   | string       | `123456789012345`                                                                                                                                                                                                                            |             |
| `original_store_transaction_id`        | orderId of first purchase or `original_transaction_id`. Can be used to find all related transactions for a single subscription.                                                                                                                                                                                                                                                      | string       | `011223344556677`                                                                                                                                                                                                                            |             |
| `refunded_at`                          | When a refund was detected, `null` if none was detected. Is not set in the case of upgraded transactions for which the App Store issues a partial refund.                                                                                                                                                                                                                            | datetime     | `2023-02-20 05:47:55`                                                                                                                                                                                                                        | ✅          |
| `unsubscribe_detected_at`              | When we detected an unsubscribe (opt-out of auto renew).                                                                                                                                                                                                                                                                                                                             | datetime     | `2023-02-16 14:17:10`                                                                                                                                                                                                                        | ✅          |
| `billing_issues_detected_at`           | When we detected billing issues, `null` if none was detected.                                                                                                                                                                                                                                                                                                                        | datetime     | `2023-02-01 08:27:15`                                                                                                                                                                                                                        | ✅          |
| `purchased_currency`                   | The currency that was used for the transaction.                                                                                                                                                                                                                                                                                                                                      | string       | `GBP`                                                                                                                                                                                                                                        | ✅          |
| `price_in_purchased_currency`          | The revenue (in the purchased currency) generated from the transaction after accounting for full and partial refunds. Can be null if product prices haven't been collected from the user's device.                                                                                                                                                                                   | float        | `0`                                                                                                                                                                                                                                          | ✅          |
| `purchase_price_in_purchased_currency` | The gross revenue (in the purchased currency) generated from the transaction. Remains set for refunded transactions. Can be null if product prices haven't been collected from the user's device.                                                                                                                                                                                    | float        | `3.99`                                                                                                                                                                                                                                       | ✅          |
| `entitlement_identifiers`              | An array of entitlements that the transaction unlocked or `null` if it didn't unlock any entitlements.                                                                                                                                                                                                                                                                               | string array | `["membership", "full_access"]`                                                                                                                                                                                                              | ✅          |
| `renewal_number`                       | Always starts at 1. Trial conversions are counted as renewals. `is_trial_conversion` is used to signify whether a transaction was a trial conversion.                                                                                                                                                                                                                                | integer      | `2`                                                                                                                                                                                                                                          |             |
| `is_trial_conversion`                  | If `true`, this transaction is a trial conversion.                                                                                                                                                                                                                                                                                                                                   | boolean      | `true`                                                                                                                                                                                                                                       |             |
| `presented_offering`                   | The offering presented to users.                                                                                                                                                                                                                                                                                                                                                     | string       | `Default Offering`                                                                                                                                                                                                                           | ✅          |
| `ownership_type`                       | Will be `PURCHASED` when a recorded transaction results from the subscriber’s direct purchase of it, or `FAMILY_SHARED` when a recorded transaction results from the subscriber having received it through Family Sharing. <br> <br>NOTE: The `FAMILY_SHARED` designation is only supported on App Store transactions.                                                           | string       | `PURCHASED`                                                                                                                                                                                                                                  | ✅          |
| `reserved_subscriber_attributes`       | The [reserved attributes](https://www.revenuecat.com/docs/customers/customer-attributes#reserved-attributes) set for the Customer (subscriber). Keys begin with `$`.                                                                                                                                                                                                                                                | string JSON  | `{"$ip": {"value": "203.78.120.117", "updated_at_ms": 1672549200}, "$gpsAdId": {"value": "80480bdc-06e0-11ee-be56-0242ac120002", "updated_at_ms": 1672549200}, "$androidId": {"value": "12345a9876b4c123", "updated_at_ms": 1673097132390}}` | ✅          |
| `custom_subscriber_attributes`         | The custom attributes set for the Customer (subscriber).                                                                                                                                                                                                                                                                                                                             | string JSON  | `{"feature_setting": {"value": "1", "updated_at_ms": 1672549200}, "survey_response": {"value": "2", "updated_at_ms": 1599112814785}}`                                                                                                        | ✅          |
| `platform`                             | Last seen platform of the subscriber.                                                                                                                                                                                                                                                                                                                                                | string       | `android`                                                                                                                                                                                                                                    | ✅          |
| `updated_at`                           | The last time an attribute of the transaction was modified.                                                                                                                                                                                                                                                                                                                          | datetime     | `2023-02-20 05:47:55`                                                                                                                                                                                                                        |             |
| `offer`                                | The offer that was used for a transaction (if applicable).                                                                                                                                                                                                                                                                                                                           | string       | `black_friday_discount`                                                                                                                                                                                                                      | ✅          |
| `offer_type`                           | The type of offer that was used for a transaction (if applicable).                                                                                                                                                                                                                                                                                                                   | string       | `offer_code`                                                                                                                                                                                                                                 | ✅          |
| `first_seen_time`                      | The time the customer was first seen by RevenueCat.                                                                                                                                                                                                                                                                                                                                  | datetime     | `2023-01-01 03:00:00`                                                                                                                                                                                                                        |             |
| `auto_resume_time`                     | The time when a Play Store subscription would resume after being paused.                                                                                                                                                                                                                                                                                                             | datetime     | `2023-03-20 03:00:00`                                                                                                                                                                                                                        | ✅          |
| `enrolled_experiments_by_layer`        | Object containing experiment enrollment information for the subscriber, keyed by layer. Replaces the deprecated `experiment_id` and `experiment_variant` columns.                                                                                                                                                                                                                    | string JSON  | `{"layer_name": [{"experiment_id": "prexpaaaaaaaaaa", "variant": "a", "enrolled_at": "1970-01-01 00:00:00"}]}`                                                                                                                               | ✅          |

### Virtual Currency

Per-row virtual currency ledger data. Delivered to `<base>/virtual_currency/<date>/virtual_currency_<timestamp>.<ext>`. This is a full-snapshot feed: each run re-exports the entire ledger and it doesn't support incremental delivery. To set up virtual currencies in your project before exporting them, see [Virtual Currency](https://www.revenuecat.com/docs/offerings/virtual-currency).

The Virtual Currency feed exports one row per balance adjustment in your virtual currency ledger. Each row records a credit or debit, the running balance after that adjustment, and — when the adjustment originated from a store purchase — the associated purchase fields.

Purchase-related columns (`store`, `is_sandbox`, `store_transaction_id`, `product_identifier`, and price fields) are `null` on non-purchase rows. Refund columns (`refund_amount_usd`, `refund_amount_in_purchased_currency`, `refund_type`) are populated only on `REFUND_REMOVAL` rows with a positive refund delta.

:::info
All dates and times are provided in UTC.
:::

| Header                                 | Description                                                                                                                                         | Type     | Example value                                     | Can be null |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------- | ----------- |
| `rc_original_app_user_id`              | The original app user ID for the subscriber. Can be used as a unique user identifier to find all of a user's virtual currency events.               | string   | `$RCAnonymousID:87c6049c58069238dce29853916d624c` | ✅          |
| `rc_last_seen_app_user_id_alias`       | The last seen alias for the app user ID. Can be used together with `rc_original_app_user_id` to match events with user identifiers in your systems. | string   | `$RCAnonymousID:87c6049c58069238dce29853916d624c` | ✅          |
| `transaction_type`                     | Virtual currency event type. One of `PURCHASE`, `DEDUCTION`, `GRANT`, `REFUND_REMOVAL`, `expiration`, or `other`.                                   | string   | `PURCHASE`                                        | ✅          |
| `currency_code`                        | Code of the virtual currency.                                                                                                                       | string   | `coins`                                           | ✅          |
| `adjustment_amount`                    | Signed amount of this balance adjustment. Positive values are credits; negative values are debits.                                                  | integer  | `100`                                             | ✅          |
| `updated_balance`                      | Running balance for this balance ID after this adjustment, floored at 0.                                                                            | integer  | `250`                                             | ✅          |
| `event_timestamp`                      | When the underlying virtual currency transaction occurred.                                                                                          | datetime | `2023-01-01 08:27:06`                             | ✅          |
| `updated_at`                           | When this ledger row was last updated in RevenueCat.                                                                                                | datetime | `2023-02-20 05:47:55`                             | ✅          |
| `idempotency_key`                      | Idempotency key of the underlying virtual currency transaction. Use this to deduplicate rows if you ingest the same delivery more than once.        | string   | `vc_tx_abc123`                                    | ✅          |
| `store`                                | Store the originating purchase came from (`app_store`, `play_store`, etc.). `null` for non-purchase rows.                                           | string   | `app_store`                                       | ✅          |
| `is_sandbox`                           | Whether the originating purchase was made in a [sandbox environment](https://www.revenuecat.com/docs/test-and-launch/sandbox). `null` for non-purchase rows.                       | boolean  | `false`                                           | ✅          |
| `store_transaction_id`                 | Identifier the store assigned to the originating purchase.                                                                                          | string   | `123456789012345`                                 | ✅          |
| `product_identifier`                   | Identifier of the product purchased.                                                                                                                | string   | `coins_100_pack`                                  | ✅          |
| `price_in_usd`                         | Price of the purchase in USD, after any refunds.                                                                                                    | float    | `4.99`                                            | ✅          |
| `purchase_price_in_usd`                | Catalog price of the purchase in USD, before discounts and refunds.                                                                                 | float    | `4.99`                                            | ✅          |
| `purchased_currency`                   | Currency the purchase was made in.                                                                                                                  | string   | `USD`                                             | ✅          |
| `price_in_purchased_currency`          | Price of the purchase in its purchased currency, after any refunds.                                                                                 | float    | `4.99`                                            | ✅          |
| `purchase_price_in_purchased_currency` | Catalog price of the purchase in its purchased currency, before discounts and refunds.                                                              | float    | `4.99`                                            | ✅          |
| `refund_amount_usd`                    | Refunded amount in USD. Populated only on `REFUND_REMOVAL` rows with a positive refund delta.                                                       | float    | `4.99`                                            | ✅          |
| `refund_amount_in_purchased_currency`  | Refunded amount in the purchased currency. Populated only on `REFUND_REMOVAL` rows with a positive refund delta.                                    | float    | `4.99`                                            | ✅          |
| `refund_type`                          | `FULL` or `PARTIAL`. Populated only on `REFUND_REMOVAL` rows with a positive refund delta.                                                          | string   | `FULL`                                            | ✅          |

## A note on transaction data

The following guidance applies to the **Transactions** feed.

All transaction data is based on the store receipts that RevenueCat has received. Receipts often have inconsistencies and quirks that you may need to account for. For example:

- The expiration date of a purchase can be before the purchase date. This is Google's way of invalidating a transaction, for example when Google is unable to bill a user some time after a subscription renews. This doesn't occur on iOS.
- If you migrated to RevenueCat, Google subscriptions that were expired for more than 60 days before being migrated will not have transaction histories in export files.
- Apple and Google do not always provide the transaction price directly, so we rely on historical data and store APIs. This may result in inaccuracies if receipts were imported, or if a product price was increased before your [App Store Connect API Key](https://www.revenuecat.com/docs/subscription-guidance/price-changes#price-detection) was added.
- Renewal numbers start at 1, even for trials. Trial conversions increase the renewal number.
- Data is pulled from a snapshot of the current receipt state. This means the same transaction can differ from one delivery to another if something changed (for example, due to a refund or billing issue). Recompute metrics for past time periods periodically to account for these changes, and use the `updated_at` field to detect whether a transaction changed since a prior export.
- Data is current as of when the export begins generating. Changes that occur between the start of generation and delivery aren't reflected in that export.

We try to normalize or at least annotate these quirks as much as possible, but by and large we consider receipts the source of truth, so any inconsistencies in the transaction data can always be traced back to the receipt.

## Handling updated transactions correctly

These recommendations apply to the **Transactions** feed.

We strongly recommend keeping **Receive new and updated transactions only** enabled to significantly reduce the amount of data you need to process in each daily export.

![The Receive new and updated transactions only option in the integration settings](https://www.revenuecat.com/docs_images/integrations/scheduled-data-exports/new-and-updated.png)

However, handling transaction updates can be tricky, so consider these tips to make it easier:

1. For most stores, `store_transaction_id` will be unique for each transaction, but for Stripe it is not; so for best results we recommend treating every unique set of \[`store_transaction_id` + `renewal_number`] as a unique transaction.
2. Instead of overwriting prior transaction states when receiving an updated transaction, consider adding them as new rows to your output table and setting a property like `is_latest` to ensure you're never double-counting different versions of the same transaction. Or, you could set an `ingested_time` property to order the transactions by the most recent version you received from RevenueCat.
3. When in doubt, use `updated_at` (provided by RevenueCat in your export) as a reference point to determine the latest version of a transaction if you have multiple prior versions and can't otherwise confidently determine which one is latest.

## Sample queries for RevenueCat measures

The following sample queries use the **Transactions** feed. They are written in PostgreSQL and serve as starting points for reproducing common RevenueCat measures.

**Active Subscriptions**

```pgsql
-- Active Subscriptions as of a specified date

SELECT
  COUNT(*)
FROM
  [revenuecat_data_table]
WHERE date(effective_end_time) > [targeted_date]
  AND date(start_time) <= [targeted_date]
  AND is_trial_period = 'false'
  AND DATE_DIFF('s', start_time, end_time)::float > 0
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'

-- The RevenueCat Active Subscriptions chart excludes trials,
-- promotional transactions, and transactions resulting from family sharing
-- since they do not reflect auto-renewing future payments.
```

**Active Subscriptions Movement**

```pgsql
-- Active Subscriptions Movement within a specified date range

WITH

filtered_subscription_transactions AS (
    SELECT
        *
    FROM [revenuecat_data_table]
    /* Filter down to the date range that you want to measure MRR Movement for */
    WHERE (start_time BETWEEN [targeted_start_date] and [targeted_end_date] 
        OR effective_end_time BETWEEN [targeted_start_date] and [targeted_end_date])
        /* Exclude trials, which do not contribute to MRR */
        AND is_trial_period = 'false'
        AND DATE_DIFF('s', start_time, end_time)::float > 0
        AND ownership_type != 'FAMILY_SHARED'
        AND store != 'promotional'
        AND is_sandbox != 'true'),

actives AS (
  SELECT
    DATE(start_time) AS date,
    COUNT(
        CASE 
            WHEN renewal_number = 1
                OR is_trial_conversion = 'true'
            THEN 1 
            ELSE NULL 
        END) AS num_new_actives,
    COUNT(
        CASE 
            WHEN renewal_number > 1
                AND is_trial_conversion = 'false'  
            THEN 1 
            ELSE NULL 
        END
    ) AS num_renewals
    
  FROM filtered_subscriptipon_transactions
  GROUP BY 1),
  
expirations AS (
  SELECT
    DATE(effective_end_time) AS date,
    COUNT(*) AS num_expirations
  FROM filtered_subscriptipon_transactions
  GROUP BY 1)

SELECT
    COALESCE(a.date, e.date) AS date,
    COALESCE(a.num_new_actives, 0) AS new_actives, /* "New Actives" in the Active Subscriptions Movement Chart */
    COALESCE(a.num_renewals, 0) AS num_renewals,
    COALESCE(e.num_expirations, 0) AS num_expirations,
    num_expirations - num_renewals AS churned_actives, /* "Churned Actives" in the Active Subscriptions Movement Chart */
FROM actives a
FULL JOIN expirations e ON a.date = e.date
WHERE a.date BETWEEN [targeted_start_date] AND [targeted_end_date]
    AND e.date BETWEEN [targeted_start_date] AND [targeted_end_date]
```

**MRR**

```pgsql
-- MRR as of a specified date

SELECT
    SUM(
        CASE WHEN effective_end_time IS NOT NULL THEN
            CASE 
                /* Handle cases where product_duration cannot be used for the transaction first */
                WHEN (is_in_intro_offer_period = 'true' OR product_duration IS NULL) THEN 
                CASE
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 0 AND 1 
                        THEN (30 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) = 3 
                        THEN (10 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 6 AND 8 
                        THEN (4 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 12 AND 16 
                        THEN (2 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 27 AND 33 
                        THEN (1 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 58 AND 62 
                        THEN (0.5 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 88 AND 95 
                        THEN (0.333333 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 179 AND 185 
                        THEN (0.1666666 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 363 AND 375 
                        THEN (0.08333 * price)::DECIMAL(18,2)
                    ELSE ((28 / (DATE_DIFF('s', start_time, end_time)::float / (24 * 3600))) * price)::DECIMAL(18,2)
                END
                /* Then handle cases where product_duration can be used */
                WHEN product_duration = 'P1D' 
                    THEN (30 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P3D' 
                    THEN (10 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P7D' 
                    THEN (4 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P1W' 
                    THEN (4 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P2W' 
                    THEN (2 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P4W' 
                    THEN (1 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P1M' 
                    THEN (1 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P2M' 
                    THEN (0.5 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P3M' 
                    THEN (0.333333 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P6M' 
                    THEN (0.1666666 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P12M' 
                    THEN (0.08333 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P1Y' 
                    THEN (0.08333 * price)::DECIMAL(18,2)
                ELSE ((28 / (DATE_DIFF('s', start_time, end_time)::float / (24 * 3600))) * price)::DECIMAL(18,2)
            END
        END 
    ) AS active_mrr
FROM [revenuecat_data_table] 

/* Filter down to the date range that you want to measure MRR for */
WHERE date(effective_end_time) > '2024-02-06'
  AND date(start_time) <= '2024-02-06'
  /* Exclude trials, which do not contribute to MRR */
  AND is_trial_period = 'false'
  AND DATE_DIFF('s', start_time, end_time)::float > 0
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'
```

**MRR Movement**

```pgsql
-- MRR Movement for a specified date range

WITH

filtered_subscription_transactions AS (
    SELECT
        *,
        CASE WHEN effective_end_time IS NOT NULL THEN
            CASE 
                /* Handle cases where product_duration cannot be used for the transaction first */
                WHEN (is_in_intro_offer_period = 'true' OR product_duration IS NULL) THEN 
                CASE
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 0 AND 1 
                        THEN (30 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) = 3 
                        THEN (10 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 6 AND 8 
                        THEN (4 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 12 AND 16 
                        THEN (2 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 27 AND 33 
                        THEN (1 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 58 AND 62 
                        THEN (0.5 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 88 AND 95 
                        THEN (0.333333 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 179 AND 185 
                        THEN (0.1666666 * price)::DECIMAL(18,2)
                    WHEN DATE_DIFF(day, start_time, end_time) BETWEEN 363 AND 375 
                        THEN (0.08333 * price)::DECIMAL(18,2)
                    ELSE ((28 / (DATE_DIFF('s', start_time, end_time)::float / (24 * 3600))) * price)::DECIMAL(18,2)
                END
                /* Then handle cases where product_duration can be used */
                WHEN product_duration = 'P1D' THEN (30 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P3D' THEN (10 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P7D' THEN (4 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P1W' THEN (4 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P2W' THEN (2 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P4W' THEN (1 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P1M' THEN (1 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P2M' THEN (0.5 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P3M' THEN (0.333333 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P6M' THEN (0.1666666 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P12M' THEN (0.08333 * price)::DECIMAL(18,2)
                WHEN product_duration = 'P1Y' THEN (0.08333 * price)::DECIMAL(18,2)
                ELSE ((28 / (DATE_DIFF('s', start_time, end_time)::float / (24 * 3600))) * price)::DECIMAL(18,2)
            END
        END AS transaction_mrr
    FROM [revenuecat_data_table]
    /* Filter down to the date range that you want to measure MRR Movement for */
    WHERE (start_time BETWEEN [targeted_start_date] and [targeted_end_date] 
        OR effective_end_time BETWEEN [targeted_start_date] and [targeted_end_date])
        /* Exclude trials, which do not contribute to MRR */
        AND is_trial_period = 'false'
        AND DATE_DIFF('s', start_time, end_time)::float > 0
        AND ownership_type != 'FAMILY_SHARED'
        AND store != 'promotional'
        AND is_sandbox != 'true'),

actives AS (
  SELECT
    DATE(start_time) AS date,
    SUM(
        CASE
            WHEN renewal_number = 1
                OR is_trial_conversion = 'true' 
            THEN transaction_mrr
            ELSE null
        END
    ) AS new_mrr,
    
    SUM(
        CASE
            WHEN renewal_number > 1 
                AND is_trial_conversion = 'false' 
            THEN transaction_mrr
            ELSE null
        END
    ) AS renewal_mrr
    
  FROM filtered_subscription_transactions
  GROUP BY 1),
  
expirations AS (
  SELECT
    DATE(effective_end_time) AS date,
    SUM(transaction_mrr) AS expired_mrr
  FROM filtered_subscription_transactions
  GROUP BY 1)

SELECT
    COALESCE(a.date, e.date) AS date,
    COALESCE(a.new_mrr, 0) AS new_mrr, /* "New MRR" in the MRR Movement Chart */
    COALESCE(a.renewal_mrr, 0) as renewal_mrr,
    COALESCE(e.expired_mrr, 0) as expired_mrr,
    expired_mrr - renewal_mrr as churned_mrr /* "Churned MRR" in the MRR Movement Chart */
FROM actives a
FULL JOIN expirations e ON a.date = e.date
WHERE a.date BETWEEN [targeted_start_date] AND [targeted_end_date]
    AND e.date BETWEEN [targeted_start_date] AND [targeted_end_date]
```

**Revenue**

```pgsql
-- Revenue generated on a specified date

SELECT
  SUM(purchase_price_in_usd) as gross_revenue,
  SUM(price_in_usd) as revenue_net_of_refunds, /* "Revenue" in the Revenue Chart */
  SUM(price_in_usd * (1 - tax_percentage)) as revenue_net_of_taxes, /* "Revenue (net of taxes)" in the Revenue Chart */
  SUM(price_in_usd * (1 - tax_percentage - commission_percentage)) as proceeds /* "Proceeds" in the Revenue Chart */
FROM
  [revenuecat_data_table]
WHERE date(start_time) = [targeted_date]
  AND is_trial_period = 'false'
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'

-- Transactions which have been refunded can be identified through the refunded_at field.
```

**Subscription Retention**

```pgsql
-- Subscription Retention per product

WITH filtered_transactions AS (
    SELECT
        *
    FROM [revenuecat_data_table] rc
    WHERE end_time IS NOT NULL /* only include subscriptions */
        AND NOT (billing_issues_detected_at IS NOT NULL
            AND (store = 'play_store' OR store = 'stripe'))
        AND is_sandbox <> 'true'
        AND is_trial_period = 'false'
        AND is_in_intro_offer_period = 'false' /* exclude introductory offers */
        AND ownership_type = 'PURCHASED'
),
  
subs AS (
    SELECT
        ft.rc_original_app_user_id,
        product_identifier,
        product_duration,
        DATE(MIN(ft.start_time)) AS first_start_time, /* first start time is used to define each cohort */
        MAX(DATE_DIFF('day', start_time, end_time)) as max_transaction_duration
    FROM filtered_transactions ft
    GROUP BY 1, 2, 3
),

/* some products don't have a set duration, so this CTE will calculate the duration on a per subscription basis */
calculated_product_duration AS (
    SELECT
        rc_original_app_user_id,
        product_identifier,
        CASE
            WHEN product_duration IS NOT NULL THEN 
            CASE
                WHEN product_duration = 'P7D' THEN 'P1W'
                WHEN product_duration = 'P30D' THEN 'P1M'
                WHEN product_duration = 'P4W' THEN 'P1M'
                WHEN product_duration = 'P12M' THEN 'P1Y'
                WHEN product_duration = 'P200Y' THEN 'lifetime'
                WHEN product_duration = 'P999Y' THEN 'lifetime'
            ELSE product_duration
            END
        ELSE 
            CASE
                WHEN max_transaction_duration BETWEEN 0 AND 1 THEN 'P1D'
                WHEN max_transaction_duration = 3 THEN 'P3D'
                WHEN max_transaction_duration BETWEEN 6 AND 8 THEN 'P1W'
                WHEN max_transaction_duration BETWEEN 12 AND 16 THEN 'P2W'
                WHEN max_transaction_duration BETWEEN 27 AND 37 THEN 'P1M'
                WHEN max_transaction_duration BETWEEN 58 AND 62 THEN 'P2M'
                WHEN max_transaction_duration BETWEEN 88 AND 95 THEN 'P3M'
                WHEN max_transaction_duration BETWEEN 179 AND 185 THEN 'P6M'
                WHEN max_transaction_duration BETWEEN 363 AND 375 THEN 'P1Y'
            ELSE NULL
            END
        END AS calculated_product_duration
    FROM subs s
    GROUP BY 1, 2, 3
),
  
retention AS (
    SELECT
        subs.first_start_time,
        subs.product_identifier,
        cpd.calculated_product_duration,
        /* Each period number represents the number of billing cycles the subscriber was active for */
        CASE
            WHEN calculated_product_duration = 'P1D' THEN DATE_DIFF('day', subs.first_start_time, start_time)
            WHEN calculated_product_duration = 'P1W' THEN DATE_DIFF('week', subs.first_start_time, start_time)
            WHEN calculated_product_duration = 'P1M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(30 AS NUMERIC)) AS INTEGER)
            WHEN calculated_product_duration = 'P2M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(60 AS NUMERIC)) AS INTEGER)
            WHEN calculated_product_duration = 'P3M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(90 AS NUMERIC)) AS INTEGER)
            WHEN calculated_product_duration = 'P6M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(180 AS NUMERIC)) AS INTEGER)
            WHEN calculated_product_duration = 'P1Y' THEN CAST(ROUND(DATE_DIFF('month', subs.first_start_time, start_time) / CAST(12 AS NUMERIC)) AS INTEGER)
        END AS period_number,
        count(1) AS subscriptions
    FROM filtered_transactions ft
    INNER JOIN subs ON 
        subs.rc_original_app_user_id = ft.rc_original_app_user_id AND
        subs.product_identifier = ft.product_identifier
    INNER JOIN calculated_product_duration cpd ON 
        cpd.rc_original_app_user_id = ft.rc_original_app_user_id AND
        cpd.product_identifier = ft.product_identifier
    WHERE period_number IS NOT NULL
    GROUP BY 1, 2, 3, 4
),
  
pending_retention AS (
    SELECT
        subs.first_start_time,
        subs.product_identifier,
        cpd.calculated_product_duration,
        CASE
            WHEN calculated_product_duration = 'P1D' THEN DATE_DIFF('day', subs.first_start_time, start_time) + 1
            WHEN calculated_product_duration = 'P1W' THEN DATE_DIFF('week', subs.first_start_time, start_time) + 1
            WHEN calculated_product_duration = 'P1M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(30 AS NUMERIC)) AS INTEGER) + 1
            WHEN calculated_product_duration = 'P2M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(60 AS NUMERIC)) AS INTEGER) + 1
            WHEN calculated_product_duration = 'P3M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(90 AS NUMERIC)) AS INTEGER) + 1
            WHEN calculated_product_duration = 'P6M' THEN CAST(ROUND(DATE_DIFF('day', subs.first_start_time, start_time) / CAST(180 AS NUMERIC)) AS INTEGER) + 1
            WHEN calculated_product_duration = 'P1Y' THEN CAST(ROUND(DATE_DIFF('month', subs.first_start_time, start_time) / CAST(12 AS NUMERIC)) AS INTEGER) + 1
        END AS period_number,
        count(1) AS subscriptions
    FROM filtered_transactions ft
    INNER JOIN subs ON 
        subs.rc_original_app_user_id = ft.rc_original_app_user_id AND
        subs.product_identifier = ft.product_identifier
    INNER JOIN calculated_product_duration cpd ON 
        cpd.rc_original_app_user_id = ft.rc_original_app_user_id AND
        cpd.product_identifier = ft.product_identifier
    WHERE unsubscribe_detected_at IS NULL /* count only subscriptions that are set to renew */
        AND
            ((calculated_product_duration = 'P1D' AND DATE_ADD(start_time, '1 day') > CURRENT_DATE)
            OR (calculated_product_duration = 'P1W' AND DATE_ADD(start_time, '1 week') > CURRENT_DATE)
            OR (calculated_product_duration = 'P1M' AND DATE_ADD(start_time, '1 month') > CURRENT_DATE)
            OR (calculated_product_duration = 'P2M' AND DATE_ADD(start_time, '2 months') > CURRENT_DATE)
            OR (calculated_product_duration = 'P3M' AND DATE_ADD(start_time, '3 months') > CURRENT_DATE)
            OR (calculated_product_duration = 'P6M' AND DATE_ADD(start_time, '6 months') > CURRENT_DATE)
            OR (calculated_product_duration = 'P1Y' AND DATE_ADD(start_time, '1 year') > CURRENT_DATE))
        AND period_number IS NOT NULL
    GROUP BY 1, 2, 3, 4
)

SELECT
    COALESCE(retention.first_start_time, pending_retention.first_start_time) AS first_start_date,
    COALESCE(retention.calculated_product_duration, pending_retention.calculated_product_duration) AS product_duration,
    COALESCE(retention.product_identifier, pending_retention.product_identifier) AS product_identifier,
    COALESCE(retention.period_number, pending_retention.period_number) AS period_number,
    CASE
        WHEN retention.period_number = 0 THEN 'Subscriptions'
        WHEN retention.calculated_product_duration = 'P1Y' THEN CONCAT('Year ', retention.period_number)
        WHEN retention.calculated_product_duration = 'P6M' THEN CONCAT('Month ', 6 * retention.period_number)
        WHEN retention.calculated_product_duration = 'P3M' THEN CONCAT('Month ', 3 * retention.period_number)
        WHEN retention.calculated_product_duration = 'P2M' THEN CONCAT('Month ', 2 * retention.period_number)
        WHEN retention.calculated_product_duration = 'P1M' THEN CONCAT('Month ', retention.period_number)
        WHEN retention.calculated_product_duration = 'P1W' THEN CONCAT('Week ', retention.period_number)
        WHEN retention.calculated_product_duration = 'P1D' THEN CONCAT('Day ', retention.period_number)
    ELSE CONCAT('Period ', retention.period_number)
    END AS period_name,
    COALESCE(retention.subscriptions, 0) + COALESCE(pending_retention.subscriptions, 0) AS subscriptions
FROM retention
FULL OUTER JOIN pending_retention ON
    pending_retention.first_start_time = retention.first_start_time AND
    pending_retention.calculated_product_duration = retention.calculated_product_duration AND
    pending_retention.product_identifier = retention.product_identifier AND
    pending_retention.period_number = retention.period_number
WHERE first_start_date >= /* desired date range */
ORDER BY product_identifier, first_start_date, period_number
```

## Sample queries for customized measures

These examples also use the **Transactions** feed. Scheduled Data Exports are a powerful way to add your own customizations on top of the core measures provided by RevenueCat. Check out the following sample queries (written in PostgreSQL) for some ideas.

**Active Subs by Custom Attribute**

```pgsql
-- How many Active Subscriptions do I have with a given custom attribute value?
  
SELECT
  COUNT(*)
FROM
  [revenuecat_data_table] rc
  
WHERE date(effective_end_time) > [targeted_date]
  AND date(start_time) <= [targeted_date]
  AND is_trial_period = 'false'
  AND DATE_DIFF('s', start_time, end_time)::float > 0
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'
  AND json_extract_path_text(custom_subscriber_attributes, '[custom_attribute_key].value') = [custom_attribute_value]
```

**Active Subs by Auto Renew Status**

```pgsql
-- What is my split of Active Subs by auto renew status?
  
SELECT
  CASE 
    WHEN unsubscribe_detected_at IS NOT NULL THEN 'Set to cancel' 
    ELSE 'Set to renew' 
  END as auto_renew_status,
  COUNT(*) as active_subscriptions
FROM
  [revenuecat_data_table]
  
WHERE date(effective_end_time) > [targeted_date]
  AND date(start_time) <= [targeted_date]
  AND is_trial_period = 'false'
  AND DATE_DIFF('s', start_time, end_time)::float > 0
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'
  GROUP BY 1
```

**Weekly Revenue (starting Monday)**

```pgsql
-- What is my weekly revenue, where Monday is set as the start day of the week?

SELECT
  date_trunc('week', start_time) as week,
  SUM(price_in_usd) as total_revenue
FROM
  [revenuecat_data_table]
WHERE date(start_time) BETWEEN [targeted_period_start_date] AND [targeted_period_end_date]
  AND is_trial_period = 'false'
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'
GROUP BY week
```

**Realized LTV Segments**

```pgsql
-- What is my Realized LTV of each monthly subscription cohort, segmented by whether they were offered a trial?
  
WITH 
(SELECT
  MIN(start_time) as subscription_start_time,
  original_store_transaction_id,
  MAX(is_trial_period) as had_a_trial,
  SUM(price_in_usd) as realized_ltv
FROM
  [revenuecat_data_table]
WHERE date(start_time) > [targeted_period_start_date]
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'
  GROUP BY original_store_transaction_id) as subscriptions
  
SELECT
  date_trunc('month', subscription_start_time) as subscription_start_month,
  had_a_trial,
  COUNT(*) as subscriptions,
  SUM(realized_ltv) as realized_ltv,
  SUM(realized_ltv) / COUNT(*) as realized_ltv_per_subscription
FROM
  subscriptions
```

**Active Trials by Grace Period Status**

```pgsql
-- What portion of my Active Trials are in a grace period?
  
SELECT
  CASE
    WHEN grace_period_end_time IS NOT NULL THEN 'in_grace_period'
    ELSE 'in_trial_period'
    END as period_type,
  COUNT(*) as active_trials
FROM
  [revenuecat_data_table]
WHERE date(effective_end_time) > [targeted_date]
  AND date(start_time) <= [targeted_date]
  AND is_trial_period = 'true'
  AND DATE_DIFF('s', start_time, effective_end_time)::float > 0
  AND ownership_type != 'FAMILY_SHARED'
  AND store != 'promotional'
  AND is_sandbox != 'true'
GROUP BY period_type
```

**Realized LTV Per Paying Customer by First Purchase Date**

```pgsql
-- What is my Realized LTV per Paying Customer cohorted by First Purchase Date?
  
WITH filtered_transactions AS
  (SELECT *
  FROM [revenuecat_data_table]
  WHERE is_trial_period = 'false'
    AND ownership_type != 'FAMILY_SHARED'
    AND store != 'promotional'
    AND is_sandbox != 'true'
    AND refunded_at IS NULL
    AND price > 0),

first_purchase_dates AS
  (SELECT
    rc_original_app_user_id,
    MIN(start_time) as first_purchase_date
  FROM filtered_transactions
  GROUP BY 1)

SELECT

  DATE(fpd.first_purchase_date) AS first_purchase_date,
  COUNT(DISTINCT rc_original_app_user_id) AS paying_customers,
  SUM(CASE WHEN DATEADD(day, 7, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)::DECIMAL(18,2) AS total_ltv_7_days,
  SUM(CASE WHEN DATEADD(day, 30, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)::DECIMAL(18,2) AS total_ltv_30_days,
  SUM(CASE WHEN DATEADD(month, 6, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)::DECIMAL(18,2) AS total_ltv_6_months,
  SUM(CASE WHEN DATEADD(month, 12, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)::DECIMAL(18,2) AS total_ltv_12_months,
  SUM(CASE WHEN DATEADD(month, 24, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)::DECIMAL(18,2) AS total_ltv_24_months,
  SUM(price_in_usd)::DECIMAL(18,2) AS total_ltv_unbounded,

  (SUM(CASE WHEN DATEADD(day, 7, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)/COUNT(DISTINCT rc_original_app_user_id))::DECIMAL(18,2) AS avg_ltv_7_days,
  (SUM(CASE WHEN DATEADD(day, 30, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)/COUNT(DISTINCT rc_original_app_user_id))::DECIMAL(18,2) AS avg_ltv_30_days,
  (SUM(CASE WHEN DATEADD(month, 6, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)/COUNT(DISTINCT rc_original_app_user_id))::DECIMAL(18,2) AS avg_ltv_6_months,
  (SUM(CASE WHEN DATEADD(month, 12, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)/COUNT(DISTINCT rc_original_app_user_id))::DECIMAL(18,2) AS avg_ltv_12_months,
  (SUM(CASE WHEN DATEADD(month, 23, first_purchase_date) > start_time 
    THEN price_in_usd ELSE 0 END)/COUNT(DISTINCT rc_original_app_user_id))::DECIMAL(18,2) AS avg_ltv_24_months,
  (SUM(price_in_usd)/COUNT(DISTINCT rc_original_app_user_id))::DECIMAL(18,2) AS avg_ltv_unbounded

FROM filtered_transactions ft
LEFT JOIN first_purchase_dates fpd 
  ON fpd.rc_original_app_user_id = ft.rc_original_app_user_id
GROUP BY 1
```

**Trial Conversion Rate by Trial End Date**

```pgsql
-- What was the conversion-to-paid of all trials that ended each day?

WITH trials AS
(SELECT
  rc_original_app_user_id,
  DATE(effective_end_time) AS end_time
  FROM [revenuecat_data_table] 
  WHERE is_trial_period
),

conversions AS
(SELECT
  rc_original_app_user_id,
  DATE(start_time) AS start_time
  FROM [revenuecat_data_table] 
  WHERE is_trial_conversion
)

SELECT
  DATE(t.end_time) AS date,
  COUNT(t.*) AS trials_ending,
  COUNT(c.*) AS conversions,
  (COUNT(c.*)::real / COUNT(t.*)::real) AS cvr
  FROM trials AS t
  LEFT JOIN conversions AS c ON c.rc_original_app_user_id=t.rc_original_app_user_id
  WHERE t.end_time < CURRENT_DATE
  GROUP BY date;
```
