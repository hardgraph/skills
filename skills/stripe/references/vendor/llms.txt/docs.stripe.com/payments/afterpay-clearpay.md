# Afterpay and Clearpay payments

Offer your customers flexible financing while getting paid upfront with Afterpay (also known as Clearpay in the UK)..

[Afterpay](https://www.afterpay.com/) is a buy now, pay later (BNPL) payment method that allows customers to split purchases into interest-free installments. When customers select Afterpay as their payment method, Stripe redirects them to complete authentication and approval. You’re paid immediately while customers pay over time.
Payment method family: Buy Now, Pay Later
Usability: Single-use
Access type: Instant provisional access
Payment confirmation timing: Immediate
Settlement timing: Standard payout schedule
Pricing: https://stripe.com/en-us/pricing/local-payment-methods#cash-app-afterpay
> Afterpay has rebranded to Cash App Afterpay in the US. This update gives your business access to Cash App users without requiring any changes, unless you have custom Afterpay components. For more information about the change, see the [Cash App Afterpay support page](https://support.stripe.com/questions/afterpay-is-now-branded-as-cash-app-afterpay-in-the-us).

## Eligibility and availability 

### Account eligibility
Business location: AU, CA, GB, NZ
Account type: ✓ Merchant, ✓ Platform or marketplace (Connect)
Business model: ✗ B2B, ✓ B2C
Business category: View list of prohibited and restricted business categories
For more information about Afterpay eligibility for your account, go to your [Payment methods settings](https://dashboard.stripe.com/settings/payment_methods).

In addition to the categories of [businesses restricted from using Stripe overall](https://stripe.com/restricted-businesses), the following categories are prohibited from using Afterpay.

- Alcohol
- Donations
- Pre-orders
- NFTs
- B2B

For the complete list, see the [terms of service](https://stripe.com/afterpay-clearpay/legal#restricted-businesses).

Your account might be reviewed after activation to confirm eligibility. While Afterpay doesn’t enforce any additional website requirements, make sure that you provide a website that meets the [Stripe account activation requirements](https://support.stripe.com/questions/business-website-for-account-activation-faq). You can contact [Stripe support](https://support.stripe.com/) to appeal any Afterpay capability restrictions.

Review the list of *merchant category codes* (A Merchant Category Code (MCC) is a four-digit number that classifies the type of goods or services a business offers) that are prohibited or restricted for Afterpay.

- **Prohibited** MCCs are not supported by this payment method.
- **Restricted** MCCs may be supported, but require additional information at account creation.

| MCC | Description | Type |
| --- | --- | --- |
| 2842 | Specialty Cleaning | Restricted |
| 4411 | Cruise Lines | Prohibited |
| 4829 | Money Orders - Wire Transfers | Prohibited |
| 4900 | Utilities | Restricted |
| 5122 | Drugs, Drug Proprietaries, and Druggist Sundries | Prohibited |
| 5169 | Chemicals and Allied Products (Not Elsewhere Classified) | Restricted |
| 5172 | Petroleum and Petroleum Products | Restricted |
| 5199 | Nondurable Goods (Not Elsewhere Classified) | Restricted |
| 5521 | Car and Truck Dealers (Used Only) | Prohibited |
| 5592 | Motor Homes Dealers | Restricted |
| 5933 | Pawn Shops | Prohibited |
| 5960 | Direct Marketing - Insurance Services | Restricted |
| 5962 | Direct Marketing - Travel | Prohibited |
| 5963 | Door-To-Door Sales | Prohibited |
| 5964 | Direct Marketing - Catalog Merchant | Restricted |
| 5965 | Direct Marketing - Combination Catalog and Retail Merchant | Restricted |
| 5966 | Direct Marketing - Outbound Telemarketing | Prohibited |
| 5967 | Adult Content and Services | Prohibited |
| 5968 | Direct Marketing - Subscription | Restricted |
| 5969 | Direct Marketing - Other | Restricted |
| 5993 | Cigar Stores and Stands | Prohibited |
| 6010 | Manual Cash Disburse | Prohibited |
| 6011 | Financial Service | Prohibited |
| 6012 | Financial Institutions | Prohibited |
| 6051 | Cryptocurrency exchanges and wallets | Prohibited |
| 6211 | Security Brokers/Dealers | Prohibited |
| 6300 | Insurance Underwriting, Premiums | Prohibited |
| 6540 | Non-FI, Stored Value Card Purchase/Load | Prohibited |
| 7011 | Hotels, Motels, and Resorts | Restricted |
| 7012 | Timeshares | Restricted |
| 7321 | Credit Reporting Agencies | Restricted |
| 7394 | Equipment Rental | Restricted |
| 7800 | Government-Owned Lotteries (US Region only) | Prohibited |
| 7801 | Government Licensed On-line Casinos (On-Line Gambling) | Prohibited |
| 7802 | Government-Licensed Horse/Dog Racing | Prohibited |
| 7829 | Picture/Video Production | Restricted |
| 7841 | Video Tape Rental Stores | Prohibited |
| 7995 | Betting/Casino Gambling | Prohibited |
| 8111 | Legal Services, Attorneys | Restricted |
| 8398 | Charitable and Social Service Organizations - Fundraising | Restricted |
| 8641 | Civic, Social, Fraternal Associations | Restricted |
| 8651 | Political Organizations | Restricted |
| 8661 | Religious Organizations | Restricted |
| 8999 | Professional Services | Restricted |
| 9211 | Court Costs | Prohibited |
| 9222 | Fines - Government Administrative Entities | Prohibited |
| 9223 | Bail and Bond Payments | Prohibited |
| 9311 | Tax Payments - Government Agencies | Prohibited |
| 9405 | US Federal Govt Agencies or Departments | Restricted |
| 9950 | Intra-Company Purchases | Prohibited |

Stripe accounts in the following countries can accept Afterpay payments with local currency settlement.

AMER: CA, US

EMEA: GB

APAC: AU, NZ

Customers in the following countries can use Afterpay.

AMER: CA, US

EMEA: GB

APAC: AU, NZ

### Payment support
Buyer location: AU, CA, GB, NZ
Presentment currency: AUD, NZD, GBP, USD, CAD
Geographic coverage: ✓ Domestic, ✓ Crossborder
Transaction limits: Minimum amount: 1.00 USD

Maximum amount: 4,000.00 USD
> Afterpay only supports domestic transactions in general availability, meaning you can only sell to customers in the same country as your business. Cross-border transactions are available in private preview. If you’re using [Dynamic payment methods](https://docs.stripe.com/payments/payment-methods/dynamic-payment-methods.md), Stripe handles a customer’s payment method eligibility automatically. If you use [payment_method_types](https://docs.stripe.com/api/payment_intents/object.md#payment_intent_object-payment_method_types), you must either configure your integration so that it only presents Afterpay to eligible customers, or use dynamic payment methods.

### Payment options

### AU

| Payment option | Description | Amount range |
| --- | --- | --- |
| Pay in 4 | 4 interest-free bi-weekly payments | AUD&nbsp;1–AUD&nbsp;4,000 |

### CA

| Payment option | Description | Amount range |
| --- | --- | --- |
| Pay in 4 | 4 interest-free bi-weekly payments | CAD&nbsp;1–CAD&nbsp;2,000 |

### NZ

| Payment option | Description | Amount range |
| --- | --- | --- |
| Pay in 4 | 4 interest-free bi-weekly payments | NZD&nbsp;1–NZD&nbsp;4,000 |

### GB

| Payment option | Description | Amount range |
| --- | --- | --- |
| Pay in 4 | 4 interest-free bi-weekly payments | GBP&nbsp;1–GBP&nbsp;1,200 |

### US

| Payment option | Description | Amount range |
| --- | --- | --- |
| Pay in 4 | 4 interest-free bi-weekly payments | USD&nbsp;1–USD&nbsp;2,000 |
| Monthly installments | 6 or 12 month interest-bearing installments | USD&nbsp;400–USD&nbsp;4,000 |

### Customer country filtering 

Customer country filtering applies when you enable a dynamic payment method on the Payment Element or Checkout Session. Afterpay only displays as a payment method option if the customer’s country is supported.

We determine the customer’s country in the following priority order:

1. Shipping address country: The two-letter country code, not the full name of the country.
2. Geocoded country: The country based on the client-side IP address.

## Capabilities 
Recurring payments: ✗ Not supported

Payment authorizations:
  ✗ Extended authorizations
  ✗ Flexible extended authorizations
  ✗ Incremental authorizations
  ✗ Decremental authorizations
  ✗ Re-authorizations

Payment captures:
  ✓ Manual capture
  ✓ Partial capture
  ✗ Multi-capture
  ✗ Over-capture

Refunds:
  ✓ Partial refunds
  ✓ Full refunds
  Submission window: 120 days
  Processing time: 10 business days

Disputes:
  ✗ Partial disputes
  ✓ Full disputes
  Submission window: 120 days
### Refunds 

You have up to 120 days from the original payment to submit a refund using the [Dashboard](https://dashboard.stripe.com/payments) or [Refunds API.](https://docs.stripe.com/api/refunds/create.md) This is the maximum the payment method allows-your return policy determines what you offer customers.

Refunds for Afterpay payments are asynchronous and take up to 10 business days to complete. Afterpay refunds can’t be canceled. 

#### Refund process 



Afterpay refunds are never processed as reversals. When you issue a refund, Afterpay cancels any remaining scheduled payments and returns any amounts the customer has already paid. The customer sees the update reflected in their Afterpay account.

#### Tracking refunds 

You can view refund status in the [Dashboard](https://dashboard.stripe.com/payments). Open the payment and click **View Details** on the refund entry, or [retrieve the Refund object](https://docs.stripe.com/api/refunds/retrieve.md) and check its `status` field. Stripe also notifies you of the final refund status using the `refund.updated` or `refund.failed` *webhook* (A webhook is a real-time push notification sent to your application as a JSON payload through HTTPS requests) event. When a refund succeeds, the status of the [Refund](https://docs.stripe.com/api/refunds/object.md) object transitions to `succeeded`. If a refund fails, the status transitions to `failed`, Stripe returns the amount to your Stripe balance, and you must arrange an alternative way to provide your customer with a refund.

### Disputes 

Customers have up to 120 calendar days from the date of purchase to file Afterpay disputes.

Afterpay payments can only be disputed once.

#### Dispute process 

1. **Dispute opened**: Afterpay opens a dispute. Stripe immediately withholds the disputed amount from your balance. You have 14 calendar days to submit evidence.
2. **Decision**: Afterpay reviews your evidence and issues a final decision within 30 calendar days of dispute creation.

#### Dispute notifications 

When a Afterpay dispute is opened, Stripe notifies you through:

- Email
- The [Stripe Dashboard](https://dashboard.stripe.com/disputes)
- A [charge.dispute.created](https://docs.stripe.com/api/events/types.md#event_types-charge.dispute.created) webhook event

The dispute reason is available in the Dashboard and in the `reason` field of the [Dispute object](https://docs.stripe.com/api/disputes/object.md). The evidence submission deadline is available in the Dashboard and in the `evidence_due_by` field of the Dispute object.

#### Responding to disputes 

You have 14 calendar days from dispute creation to submit evidence.

Stripe requests that you upload compelling evidence that you fulfilled the purchase order using the Stripe Dashboard. This evidence can include: a received return confirmation, the tracking ID, the shipping date, a record of purchase for intangible goods (such as an IP address or email receipt), or a record of purchase for services or physical goods (such as a phone number or proof of receipt).

You can submit evidence and manage Afterpay disputes in the [Dashboard](https://dashboard.stripe.com/disputes) or programmatically using the [Disputes API](https://docs.stripe.com/api/disputes/update.md). In the Dashboard, go to the **Needs Response** tab, click the disputed payment, then click **Counter dispute** to submit your evidence, or **Accept dispute** to accept the loss. To submit evidence through the API, upload supporting files using the [Files API](https://docs.stripe.com/api/files/create.md) and include the returned file IDs when you update the [Dispute Evidence object](https://docs.stripe.com/api/disputes/update.md).

#### Dispute outcomes 

If Afterpay resolves the dispute in your favor, Stripe returns the disputed amount to your Stripe balance.

If they rule in favor of the customer, the balance charge becomes permanent.

#### Fraud and liability 

Customers must authenticate Afterpay payments by logging into their Afterpay account. This requirement helps reduce the risk of fraud or unrecognized payments. Afterpay covers losses incurred from customer fraud or the inability to repay installments. However, Stripe might contact you on behalf of Afterpay and request to stop or pause a shipment before any losses are incurred. It’s important to comply promptly with these requests.

## Stripe product support 
Products:
  ✓ Checkout
  ✓ Payment Links
  ✓ Payment Element
  ✗ Express Checkout Element
  ✓ Mobile Payment Element
  ✓ Managed Payments
  ✗ Billing
  ✗ Invoicing
  ✓ Adaptive Pricing
  ✗ Customer Portal
  ✗ Radar
  ✗ Terminal
  ✓ Connect

APIs:
  ✓ PaymentIntents
  ✗ PaymentIntents with setup_future_usage
  ✗ SetupIntents
  ✓ CheckoutSessions
### Add Afterpay branding to your website 

Let your customers know you accept payments with Afterpay by including the [Payment Method Messaging Element](https://docs.stripe.com/elements/payment-method-messaging.md) on your product and cart pages.

Afterpay also provides static [visual assets and branding guidance](https://www.afterpay.com/retailer-resources). In AU, CA, NZ and the US, consumers know Afterpay as ‘Afterpay’. In the UK, they know it as ‘Clearpay’. Make sure you pick the right location (see the footer in the [Afterpay documentation](https://www.afterpay.com/retailer-resources)) so that you get the appropriate assets. For Clearpay, see the [UK assets and branding guidance](https://www.clearpay.co.uk/en-GB/retailer-resources).

## Customer experience 

#### 1. Select payment method

The customer selects Afterpay at checkout.

#### 2. Redirect to Afterpay

On desktop, the customer is redirected to Afterpay to create or log in to their account. On mobile, the customer is redirected to the Afterpay app if it is installed, otherwise to Afterpay's website.

#### 3. Select payment plan

The customer selects a payment plan and accepts the repayment terms. Customers have 3 hours to complete authorization.

#### 4. Payment Complete

The customer is redirected back to the checkout page once the payment is authorized. Afterpay collects repayment directly from the customer over time.

### Transaction identifiers 

After a customer completes an Afterpay payment, the transactions that appear on the customer’s bank or card statement show *AFTERPAY* along with the merchant statement descriptor.

Set a custom [statement descriptor](https://docs.stripe.com/payments/payment-intents.md#dynamic-statement-descriptor) on the PaymentIntent before confirming the payment. If you don’t set one, the descriptor defaults to the [account level statement descriptor](https://docs.stripe.com/get-started/account/statement-descriptors.md). For connect scenarios, learn more about how [statement descriptors are set with Connect](https://docs.stripe.com/connect/statement-descriptors.md).

After the payment completes, expand `latest_charge` on the PaymentIntent to retrieve the Afterpay order ID from `latest_charge.payment_method_details.afterpay_clearpay.order_id`. Share it with customers to help them locate the payment in their Afterpay account or when contacting Afterpay support.

## Enable Afterpay 

If you use our front-end products, you can enable Afterpay directly from your [payment method settings](https://dashboard.stripe.com/settings/payment_methods). Stripe then automatically determines the most relevant payment methods to display to your customers.

If your integration requires manually listing payment methods, learn how to [manually configure Afterpay as a payment](https://docs.stripe.com/payments/afterpay-clearpay/accept-a-payment.md).
