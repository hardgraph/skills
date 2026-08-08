# Invoice rendering templates

Use invoice rendering templates to personalize your invoice appearance for different customers

You can tailor content on your invoices for specific groups of customers. Examples might include:

- Displaying specific text in your invoice’s footer field for customers from a particular country
- Displaying a specific memo note for customers with a particular revenue channel
- Grouping line items in a certain manner for customers with complex transactions

This guide describes how you can use invoice rendering templates to:

- Store and reuse common values for invoice fields instead of entering them for every relevant invoice
- Store, manage, and update invoice field values that apply to large sets of customers
- Configure Line Item Grouping rules, which can’t be set outside of invoice rendering templates
- For subscription invoices, customize invoice values without interacting directly with the invoice

## Set up invoice rendering templates

You can create invoice rendering templates only in the Dashboard. You can’t create them using the API.

1. In your Billing settings, select the [Invoices > Templates tab](https://dashboard.stripe.com/settings/billing/invoice-templates).
2. Click **+ Create template**.
3. Enter a name for your template.
4. Specify values for the fields you want to include in the template. Invoice templates support the memo, footer, and custom fields.
5. Optionally, you can [define one or more line item groups using Common Expression Language (CEL) expressions](https://docs.stripe.com/invoicing/group-line-items.md).
6. To see a preview of your template, enter the ID of an applicable invoice in the **Invoice ID** field of the Preview pane. However, any values set directly on that invoice override the corresponding template value. For example, if that invoice has a footer value, the preview displays the invoice’s footer, not the template’s footer.

## Apply invoice rendering templates to invoices

#### Dashboard

In the Dashboard, you can apply a template to invoices in the following ways:

- In the Invoice editor
- In the Subscriptions editor, to apply to all invoices associated with the subscription
- In a customer’s invoice settings, to apply to all future invoices associated with that customer

#### API

> #### Use the Accounts v2 API to represent customers
> 
> The Accounts v2 API is generally available for Connect users, and in public preview for other Stripe users. If you’re part of the Accounts v2 preview, you need to specify a [preview version](https://docs.stripe.com/api-v2-overview.md#sdk-and-api-versioning) in your code.
> 
> To join the Accounts v2 preview, go to [Account previews and features](https://dashboard.stripe.com/settings/previews) in your Dashboard and enable **Reusable payment methods for Global Payouts**.
> 
> For most use cases, we recommend [modeling your customers as customer-configured Account objects](https://docs.stripe.com/accounts-v2/use-accounts-as-customers.md) instead of using [Customer](https://docs.stripe.com/api/customers.md) objects.

#### Accounts v2

With the API, you can apply a template to invoices in the following ways:

- When you create a draft invoice using the [Invoicing API](https://docs.stripe.com/api/invoices/create.md#create_invoice-rendering-template):

```curl
curl https://api.stripe.com/v1/invoices \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d "customer_account={{CUSTOMERACCOUNT_ID}}" \
  -d collection_method=send_invoice \
  -d days_until_due=30 \
  -d "rendering[template]=inrtem_xxx"
```

- When you create or update a customer-configured `Account`, use [configuration.customer.billing.invoice.rendering.template](https://docs.stripe.com/api/v2/core/accounts/object.md#v2_account_object-configuration-customer-billing-invoice-rendering-template) to apply a template to all future invoices associated with that customer, including subscription-generated invoices:

```curl
curl -X POST https://api.stripe.com/v2/core/accounts \
  -H "Authorization: Bearer <<YOUR_SECRET_KEY>>" \
  -H "Stripe-Version: 2026-07-29.preview" \
  --json '{
    "contact_email": "jenny.rosen@example.com",
    "display_name": "Jenny Rosen",
    "configuration": {
        "customer": {
            "billing": {
                "invoice": {
                    "rendering": {
                        "template": "inrtem_xxx"
                    }
                }
            }
        }
    },
    "include": [
        "configuration.customer"
    ]
  }'
```

#### Customers v1

With the API, you can apply a template to invoices in the following ways:

- When you create a draft invoice using the [Invoicing API](https://docs.stripe.com/api/invoices/create.md#create_invoice-rendering-template):

```curl
curl https://api.stripe.com/v1/invoices \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d customer=cus_xxx \
  -d collection_method=send_invoice \
  -d days_until_due=30 \
  -d "rendering[template]=inrtem_xxx"
```

- When you create or update a `Customer`, use [invoice_settings.rendering_options.template](https://docs.stripe.com/api/customers/create.md#create_customer-invoice_settings-rendering_options-template) to apply a template to all future invoices associated with that customer, including subscription-generated invoices:

```curl
curl https://api.stripe.com/v1/customers \
  -u "<<YOUR_SECRET_KEY>>:" \
  -d "name=Jenny Rosen" \
  -d "invoice_settings[rendering_options][template]=inrtem_xxx"
```

You can’t use the API to apply invoice templates directly to subscriptions, subscription schedules, or quotes. Instead, you can attach a template to a customer to apply to all future invoices associated with that customer, or apply a template to invoices while they’re in draft status.

## Override and update templates

You can configure invoice footer, memo, and custom field values in multiple places. When multiple values can apply to an invoice field, they’re prioritized in the following order, from highest to lowest:

| Priority | Setting | Method |
| --- | --- | --- |
| 1 | On the invoice | Dashboard or API |
| On the subscription | Dashboard only |
| 2 | Template applied to the invoice | Dashboard or API |
| Template applied to the subscription | Dashboard only |
| 3 | Template attached to the customer | Dashboard or API |
| 4 | Invoice settings on the customer | API only |
| 5 | Invoice settings on the account | Dashboard only |

> Defined invoice settings always apply unless overridden by a higher-priority setting. For example, to create a one-time invoice for a customer that overrides the customer’s attached template, you must either apply a different template to the invoice or set the invoice values directly.

When you update or replace a template, the new values apply to all future invoices associated with the template. For example, if you update a value in a customer’s template, the new value applies to all future invoices for that customer’s existing subscriptions unless the subscription template overrides it.
