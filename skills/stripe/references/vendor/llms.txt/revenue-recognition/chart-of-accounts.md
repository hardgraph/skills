# Map to your chart of accounts

Map transactions from the Stripe default accounts to the chart of accounts in your general ledger.

You can customize Stripe Revenue Recognition reporting to use your General Ledger (GL) chart of accounts instead of using the default [Stripe accounts](https://docs.stripe.com/revenue-recognition/methodology.md). You can configure a rule to map transactions by product, shipping region, or invoice metadata to your GL account. Stripe applies your custom mappings to the [CSV reports](https://docs.stripe.com/revenue-recognition/reports.md#statements) you download and also when you [audit your revenue numbers](https://docs.stripe.com/revenue-recognition/reports/audit-numbers.md). A mapping rule consists of the following:

| Mapping rule attribute | Description |
| --- | --- |
| Stripe account | The [Stripe default account](https://docs.stripe.com/revenue-recognition/chart-of-accounts.md#stripe-chart-of-accounts) that you want to override. |
| GL account | The name of the GL account you want to override the Stripe account with. |
| GL account number | The number corresponding to the GL account. |
| Time period | The time period the mapping applies to.

An [invoice line item](https://docs.stripe.com/api/invoice-line-item/object.md) fulfills the time period requirement if the finalization time of the invoice is within the specified time period.

A [charge](https://docs.stripe.com/api/charges.md) fulfills the time period requirement if the balance transaction it corresponds to has a creation time that’s within the specified time period. |
| Condition | An optional criteria to map transactions by product, shipping region, invoice metadata, or external payment method. If not specified, all transactions involving the configured Stripe account are mapped to the GL account. |
| Status | **Active**: The mapping rule is active and all transactions are mapped as per the rule.

**Processing**: The rule is processing. On completion, the rule is active and transactions are mapped accordingly. |

## Stripe chart of accounts

The following table lists all accounts used in Revenue Recognition across both income statement and balance sheet classifications.

| Account | Debit or Credit type | Description |
| --- | --- | --- |
| Revenue | Revenue (credit) | Recognizable portion of finalized invoices, prorated invoice items, and usage-based billing that count towards revenue during the month. For example, if an invoice line item is for 90 USD with 10 USD in taxes, the total invoice is 100 USD, but the recognizable portion is only 90 USD. |
| Revenue share | Revenue (credit) | Revenue recognized from revenue-sharing arrangements between a platform and its connected accounts. This account is credited when a portion of processed revenue is allocated as shared revenue under a revenue-sharing agreement. |
| Refunds | Contra revenue (debit) | Portion of the refunded amount previously recognized. For example, if you issue a 120 USD refund on an annual subscription during the second month, 20 USD for the first 2 months is contra revenue. The remaining 100 USD is adjusted and reflected in your deferred revenue balance in the balance sheet. |
| Disputes | Contra revenue (debit) | Portion of the disputed amount previously recognized. For example, if there’s a 120 USD dispute on an annual subscription during the second month, 20 USD for the first 2 months is contra revenue. The remaining 100 USD is adjusted and reflected in your deferred revenue balance in the balance sheet. |
| Credit notes | Contra revenue (debit) | Portion of the credit note amount previously recognized. For example, if there’s a 120 USD credit note on an annual subscription during the second month, 20 USD for the first 2 months is contra revenue. The remaining 100 USD is adjusted and reflected in your deferred revenue balance in the balance sheet. |
| Bad debt | Contra revenue (debit) | Previously recognized revenue from invoices that have been marked as uncollectible. |
| Voids | Contra revenue (debit) | Previously recognized revenue from invoices that have been voided. |
| Unbilled voids | Contra revenue (debit) | Previously recognized revenue from prorated invoice items that have been deleted. These items are sometimes deleted when they generate unbilled accounts receivable and revenue. |
| Transfer | Contra revenue (debit) | Previously recognized revenue from separate transfers. |
| Discounts | Contra revenue (debit) | The recognized revenue from invoices that received discounts. This account is only used when you enable [Record discounts as contra revenue](https://docs.stripe.com/revenue-recognition/revenue-settings.md#book-discounts-as-contra-revenue) in your settings. |
| External asset refunds | Contra revenue (debit) | Previously recognized revenue from refunds on invoices that were marked as paid outside of Stripe. When you refund an out-of-band payment, the recognized portion reduces through this contra revenue account and the external asset account decreases accordingly. |
| Revenue share refund | Contra revenue (debit) | Previously recognized revenue from revenue-sharing arrangements that has been reversed or refunded. This account is debited when a revenue share transaction is reversed, reducing the originally recognized shared revenue. |
| Overpayments | Expenses (debit) | Expenses incurred when a customer’s payment exceeds the invoiced amount and the excess cannot be applied to another open invoice or returned as a customer credit balance. |
| Customer balance adjustments | Expenses (debit) | Expenses incurred due to manual adjustments to a customer credit balance or exclusion associated with post-paid credit notes on customer balance. |
| External customer balance adjustments | Expenses (debit) | Expenses incurred due to exclusion associated with post-paid credit notes on external customer balance. |
| Underpayments | Expenses (debit) | Expenses incurred due to transfers that underpay an invoice, as used by the [customer credit balance](https://docs.stripe.com/invoicing/bank-transfer.md#underpayments) payment method. |
| Fees | Expenses (debit) | Expenses incurred due to Stripe fees. |
| Recoverables | Gains (credit) | Recovered funds that aren’t attributable to revenue. For example, if you have a 120 USD dispute on an annual subscription during the second month, 20 USD for the first 2 months is contra revenue and the remaining 100 USD is adjusted from the deferred revenue balance. If you win the dispute and 120 USD is returned to you, 20 USD is reflected as revenue and the remaining 100 USD is reflected as recoverables. |
| Exclusion | Gains (credit) | Excluded funds that aren’t attributable to revenue. To exclude transactions, set up [exclusion rules](https://docs.stripe.com/revenue-recognition/rules/create-a-rule.md#treatments) or use [exclusion import](https://docs.stripe.com/revenue-recognition/data-import.md#exclusion-import). |
| FX loss | Losses (debit) | Total loss due to foreign currency exchange rates. |
| FX clearing | Losses (debit) | A transient clearing account that records intermediate foreign currency exchange differences during multi-currency settlement. Amounts in this account are reclassified to FX loss or offset by corresponding entries as FX transactions are finalized within the settlement period. |
| Unrealized FX loss | Losses (debit) | Mark-to-market losses on open foreign currency positions that haven’t yet been settled. Unlike FX loss, these amounts are unrealized and may reverse in future periods when the underlying foreign currency transaction settles. |
| Other loss | Losses (debit) | The portion of contra revenue that exceeds the total invoice represents an overcompensation in cash to the customer. For example, if a 100 USD invoice is partially refunded by 80 USD and then disputed for an additional 80 USD, 60 USD is categorized as “Other loss.” |
| Connect transfer loss | Losses (debit) | Losses when your platform covers a connected account’s negative balance. This includes destination-charge refunds taken from the platform balance and Connect collection transfers from the platform reserve balance to the connected account balance. A transfer reversal reverses the `ConnectTransferLoss` account. |
| Accounts receivable | Assets (debit) | `AccountsReceivable` represents the amount you bill to customers and might be inclusive of taxes and other amounts that aren’t included in recognizable revenue. This account increases when invoices are finalized and decreases when invoices are paid. The ending balance reflects the amount due from customers at the end of each month. |
| Customer cash balance | Assets (debit) | The cash balance for a customer. Funds can be put into or taken out of the account by either the customer or merchant. |
| Unbilled accounts receivable | Assets (debit) | Transactions (such as prorations due to upgrades or downgrades) that have service periods that start before an invoice is issued. The ending balance reflects the transactions that have accrued revenue but haven’t been invoiced yet. |
| External asset | Assets (debit) | Invoices you manually mark as paid when you receive funds outside of Stripe. The ending balance reflects the amount of invoices that were marked as paid using the Stripe Dashboard. This reduces `AccountsReceivable`. |
| Pending cash | Assets (debit) | Cash from payments that haven’t been confirmed. It can take several days to confirm whether a payment is successful. When the amount is confirmed, it’s transferred to the Cash account. This happens with [delayed notification payment methods](https://docs.stripe.com/payments/payment-methods.md#payment-notification) like ACH debit. |
| Deferred fees | Assets (debit) | Stripe fees associated with invoices that span future service periods, deferred and expensed over the period in which the corresponding revenue is recognized. This account decreases as the deferred fee is expensed. |
| Cash | Assets (debit) | Cash is the net cash amount received. **This doesn’t include Stripe fees or payouts.** It’s calculated by subtracting refunds, disputes, and dispute reversals from your Stripe Balance Net Charge amount. Cash increases when customers pay their outstanding invoice balance. This also results in a corresponding decrease in AccountsReceivable. |
| Deferred revenue | Liabilities (credit) | Services that have been invoiced but not recognized as revenue. Deferred revenue is a liability on your balance sheet. It represents cash you’ve collected for services you haven’t yet delivered. Stripe enables [long-term deferred revenue](https://docs.stripe.com/revenue-recognition/revenue-settings.md#long-term-deferred-revenue) by default, and the month-end balance only includes amounts expected to be recognized within the next 12 accounting periods. If you disable this setting, the balance includes all deferred revenue, regardless of service period length. |
| Long-term deferred revenue | Liabilities (credit) | Non-current portion of deferred revenue for service periods extending beyond 12 accounting periods. Amounts automatically reclassify to deferred revenue when they enter the 12-period horizon. Revenue Recognition uses this account only when you enable the [long-term deferred revenue setting](https://docs.stripe.com/revenue-recognition/revenue-settings.md#long-term-deferred-revenue). It’s enabled by default. |
| Customer balance | Liabilities (credit) | Credits that your customers accrue. The ending balance reflects the amount of invoices that were paid using customer credit balance. This reduces AccountsReceivable. |
| External customer balance | Liabilities (credit) | External credits that your customers accrue. The ending balance reflects the amount of credit notes that credit outside of Stripe. |
| Customer cash adjustment | Liabilities (credit) | Liability incurred due to adjustments to the customer’s cash balance associated with bank transfer payments. This balance represents how much of the paid cash balance hasn’t been used. |
| Passthrough fees | Liabilities (credit) | Passthrough fees occur when you’re expected to collect cash from a customer on behalf of a third party. The account can be set up by rules. |
| Deferred tax liability | Liabilities (credit) | The deferred tax liability of issued invoices. The month-end balance reflects the amount expected to be booked as tax liability in future periods. Stripe Revenue Recognition deferred tax liability support is currently in private preview. |
| Tax liability | Liabilities (credit) | The tax component of issued invoices. The ending balance represents the tax amount invoiced to customers but is still owed to relevant tax authorities. |
| Deferred discounts | Liabilities (credit) | The deferred discount of issued invoices. The month-end balance reflects the amount expected to be booked as a discount in future periods. Revenue Recognition uses this account only when you enable [Record discounts as contra revenue](https://docs.stripe.com/revenue-recognition/revenue-settings.md#book-discounts-as-contra-revenue) in your settings. |

## Configuring a mapping rule

Mapping rule configuration is a 4-step process—click the add mapping button on the accounts mapping page to begin.

1. **Select Stripe account**: Select the default Stripe account from the dropdown for which you want to create the rule.
2. **Select GL account**: You can select your GL account from the dropdown or add one if you can’t find it in the dropdown. When setting up the rules for the first time, you have to add these accounts by specifying the GL account name and number. You have to specify at least a name or a number to add the account.
3. **Specify time period**: The time period is the time frame in which the mapping rule is applicable. Select a start and end date from the dropdown to configure the time period. If you specify an time period that overlaps with closed accounting periods, you’ll see corrections in your report in the current open accounting period. You can reopen the past accounting periods corresponding to the time period to avoid corrections.
4. (Optional) **Specify mapping condition\***: You can specify a mapping condition on any of the following attributes:
   - **Product**: If you have product specific accounts in your GL, you can classify your transactions based on the products that you’ve configured in the Stripe Dashboard.
   - **Shipping region**: Similar to products, you can specify the shipping region to map transactions to the relevant GL account. Only ISO-compliant country and state codes are supported.
   - **Invoice metadata**: You can configure a custom rule using invoice metadata if your GL accounts don’t track transactions by product or shipping region. Create a rule by selecting a key and adding a value. The keys are from metadata you created in past invoices.**
   - **Price**: Use Price IDs configured in the Stripe Dashboard to map transactions to the relevant GL account.
   - **Credit note**: Map credit note transactions to the relevant GL account using the following:
     - **Credit note line item description**: Specify the credit note line item description to map transactions to the relevant GL account. Stripe matches the credit note line item description using a case-insensitive substring match, so a rule configured with `monthly subscription` also matches a credit note line item described as `Monthly Subscription - Pro plan`, providing flexibility in how you map transactions.

     - **Credit note custom reason**: Specify the custom reason you provide when issuing a credit note to map transactions to the relevant GL account. Because custom reasons are free-form text, Stripe matches them using a case-insensitive substring match. For example, a rule configured with the custom reason `product defect` also matches a credit note issued with the reason `Product Defect - batch 42`.
   - **Coupon**: Use Coupon IDs configured in the Stripe Dashboard to map transactions to the relevant GL account.

   - **External payment method**: Map transactions to a GL account based on the external (custom) payment method used. Use this option when you accept payments through custom payment methods (for example, cash, checks, or third-party processors) configured in Stripe and want to route them to separate GL accounts. You can match by custom payment method ID or by the payment method’s display name.

Click **Map chart of accounts** to create the mapping rule and for Stripe to [process the data](https://docs.stripe.com/revenue-recognition/data-freshness.md). The rule’s status changes to active when the data processing is complete, and you can then download reports with the mapped GL accounts.

## Mapping rule configuration example

The following example involves 3 different products:

- Product A: Annual subscription cost of 1,200 USD
- Product B: Annual subscription cost of 2,400 USD
- Product C: Annual subscription cost of 3,600 USD

If you sell 1 subscription each for A, B, and C in January, your journal entry at the end of the month appears as follows without account mapping:

| Account | January |
| --- | --- |
| Revenue | +600 USD |
| Deferred Revenue | +6600 USD |

However, the user has 3 separate revenue accounts in its GL, say revenue_A, revenue_B, and revenue_C for tracking revenue corresponding to these 3 products. The user has to do manual work to identify revenue in these accounts before posting to its GL.

If you have product-specific accounts in your General Ledger that you want to map to, you can create 3 mapping rules:

| Stripe account | GL account number | GL account | Condition | Time period |
| --- | --- | --- | --- | --- |
| Revenue | 10001 | revenue_A | Product A | Jan 2026 - Indefinite |
| Revenue | 10002 | revenue_B | Product B | Jan 2026 - Indefinite |
| Revenue | 10003 | revenue_C | Product C | Jan 2026 - Indefinite |

After you set up these rules, your journal entries will contain three line items reflecting the revenue distribution for each product. This can help you streamline the process of posting to your GL.

| GL account number | Account | January |
| --- | --- | --- |
| 1001 | revenue_A | +100 USD |
| 1002 | revenue_B | +200 USD |
| 1003 | revenue_C | +300 USD |
| - | Deferred Revenue | +6600 USD |

If you need to create multiple mapping rules at once, you can use our [bulk account mapping feature](https://docs.stripe.com/revenue-recognition/chart-of-accounts/bulk-account-mappings.md#upload-mappings) to upload mappings via CSV file. This reduces manual effort and minimizes the risk of errors when configuring numerous GL accounts.

\* For a default Stripe account, you can only pick one attribute to create a rule. Please [create a ticket](https://support.stripe.com/contact/email?topic=financial_reports) on our support page if you have any questions.\** Don’t import any personally identifiable information and/or protected health information.
