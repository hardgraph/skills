# Revenue Recognition API

Access Stripe Revenue Recognition reports programmatically to automate your accrual accounting.

For accrual accounting, Stripe Revenue Recognition provides [downloadable reports](https://docs.stripe.com/revenue-recognition/reports.md), such as a [period summary](https://docs.stripe.com/revenue-recognition/reports/period-summary.md) and a [revenue waterfall](https://docs.stripe.com/revenue-recognition/reports/waterfall.md). You can download these reports in CSV format through the [Dashboard](https://dashboard.stripe.com/revenue-recognition) or you can programmatically access them through the API.

Stripe Revenue Recognition has six supported report types:

- `revenue_recognition.debit_credit_summary.1`
- `revenue_recognition.debit_credit_by_price.1`
- `revenue_recognition.debit_credit_by_product.1`
- `revenue_recognition.debit_credit_by_customer.1`
- `revenue_recognition.debit_credit_by_invoice.1`
- `revenue_recognition.debit_credit_by_invoice_line_item.1`

> Because this feature is in beta, the data fields might change.

## Download a report

The following example downloads the debits and credits by summary for May 2025.

First, create a report run using [Create a Report Run](https://docs.stripe.com/api/reporting/report_run/create.md).

To get a report for May 2025, set `parameters[interval_start]` to 1 May 2025 and `parameters[interval_end]` to 1 Jun 2025.

```curl
curl https://api.stripe.com/v1/reporting/report_runs \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d "report_type=revenue_recognition.debit_credit_summary.1" \
  -d "parameters[interval_start]=1746057600" \
  -d "parameters[interval_end]=1748736000"
```

Next, check whether the [Report Run](https://docs.stripe.com/api/reporting/report_run/object.md) object succeeds by fetching the report run object:

The report run object ID starts with `frr_`.

```curl
curl https://api.stripe.com/v1/reporting/report_runs/{{REPORT_RUN_OBJECT_ID}} \
  -u "<<YOUR_SECRET_KEY>>:"
```

When the object’s `status` is `succeeded`, you can download the CSV using its `result.id` value, as in the following example:

The report run result ID starts with `file_`.

#### curl

```bash
curl https://files.stripe.com/v1/files/{{REPORT_RUN_RESULT_ID}}/contents \
  -u <<YOUR_SECRET_KEY>>:
```

## Report Run Parameters

| Report Type | Required Run Parameters | Optional Run Parameters |
| --- | --- | --- |
| revenue_recognition.debit_credit_summary.1 | - `interval_start`
- `interval_end` | - `decimal_format` |
| revenue_recognition.debit_credit_by_price.1 | - `interval_start`
- `interval_end` | - `customer`
- `decimal_format` |
| revenue_recognition.debit_credit_by_product.1 | - `interval_start`
- `interval_end` | - `customer`
- `decimal_format` |
| revenue_recognition.debit_credit_by_customer.1 | - `interval_start`
- `interval_end` | - `decimal_format` |
| revenue_recognition.debit_credit_by_invoice.1 | - `interval_start`
- `interval_end` | - `customer`
- `invoice`
- `invoice_line_item`
- `decimal_format` |
| revenue_recognition.debit_credit_by_invoice_line_item.1 | - `interval_start`
- `interval_end` | - `customer`
- `invoice`
- `invoice_line_item`
- `decimal_format` |

## Report Run Columns

By default, reports are run with the default set of columns. You can customize the selection and ordering of columns in the output by including the optional columns parameter with a [list of column names](https://docs.stripe.com/reports/api.md#report-runs). You can find the supported columns for each report type below.

### Summary

API report type: `revenue_recognition.debit_credit_summary.1`

| Column Name | Default | Description |
| --- | --- | --- |
| accounting_period | ✓ default | The accounting period |
| open_accounting_period | ✓ default | The open accounting period; entries in open periods are subject to change |
| currency | ✓ default | Three-letter [ISO code for the currency](https://docs.stripe.com/currencies.md) of the amount. |
| debit | ✓ default | The debited account |
| credit | ✓ default | The credited account |
| amount | ✓ default | Amount change, expressed in major units of the currency (for example, dollars for USD, or pesos for MXN). |
| credit_gl_code |  | The credited general ledger code |
| debit_gl_code |  | The debited general ledger code |

### By Price

API report type: `revenue_recognition.debit_credit_by_price.1`

| Column Name | Default | Description |
| --- | --- | --- |
| accounting_period | ✓ default | The accounting period |
| open_accounting_period | ✓ default | The open accounting period; entries in open periods are subject to change |
| currency | ✓ default | Three-letter [ISO code for the currency](https://docs.stripe.com/currencies.md) of the amount. |
| price_id | ✓ default | The price associated with this change. |
| debit | ✓ default | The debited account |
| credit | ✓ default | The credited account |
| amount | ✓ default | Amount change, expressed in major units of the currency (for example, dollars for USD, or pesos for MXN). |
| credit_gl_code |  | The credited general ledger code |
| debit_gl_code |  | The debited general ledger code |

### By Product

API report type: `revenue_recognition.debit_credit_by_product.1`

| Column Name | Default | Description |
| --- | --- | --- |
| accounting_period | ✓ default | The accounting period |
| open_accounting_period | ✓ default | The open accounting period; entries in open periods are subject to change |
| currency | ✓ default | Three-letter [ISO code for the currency](https://docs.stripe.com/currencies.md) of the amount. |
| product_id | ✓ default | The product associated with this change. |
| debit | ✓ default | The debited account |
| credit | ✓ default | The credited account |
| amount | ✓ default | Amount change, expressed in major units of the currency (for example, dollars for USD, or pesos for MXN). |
| credit_gl_code |  | The credited general ledger code |
| debit_gl_code |  | The debited general ledger code |

### By Customer

API report type: `revenue_recognition.debit_credit_by_customer.1`

| Column Name | Default | Description |
| --- | --- | --- |
| accounting_period | ✓ default | The accounting period |
| open_accounting_period | ✓ default | The open accounting period; entries in open periods are subject to change |
| currency | ✓ default | Three-letter [ISO code for the currency](https://docs.stripe.com/currencies.md) of the amount. |
| customer_id | ✓ default | The customer associated with this change. |
| debit | ✓ default | The debited account |
| credit | ✓ default | The credited account |
| amount | ✓ default | Amount change, expressed in major units of the currency (for example, dollars for USD, or pesos for MXN). |
| credit_gl_code |  | The credited general ledger code |
| debit_gl_code |  | The debited general ledger code |

### By Invoice

API report type: `revenue_recognition.debit_credit_by_invoice.1`

| Column Name | Default | Description |
| --- | --- | --- |
| accounting_period | ✓ default | The accounting period |
| open_accounting_period | ✓ default | The open accounting period; entries in open periods are subject to change |
| currency | ✓ default | Three-letter [ISO code for the currency](https://docs.stripe.com/currencies.md) of the amount. |
| transaction_model_id | ✓ default | The model in Stripe associated with this change - either an invoice line item, invoice, invoiceitem, charge, or and so on |
| debit | ✓ default | The debited account |
| credit | ✓ default | The credited account |
| booked_date | ✓ default | The date that the ledger entry is added to the books. |
| amount | ✓ default | Amount change, expressed in major units of the currency (for example, dollars for USD, or pesos for MXN). |
| debit_gl_code |  | The debited general ledger code |
| credit_gl_code |  | The credited general ledger code |
| invoice_id |  | The invoice associated with this change. Standalone charges or invoice items not associated with an invoice are `null`. |
| invoice_line_item_id |  | The ID of the invoice line_item. |
| invoice_item_id |  | The ID of the invoice item |
| invoice_number |  | The customer unique number associated with the invoice. |
| subscription_item_id |  | The ID of the subscription_item. |
| price_id |  | The price associated with this change. Standalone charges or invoice items not associated with a price are `null`. |
| product_id |  | The product associated with this price. |
| customer_id |  | The customer associated with this change. |
| subscription_id |  | The subscription associated with this change. |
| charge_id |  | The charge associated with this change. |
| refund_id |  | The refund associated with this change. |
| dispute_id |  | The dispute associated with this change. |
| presentment_currency |  | The presentment (customer facing) currency of the transaction. |
| presentment_amount |  | The presentment (customer facing) amount. |

### By Invoice Line Item

API report type: `revenue_recognition.debit_credit_by_invoice_line_item.1`

| Column Name | Default | Description |
| --- | --- | --- |
| accounting_period | ✓ default | The accounting period |
| open_accounting_period | ✓ default | The open accounting period; entries in open periods are subject to change |
| currency | ✓ default | Three-letter [ISO code for the currency](https://docs.stripe.com/currencies.md) of the amount. |
| transaction_model_id | ✓ default | The model in Stripe associated with this change—an invoice line item, invoice, invoice item, charge, and so on. |
| debit | ✓ default | The debited account |
| credit | ✓ default | The credited account |
| booked_date | ✓ default | The date that the ledger entry is added to the books. |
| amount | ✓ default | Amount change, expressed in major units of the currency (for example, dollars for USD, or pesos for MXN). |
| debit_gl_code |  | The debited general ledger code |
| credit_gl_code |  | The credited general ledger code |
| invoice_id |  | The invoice associated with this change. Standalone charges or invoice items not associated with an invoice are `null`. |
| invoice_line_item_id |  | The ID of the invoice line_item. |
| invoice_item_id |  | The ID of the invoice item |
| subscription_item_id |  | The ID of the subscription_item. |
| price_id |  | The price associated with this change. Standalone charges or invoice items not associated with a price are `null`. |
| product_id |  | The product associated with this price. |
| customer_id |  | The customer associated with this change. |
| subscription_id |  | The subscription associated with this change. |
| charge_id |  | The charge associated with this change. |
| refund_id |  | The refund associated with this change. |
| dispute_id |  | The dispute associated with this change. |
| presentment_currency |  | The presentment (customer facing) currency of the transaction. |
| presentment_amount |  | The presentment (customer facing) amount. |

If you encounter any issues, you can contact [revenue-recognition-api-beta@stripe.com](mailto:revenue-recognition-api-beta@stripe.com).
