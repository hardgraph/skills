# Reconcile your Stripe balance with Revenue Recognition

Use this guide to reconcile cash and fees from Revenue Recognition with the Balance summary report.

Use this guide to reconcile cash and fees from [Revenue Recognition](https://docs.stripe.com/revenue-recognition.md) with the [Balance summary report](https://docs.stripe.com/reports/balance.md).

## Important considerations

When reconciling Revenue Recognition reports with the Balance summary report, be aware of the following:

### Timestamp differences between reports

[Revenue Recognition](https://docs.stripe.com/revenue-recognition.md) and the [Balance summary report](https://docs.stripe.com/reports/balance.md) use different timestamps for payments. Long payment processing times can cause timing mismatches during reconciliation between the two reports. When investigating discrepancies, check the payment’s **settled** and **available** timestamps.

### Open invoice revenue

Unpaid invoices that remain in [open status](https://docs.stripe.com/invoicing/overview.md#invoice-statuses), without changing to `void` or `uncollectible`, continue to be recognized as revenue in the Revenue Recognition report. Because this revenue wasn’t actually collected, the Balance summary report doesn’t include it, creating a discrepancy between the two reports. To ensure accurate reporting, we recommend marking applicable invoices as `uncollectible` or `void`. Only leave an invoice in `open` status if you expect it to be paid.

### Corrections tracked separately

The [debits and credits report](https://docs.stripe.com/revenue-recognition/reports/debits-and-credits.md) treats corrections as separate transactions, and the [Balance Sheet](https://docs.stripe.com/revenue-recognition/reports/balance-sheet.md) doesn’t include them.

If corrections exist, they are logged to the period in which they were made — so you must also download the debits and credits report (under the **Statements** tab) for that period using the **Entries** filter set to **Corrections** and include those entries in your reconciliation. You can read about this at [Review corrections](https://docs.stripe.com/revenue-recognition/reports/debits-and-credits.md#review-corrections).

Widen the date range of your corrections export to make sure all relevant corrections are captured for your reconciliation process.

To reconcile corrections against the Balance Sheet, download the debits and credits report with the **Entries** filter set to **Corrections**. Make sure the **Group Report By** filter matches across both exports, and that the following columns are included at a minimum:

| Debits and credits report | Balance Sheet |
| --- | --- |
| accounting_period | accounting_period |
| currency | currency |
| debit | debit |
| credit | credit |
| amount | amount |

Using these corresponding columns, you can sum the records across both reports to complete your reconciliation, with corrections accounted for.

### Fee exclusion before March 2025

Before March 2025, Revenue Recognition reports excluded fees, network costs, contributions, and financing paydowns. Stripe fees for payments that were processed before this date are also not displayed to avoid showing partial totals. This might cause discrepancies when fees appear in later periods (for example, during disputes). To apply Stripe fees across all accounting periods, [contact support](https://support.stripe.com/).

## Reconcile cash

By reconciling the [Balance summary report](https://docs.stripe.com/reports/balance.md) with the Revenue Recognition [balance sheet](https://docs.stripe.com/revenue-recognition/reports/balance-sheet.md), you can verify that your cash movements are consistent across both reports.

To reconcile, compare the Revenue Recognition balance sheet cash directly to the net total from the Balance summary report:

1. Download the Revenue Recognition [balance sheet](https://dashboard.stripe.com/revenue-recognition/statements?tab=balance_sheet) in summary format to get the cash amount.
2. Download the [Balance summary report](https://dashboard.stripe.com/reports/balance)’s **Balance change from activity** section in summary format. Set the currency to match your Revenue Recognition currency and the timezone to UTC.

For example, if the Revenue Recognition balance sheet shows cash of 50 USD, the Balance summary net total should also be 50 USD.

### Reconcile reports before March 2025 

For balance sheet data before March 2025, the Revenue Recognition balance sheet reflects gross transaction amounts. Stripe fees aren’t yet deducted. To reconcile against the Balance summary report, manually remove fee-related rows before comparing:

1. Download the Revenue Recognition balance sheet in summary format. For example:

| Account | Currency | Net change |
| --- | --- | --- |
| Cash | USD | +100.00 |

1. Download the Balance summary report’s **Balance change from activity** section in summary format. Set the currency to match your Revenue Recognition currency and the timezone to UTC.
2. From the Balance summary total, exclude the following rows before comparing:
   - `fee`
   - `network_cost`
   - `contribution`
   - `financing_paydown`

For example:

| Reporting category | Currency | Gross | Fee | Net |
| --- | --- | --- | --- | --- |
| `charge` | USD | +140.00 | -10.00 | +130.00 |
| `refund` | USD | -40.00 | 0.00 | -40.00 |
| `refund_failure` | USD | +20.00 | 0.00 | +20.00 |
| `partial_capture_reversal` | USD | -20.00 | 0.00 | -20.00 |
| `fee` | USD | -10.00 | 0.00 | -10.00 |
| `network_cost` | USD | -10.00 | 0.00 | -10.00 |
| `contribution` | USD | -10.00 | 0.00 | -10.00 |
| `financing_paydown` | USD | -10.00 | 0.00 | -10.00 |
| `total` | USD | +60.00 | -10.00 | +50.00 |

1. After removing the `fee`, `network_cost`, `contribution`, and `financing_paydown` rows from the gross total, you get:

   +60 − (−10) − (−10) − (−10) − (−10) = +100 USD

   This matches the Revenue Recognition balance sheet cash of 100 USD.

## Reconcile fees

You can reconcile fee-related transactions between the [debits and credits report](https://docs.stripe.com/revenue-recognition/reports/debits-and-credits.md) and the [Balance summary report](https://docs.stripe.com/reports/balance.md).

To reconcile fees:

1. Download the **Balance change from activity** section of the [Balance summary report](https://dashboard.stripe.com/reports/balance) in **summary** format. Set the currency to match your Revenue Recognition currency, the timezone to UTC, and include the following columns when downloading: **Reporting Category**, **Gross**, and **Fee**.
2. Calculate the total fee on the Balance summary report by summing:
   - **Gross column**: `fee` + `network_cost` + `contribution` + `financing_paydown` rows
   - **Fee column**: `total` row

For example:

| Reporting category | Gross | Fee |
| --- | --- | --- |
| `charge` | 100.00 | -4.00 |
| `refund` | -100.00 | 3.00 |
| `platform earning refund` | -0.10 | 0.00 |
| `fee` | **-1000.00** | 0.00 |
| `network_cost` | **-0.50** | 0.00 |
| `contribution` | **-0.40** | 0.00 |
| `financing_paydown` | **-10.00** | 0.00 |
| **total** | -1011.00 | **-1.00** |

Total fees = −1000.00 + −0.50 + −0.40 + −10.00 + −1.00 = −1011.90

1. Download the [debits and credits report](https://dashboard.stripe.com/revenue-recognition/statements?tab=debits_credits) in summary format, and you should see 1011.90 debited from the Fees expense account and credited to Cash:

| Debit | Credit | Amount |
| --- | --- | --- |
| Fees | Cash | 1011.90 |

### Reconcile reports before March 2025 

Revenue Recognition reports didn’t include fees, network costs, contributions, or financing paydowns by default. Unless you [contacted support](https://support.stripe.com/) to apply Stripe fees across all accounting periods, these fees weren’t recorded.

As a result, any fees incurred during this period weren’t deducted from the cash account, leaving cash overstated. Because of this, the [debits and credits report](https://docs.stripe.com/revenue-recognition/reports/debits-and-credits.md) and the [Balance summary report](https://docs.stripe.com/reports/balance.md) can’t be reconciled for any period before March 2025.
