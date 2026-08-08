# Period summary

Learn about the period summary report.

The [period summary](https://dashboard.stripe.com/revenue-recognition/statements?tab=period_summary) provides a detailed breakdown of your billing activity and its impact on revenue within a specific period. Use this report to understand revenue changes and prepare journal entries for your accounting system.
![Period summary for a given month](https://b.stripecdn.com/docs-statics-srv/assets/period-summary.302a1846acbfa96202e620ffdfee0d8d.png)

In this example, the net recognized revenue is 1,154,198.78 USD and the ending deferred revenue balance at the end of October is 589,467.95 USD.

## Recognized revenue 

This section shows all revenue and contra revenue for the period. Revenue items increase your recognized revenue, while contra revenue items reduce it.

| **Item** | **Category** | **Description** |  |
| --- | --- | --- | --- |
| Revenue from billings this month | Revenue | Recognized revenue from finalized invoice line items and standalone payments that occurred during the month. Standalone payment revenue is recognized immediately when the payment occurs. |
| Revenue from metered subscriptions this month | Revenue | Revenue from metered subscription usage. |
| Recognized revenue previously deferred | Revenue | Revenue recognized this month from invoice line items that were finalized in previous months. |
| Revenue from unbilled services | Revenue | Revenue from unbilled invoice items that have been recognized based on their service period. |
| Revenue from platform fees | Revenue | Revenue earned from platform fees. [Learn how platform fees impact revenue](https://docs.stripe.com/revenue-recognition/connect/destination-charges.md#revenue-collected-application-fee-amount). |
| Refunds | Contra revenue | Reduction in previously recognized revenue due to refunds. [Learn how refunds impact revenue](https://docs.stripe.com/revenue-recognition/examples/contra.md#refund). |
| Disputes | Contra revenue | Reduction in previously recognized revenue due to disputes. [Learn how disputes impact revenue](https://docs.stripe.com/revenue-recognition/examples/contra.md#dispute). |
| Bad debt | Contra revenue | Reduction in previously recognized revenue from marking invoices as uncollectible. [Learn how uncollectible invoices impact revenue](https://docs.stripe.com/revenue-recognition/examples/contra.md#uncollectible). |
| Voided billings | Contra revenue | Reduction in previously recognized revenue due to voided invoices. [Learn how voids impact revenue](https://docs.stripe.com/revenue-recognition/examples/contra.md#void). |
| Canceled unbilled invoice items | Contra revenue | Reduction in revenue from deleted unbilled invoice items. |
| Credit notes | Contra revenue | Reduction in previously recognized revenue due to credit notes issued to customers. [Learn how credit notes impact revenue](https://docs.stripe.com/revenue-recognition/examples/credit-notes.md). |
| Refunds from platform fees | Contra revenue | Reduction in revenue from refunded platform fees. [Learn how platform fee refunds impact revenue](https://docs.stripe.com/revenue-recognition/connect/destination-charges.md#loss-and-contra-revenue-with-issuing-refunds). |
| Transfer | Contra revenue | Reduction in revenue from separate transfers. [Learn how separate transfers impact revenue](https://docs.stripe.com/revenue-recognition/connect/charges-transfers.md). |
| Discounts | Contra revenue | Reduction in revenue from discounts. |
| External asset refunds | Contra revenue | Reduction in revenue from external asset refunds. |
| Net revenue | — | Total recognized revenue minus total contra revenue for the period. |

## Deferred revenue 

This section shows changes to your deferred revenue balance throughout the period.

| **Item** | **Description** |  |
| --- | --- | --- |
| Starting balance | Deferred revenue balance at the end of the previous month. |
| Deferred change from new billings this month | New deferred revenue from finalized invoice line items and standalone payments that occurred during the month. Every invoice line item and standalone payment books deferred revenue regardless of its revenue recognition schedule, except for unbilled revenue that was already recognized in previous months (included in “Revenue from unbilled services” in prior periods). |
| Less recognized revenue | Amount of deferred revenue recognized during the month. |
| Less credits issued | Remaining deferred revenue removed due to refunds, disputes, voids, uncollectible invoices, and credit notes. [Learn how refunds impact remaining deferred revenue](https://docs.stripe.com/revenue-recognition/examples/contra.md#refund). |
| Ending balance | Deferred revenue balance at the end of the month. |

## Examples

### Finalized invoice example 

This example shows how a finalized *invoice* (Invoices are statements of amounts owed by a customer. They track the status of payments from draft through paid or otherwise finalized. Subscriptions automatically generate invoices, or you can manually create a one-off invoice) affects the period summary.

**Scenario:**

- Invoice finalizes on October 10, 2025
- Single line item with a service period from October 20, 2025 to December 17, 2025
- Line item amount: 60 USD (12 USD recognized in October, 48 USD deferred)

**Period summary for October 2025:**

|  |
| **Recognized revenue** |  |
| Revenue from billings this month | 12 USD |
| **Net revenue** | 12 USD |
| **Deferred revenue** |  |
| Starting balance Oct 1 UTC | 0 USD |
| Deferred change from new billings this month | 60 USD |
| Less recognized revenue | -12 USD |
| **Ending balance Oct 31 UTC** | 48 USD |
| **Future scheduled billings** |  |
| Starting balance Oct 1 UTC | 0 USD |
| **Ending balance Oct 31 UTC** | 0 USD |

**If the invoice is refunded on November 15, 2025, the period summary for November 2025 shows:**

|  |
| **Recognized revenue** |  |
| Less refunds | -12 USD |
| **Net revenue** | -12 USD |
| **Deferred revenue** |  |
| Starting balance Nov 1 UTC | 48 USD |
| Less credits issued | -48 USD |
| **Ending balance Nov 30 UTC** | 0 USD |
| **Future scheduled billings** |  |
| Starting balance Nov 1 UTC | 0 USD |
| **Ending balance Nov 30 UTC** | 0 USD |

### Standalone payment example 

This example shows how a standalone payment (charge without an invoice) affects the period summary. Stripe recognizes standalone payment revenue immediately when the payment occurs.

**Scenario:**

- Charge occurs on October 15, 2025
- Charge amount: 17 USD

**Period summary for October 2025:**

|  |
| **Recognized revenue** |  |
| Revenue from billings this month | 17 USD |
| **Net revenue** | 17 USD |
| **Deferred revenue** |  |
| Starting balance Oct 1 UTC | 0 USD |
| Deferred change from new billings this month | 17 USD |
| Less recognized revenue | -17 USD |
| **Ending balance Oct 31 UTC** | 0 USD |
| **Future scheduled billings** |  |
| Starting balance Oct 1 UTC | 0 USD |
| **Ending balance Oct 31 UTC** | 0 USD |
