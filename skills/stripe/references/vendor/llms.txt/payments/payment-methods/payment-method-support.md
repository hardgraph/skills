# Payment method support

Learn how your integration choices affect payment method support.

[Link](https://docs.stripe.com/payments/link.md) is a payment method network. With Link, users save their payment details one time, and can use them for future purchases at any business on the Link network.

Payment methods support certain currencies, countries, products, and API options. Make sure your chosen payment methods work for your scenario before choosing an [integration option](https://docs.stripe.com/payments/payment-methods/integration-options.md) by viewing these tables:

- [Country and currency support](https://docs.stripe.com/payments/payment-methods/payment-method-support.md#country-currency-support)
- [Product support](https://docs.stripe.com/payments/payment-methods/payment-method-support.md#product-support)
- [API support](https://docs.stripe.com/payments/payment-methods/payment-method-support.md#additional-api-supportability)

All payment methods have specific requirements for their use and might contain additional restrictions that you must comply with, such as marketing guidelines, additional prohibited and restricted businesses, and information about handling disputes and refunds. These usage requirements and restrictions are described in the [documentation](https://docs.stripe.com/payments/payment-methods/overview.md) for that payment method or in the applicable [payment terms](https://stripe.com/payment-terms/legal).

## Troubleshoot a payment method

If a customer doesn’t see a specific payment method, follow [these steps](https://support.stripe.com/questions/troubleshoot-missing-payment-methods) to troubleshoot the issue using the [Dashboard payment method troubleshooting tool](https://dashboard.stripe.com/test/settings/payment_methods/review).

## Country and currency support 

Refer to the following table to see where each [payment method](https://docs.stripe.com/api/payment_methods/object.md#payment_method_object-type) is supported and what presentment currencies it accepts. This table contains all of the supported currencies and countries for a given payment method. In some cases, not all of the countries listed can accept payments in all of the listed presentment currencies. For more details on exactly what currencies are accepted, see the individual payment method’s page.

> #### Connected accounts
> 
> If you have a platform or marketplace integration that uses Connect, your connected accounts might have different eligibility requirements than your account. To learn about connected account eligibility and capabilities, see [Adding payment method capabilities](https://docs.stripe.com/connect/payment-methods.md).

| Payment method | Currencies | Business location | Customer country |
| --- | --- | --- | --- |
| [ACH Direct Debit](https://docs.stripe.com/payments/ach-direct-debit.md) | USD | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GI, GR, HU, IE, IT, LV, LI, LT, LU, MT, NL, NO, PL, PT, RO, SK, SI, ES, SE, CH, GB, US | US |
| [Affirm](https://docs.stripe.com/payments/affirm.md) | CAD, USD | CA, US | CA, US |
| [Afterpay (Clearpay)](https://docs.stripe.com/payments/afterpay-clearpay.md) | AUD, CAD, EUR, NZD, GBP, USD | AU, CA, NZ, GB, US | AU, CA, NZ, GB, US |
| [Alipay](https://docs.stripe.com/payments/alipay.md) | AUD, CAD, CNY, EUR, GBP, HKD, JPY, MYR, NZD, SGD, USD | European Union, AU, CA, HK, JP, NZ, SG, US | Chinese consumers, overseas Chinese, and Chinese travelers |
| [Alma](https://docs.stripe.com/payments/alma.md) | EUR | FR | FR |
| [Amazon Pay](https://docs.stripe.com/payments/amazon-pay.md) | USD | AT, BE, CY, DK, FR, DE, HU, IE, IT, LU, NL, PT, ES, SE, CH, GB, US | Worldwide |
| [Apple Pay](https://docs.stripe.com/apple-pay.md) | Most currencies | [Apple-supported locations](https://support.apple.com/en-us/HT204916) | [Apple-supported locations](https://support.apple.com/en-us/HT204916) |
| [Bacs Direct Debit](https://docs.stripe.com/payments/bacs-debit/accept-a-payment.md) | GBP | GB | GB |
| [Bancontact](https://docs.stripe.com/payments/bancontact.md) | EUR | European Union, AU, HK, JP, MX, NZ, SG, US | BE |
| [Bank transfers](https://docs.stripe.com/payments/bank-transfers.md) | EUR, GBP, JPY, MXN, USD | European Union, JP, MX, GB, US | European Union, JP, MX, GB, US |
| [BECS direct debit](https://docs.stripe.com/payments/au-becs-debit.md) | AUD | AU | AU |
| [BLIK](https://docs.stripe.com/payments/blik.md) | PLN | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GR, HU, IS, IE, IT, LV, LI, LT, LU, MT, NL, NO, PL, PT, RO, SK, SI, ES, SE | PL |
| [Boleto](https://docs.stripe.com/payments/boleto.md) | BRL | BR | BR |
| [Cards](https://docs.stripe.com/payments/cards/overview.md) | Most currencies | Most locations | Most locations |
| [Cash App Pay](https://docs.stripe.com/payments/cash-app-pay.md) | USD | US | US customers (excluding accounts in US territories) |
| [EPS](https://docs.stripe.com/payments/eps.md) | EUR | European Union, AU, AT, CA, HK, JP, MX, NZ, SG, US | AT |
| [FPX](https://docs.stripe.com/payments/fpx.md) | MYR | MY | MY |
| [Google Pay](https://docs.stripe.com/google-pay.md) | Most currencies | [Google-supported locations](https://support.google.com/googlepay/answer/12429287?hl=en#zippy=) | [Google-supported locations](https://support.google.com/googlepay/answer/12429287?hl=en#zippy=) |
| [GrabPay](https://docs.stripe.com/payments/grabpay.md) | MYR, SGD | MY, SG | MY, SG |
| [iDEAL](https://docs.stripe.com/payments/ideal.md) | EUR | European Union, AU, CA, HK, JP, MX, NZ, SG, US | NL |
| [Klarna](https://docs.stripe.com/payments/klarna.md) | AUD, CAD, CHF, CZK, DKK, EUR, GBP, NOK, NZD, PLN, RON, SEK, USD | AU, AT, BE, CA, HR, CY, DK, EE, FI, FR, DE, GR, IE, IT, LV, LT, LU, MT, NL, NZ, NO, PL, RO, SK, SI, ES, SE, CH, GB, US | AU, AT, BE, CA, FI, FR, DE, CH |
| [Konbini](https://docs.stripe.com/payments/konbini.md) | JPY | JP | JP |
| [Link](https://docs.stripe.com/payments/link.md) | Most currencies | AU, AT, BE, BG, CA, HR, CY, CZ, DK, EE, FI, FR, DE, GI, GR, HK, HU, IE, IT, JP, LV, LI, LT, LU, MY, MT, MX, NL, NZ, NO, PL, PT, RO, SG, SK, SI, ES, SE, CH, AE, GB, US | Most locations |
| [MB WAY](https://docs.stripe.com/payments/mb-way.md) | EUR | AU, AT, BE, BG, CA, HR, CY, CZ, DK, EE, FI, FR, DE, GI, GR, HK, HU, IE, IT, JP, LV, LI, LT, LU, MT, MX, NL, NZ, NO, PL, PT, RO, SG, SK, SI, ES, SE, CH, GB, US | PT |
| [MobilePay](https://docs.stripe.com/payments/mobilepay.md) | DKK, EUR, NOK, SEK | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GR, HU, IE, IT, LV, LI, LT, LU, MT, NL, NO, PL, PT, RO, SK, SI, ES, SE | DK, FI |
| [Multibanco](https://docs.stripe.com/payments/multibanco.md) | EUR | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GI, GR, HU, IE, IT, LI, LT, LU, LV, MT, NL, NO, PL, PT, RO, SK, SI, ES, SE, CH, GB, US | PT |
| [OXXO](https://docs.stripe.com/payments/oxxo.md) | MXN | MX | MX |
| [P24](https://docs.stripe.com/payments/p24.md) | EUR, PLN | European Union, AU, CA, HK, JP, MX, NZ, SG, US | PL |
| [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank.md) | GBP | GB | GB |
| [PayNow](https://docs.stripe.com/payments/paynow.md) | SGD | SG | SG |
| [PayPal](https://docs.stripe.com/payments/paypal.md) | AUD, CAD, CHF, CZK, DKK, EUR, GBP, HKD, NOK, NZD, PLN, SEK, SGD, USD | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GR, IE, IT, LV, LI, LT, LU, MT, NL, NO, PL, PT, RO, SK, SI, ES, SE, CH, GB | Worldwide |
| [PayPay](https://docs.stripe.com/payments/paypay.md) | JPY | JP | JP |
| [Pix](https://docs.stripe.com/payments/pix.md) | BRL | BR (Invite only), US | BR |
| [Pre-authorized debit in Canada](https://docs.stripe.com/payments/acss-debit.md) | CAD, USD | CA, US | CA |
| [PromptPay](https://docs.stripe.com/payments/promptpay.md) | THB | TH | TH |
| [SEPA debit](https://docs.stripe.com/payments/sepa-debit.md) | EUR | European Union, AU, CA, HK, JP, MX, NZ, SG, US | Europe |
| [Stablecoin payments](https://docs.stripe.com/payments/stablecoin-payments.md) | USD | US | Worldwide |
| [Swish](https://docs.stripe.com/payments/swish.md) | SEK | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GR, IE, IS, IT, LV, LI, LT, LU, NL, NO, PL, RO, SK, SI, ES, SE | SE |
| [TWINT](https://docs.stripe.com/payments/twint.md) | CHF | AT, BE, BG, HR, CY, CZ, DK, EE, FI, FR, DE, GI, GR, HU, IE, IS, IT, LV, LI, LT, LU, MT, MC, NL, NO, PL, PT, RO, SM, SK, SI, SE, ES, CH, GB | CH |
| [UPI](https://docs.stripe.com/payments/upi.md) | INR | AT, AU, BE, BG, CA, CH, CY, CZ, DE, DK, EE, ES, FI, FR, GB, GR, HR, HU, IE, IT, LI, LT, LU, LV, MT, NL, NO, PL, PT, RO, SE, SG, SI, SK, US | IN |
| [WeChat Pay](https://docs.stripe.com/payments/wechat-pay.md) | AUD, CAD, CHF (depending on business location), CNY, DKK, EUR, GBP, HKD, JPY, NOK, SEK, SGD, USD | European Union, AU, CA, HK, JP, SG, GB, US | Chinese consumers, overseas Chinese, and Chinese travelers |
| [Zip](https://docs.stripe.com/payments/zip.md) | AUD, USD (Invite only) | AU, US (Invite only) | AU, US (Invite only) |

## Product support  

To determine which payment methods each Stripe product supports, refer to the following tables:

> #### Checkout modes
> 
> Checkout has three modes: [payment](https://docs.stripe.com/payments/accept-a-payment.md?platform=web), [subscription](https://docs.stripe.com/billing/subscriptions/build-subscriptions.md?payment-ui=checkout&ui=stripe-hosted), and [setup](https://docs.stripe.com/payments/save-and-reuse.md?platform=web). Unless specified otherwise, the payment method is available in all modes.

### Bank debits product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Instant Bank Payments](https://docs.stripe.com/payments/link/instant-bank-payments.md) or [ACH Direct Debit](https://docs.stripe.com/payments/ach-direct-debit.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported |
| [Bacs Direct Debit](https://docs.stripe.com/payments/payment-methods/bacs-debit.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 1 | ✓ Supported | ✓ Supported |
| [Australia BECS Direct Debit](https://docs.stripe.com/payments/au-becs-debit.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |
| [New Zealand BECS Direct Debit](https://docs.stripe.com/payments/nz-bank-account.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |
| [Pre-authorized debit in Canada](https://docs.stripe.com/payments/acss-debit.md) | ✓ Supported | ✓ Supported 2 | - Unsupported | ✓ Supported 3,4 | - Unsupported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported |
| [SEPA Direct Debit](https://docs.stripe.com/payments/sepa-debit.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |

1 You can’t use the Payment Element to create SetupIntents for Bacs Direct Debit. Use Checkout in [setup mode](https://docs.stripe.com/payments/save-and-reuse.md?platform=checkout) instead.2 Not supported when using Checkout in subscription mode.3 Not supported when [collecting payment details before creating a PaymentIntent](https://docs.stripe.com/payments/accept-a-payment-deferred.md).4 Not supported when using [Elements with the Checkout Sessions API](https://docs.stripe.com/payments/quickstart-checkout-sessions.md).

[Contact us](https://support.stripe.com/contact) to request a new bank debit method.

### Bank redirects product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Bancontact](https://docs.stripe.com/payments/bancontact.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 7 | Invite only | - Unsupported |
| [BLIK](https://docs.stripe.com/payments/blik.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported 3,4 | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported |
| [EPS](https://docs.stripe.com/payments/eps.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 7 | Invite only | - Unsupported |
| [FPX](https://docs.stripe.com/payments/fpx.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 7 | ✓ Supported | - Unsupported |
| [iDEAL | Wero](https://docs.stripe.com/payments/ideal.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 5,6,7 | ✓ Supported 6 | - Unsupported |
| [P24](https://docs.stripe.com/payments/p24.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 7 | Invite only | - Unsupported |
| [TWINT](https://docs.stripe.com/payments/twint.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported |

1 Not supported when using Checkout in subscription mode.2 Not supported when using Checkout in setup mode.3 Not supported when [collecting payment details before creating a PaymentIntent](https://docs.stripe.com/payments/accept-a-payment-deferred.md).4 Not supported when using [Elements with the Checkout Sessions API](https://docs.stripe.com/payments/quickstart-checkout-sessions.md).5 Only supported when using [Checkout in subscription mode](https://docs.stripe.com/billing/subscriptions/ideal.md).6 Not supported when using `charge_automatically` for subscriptions or invoices. iDEAL | Wero is a [single use](https://docs.stripe.com/payments/payment-methods.md#usage) payment method where customers are required to [authenticate](https://docs.stripe.com/payments/payment-methods.md#customer-actions) each payment.7 Invoices and Subscriptions only support the [send_invoice](https://docs.stripe.com/api/invoices/object.md#invoice_object-collection_method) collection method.

[Contact us](https://support.stripe.com/contact) to request a new bank redirect payment method.

### Bank transfers product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Bank transfers | ✓ Supported | ✓ Supported 1,2 | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported | ✓ Supported | - Unsupported |

1 Not supported when using Checkout in subscription mode.2 Not supported when using Checkout in setup mode.

### Buy now, pay later product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) | [Terminal](https://docs.stripe.com/terminal.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Affirm](https://docs.stripe.com/payments/affirm.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 4 | ✓ Supported | - Unsupported | ✓ Supported |
| [Afterpay/Clearpay](https://docs.stripe.com/payments/afterpay-clearpay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | (Private preview) | - Unsupported | - Unsupported |
| [Alma](https://docs.stripe.com/payments/alma.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [Billie](https://docs.stripe.com/payments/billie.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [Klarna](https://docs.stripe.com/payments/klarna.md) | ✓ Supported | ✓ Supported 1 | ✓ Supported | ✓ Supported | ✓ Supported3 | ✓ Supported | ✓ Supported 4 | ✓ Supported | - Unsupported | (Private preview) |
| [Kriya](https://docs.stripe.com/payments/kriya.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [Mondu](https://docs.stripe.com/payments/mondu.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [Scalapay](https://docs.stripe.com/payments/scalapay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [SeQura](https://docs.stripe.com/payments/sequra.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [Sunbit](https://docs.stripe.com/payments/sunbit.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |
| [Zip](https://docs.stripe.com/payments/zip.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported |

1 Not supported when using Checkout in subscription mode.2 Not supported when using Checkout in setup mode.3 Not supported when saving payment details during payment (`setup_future_usage`).4 Invoices and Subscriptions only support the [send_invoice](https://docs.stripe.com/api/invoices/object.md#invoice_object-collection_method) collection method.

### Cards product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Cards](https://docs.stripe.com/payments/cards/overview.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |

### Link product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Link](https://docs.stripe.com/payments/link.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |

### Real-time payments product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) | [Terminal](https://docs.stripe.com/terminal.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank.md) | ✓ Supported | ✓ Supported 1 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported 3 | ✓ Supported 3 | - Unsupported | - Unsupported |
| [PayNow](https://docs.stripe.com/payments/paynow.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 3 | ✓ Supported 3 | - Unsupported | ✓ Supported |
| [PayTo](https://docs.stripe.com/payments/payto.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported |
| [PromptPay](https://docs.stripe.com/payments/promptpay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported 4 | ✓ Supported 3 | ✓ Supported 3 | - Unsupported | - Unsupported |
| [UPI](https://docs.stripe.com/payments/upi.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported |
| [Swish](https://docs.stripe.com/payments/swish.md) | ✓ Supported | ✓ Supported 1,2 | - Unsupported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 3 | - Unsupported | - Unsupported | - Unsupported |

1 Not supported when using Checkout in subscription mode.2 Not supported when using Checkout in setup mode.3 Invoices and Subscriptions only support the [send_invoice](https://docs.stripe.com/api/invoices/object.md#invoice_object-collection_method) collection method.4 Mobile Payment Element only supports PromptPay on iOS.

### Stablecoin payments product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Stablecoin Payments](https://docs.stripe.com/payments/stablecoin-payments.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | (Private preview) | ✓ Supported 1 | (Private preview) |

1 Supported with `send_invoice` [collection method](https://docs.stripe.com/billing/collection-method.md) subscriptions.

### Vouchers product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Boleto](https://docs.stripe.com/payments/boleto.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |
| [Konbini](https://docs.stripe.com/payments/konbini.md) | ✓ Supported 4 | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 3 | ✓ Supported 3 | - Unsupported |
| [Multibanco](https://docs.stripe.com/payments/multibanco.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported 3 | ✓ Supported 3 | - Unsupported |
| [OXXO](https://docs.stripe.com/payments/oxxo.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported |

1 Not supported when using Checkout in subscription mode.2 Not supported when using Checkout in setup mode.3 Invoices and Subscriptions only support the [send_invoice](https://docs.stripe.com/api/invoices/object.md#invoice_object-collection_method) collection method.4[Request an invite](https://support.stripe.com/contact/email?topic=payment_apis) to create charges [on behalf of](https://docs.stripe.com/connect/charges.md#on_behalf_of) other accounts.

### Wallets product support 

| Payment method | [Connect](https://docs.stripe.com/connect.md) | [Checkout](https://docs.stripe.com/payments/checkout.md) | [Payment Links](https://docs.stripe.com/payment-links.md) | [Payment Element](https://docs.stripe.com/payments/payment-element.md) | [Express Checkout Element](https://docs.stripe.com/elements/express-checkout-element.md) | [Mobile Payment Element](https://docs.stripe.com/payments/mobile.md) | [Subscriptions](https://docs.stripe.com/subscriptions.md) | [Invoicing](https://docs.stripe.com/invoicing.md) | [Customer Portal](https://docs.stripe.com/customer-management.md) | [Terminal](https://docs.stripe.com/terminal.md) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [Alipay](https://docs.stripe.com/payments/alipay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | Invite only | ✓ Supported | - Unsupported | N/A (online payments only) |
| [Amazon Pay](https://docs.stripe.com/payments/amazon-pay.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported 3 | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | N/A (online payments only) |
| [Apple Pay](https://docs.stripe.com/apple-pay.md)7 | ✓ Supported | ✓ Supported 5 | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |
| [Cash App Pay](https://docs.stripe.com/payments/cash-app-pay.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | N/A (online payments only) |
| [Google Pay](https://docs.stripe.com/google-pay.md)7 | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |
| [GrabPay](https://docs.stripe.com/payments/grabpay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported 4 | ✓ Supported | - Unsupported | N/A (online payments only) |
| [Link](https://docs.stripe.com/payments/wallets/link.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported 6 | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | N/A (online payments only) |
| [MB WAY](https://docs.stripe.com/payments/mb-way.md) | ✓ Supported | ✓ Supported 1,2,3 | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | N/A (online payments only) |
| [MobilePay](https://docs.stripe.com/payments/mobilepay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | N/A (online payments only) |
| [PayPal](https://docs.stripe.com/payments/paypal.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported 3 | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | N/A (online payments only) |
| [PayPay](https://docs.stripe.com/payments/paypay.md) | ✓ Supported 8 | ✓ Supported 1,2,3 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | N/A (online payments only) |
| [Revolut Pay](https://docs.stripe.com/payments/revolut-pay.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | N/A (online payments only) |
| [Samsung Pay](https://docs.stripe.com/payments/samsung-pay/accept-a-payment.md) | - Unsupported | ✓ Supported 1,2 | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | - Unsupported | ✓ Supported |
| [Satispay](https://docs.stripe.com/payments/satispay.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | N/A (online payments only) |
| [Stablecoins and crypto](https://docs.stripe.com/payments/stablecoin-payments.md) | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | Invite only | ✓ Supported | ✓ Supported | N/A (online payments only) |
| [Vipps](https://docs.stripe.com/payments/vipps.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | N/A (online payments only) |
| [WeChat Pay](https://docs.stripe.com/payments/wechat-pay.md) | ✓ Supported | ✓ Supported 1,2 | ✓ Supported | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported 4 | ✓ Supported 4 | - Unsupported | N/A (online payments only) |

1 Not supported when using Checkout in subscription mode.2 Not supported when using Checkout in setup mode.3 Not supported when saving payment details during payment (`setup_future_usage`).4 Invoices and Subscriptions only support the `send_invoice` [collection method](https://docs.stripe.com/api/invoices/object.md#invoice_object-collection_method).5 Checkout with [ui_mode](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-ui_mode) set to `embedded_page` supports only Safari version 17 or later and iOS version 17 or later.6 The Payment Element doesn’t support Link in Brazil or India.7 Stripe doesn’t display Apple Pay or Google Pay for IP addresses in India.8 [Request an invite](https://support.stripe.com/contact/email?topic=payment_apis) to use Connect.

## Additional API support 

To learn about payment method API support, refer to the following tables.

[Bank transfers](https://docs.stripe.com/payments/bank-transfers.md#connect) implemented through the `customer_balance` payment method type have limited support for the [on_behalf_of](https://docs.stripe.com/connect/charges.md#on_behalf_of) parameter on [Connect](https://docs.stripe.com/connect.md) charges, and that [PayPal](https://docs.stripe.com/payments/paypal.md#connect) doesn’t support `on_behalf_of`.

The following payment methods support `on_behalf_of` in private preview:

- [Alipay](https://docs.stripe.com/payments/alipay.md)
- [WeChat Pay](https://docs.stripe.com/payments/wechat-pay.md)

### Bank debits API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [ACH Direct Debit](https://docs.stripe.com/payments/ach-direct-debit.md) | `us_bank_account` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |
| [Bacs Direct debit](https://docs.stripe.com/payments/payment-methods/bacs-debit.md) | `bacs_debit` | ✓ Supported 3 | - Unsupported 4 | - Unsupported | ✓ Supported | No |
| [Australia BECS Direct Debit](https://docs.stripe.com/payments/au-becs-debit.md) | `au_becs_debit` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |
| [New Zealand BECS Direct Debit](https://docs.stripe.com/payments/nz-bank-account.md) | `nz_bank_account` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |
| [Pre-authorized debit in Canada](https://docs.stripe.com/payments/acss-debit.md)5 | `acss_debit` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |
| [SEPA debit](https://docs.stripe.com/payments/sepa-debit.md) | `sepa_debit` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.3 PaymentIntents support confirmation with Bacs Direct Debit payment methods when the [Mandate](https://docs.stripe.com/api/mandates.md) has been collected by a Stripe-owned flow such as [Checkout](https://docs.stripe.com/payments/checkout.md), [Payment Element](https://docs.stripe.com/payments/payment-element.md), and [Payment Links](https://docs.stripe.com/payment-links.md).4 You can create SetupIntents for Bacs Direct Debit through [Checkout](https://docs.stripe.com/payments/checkout.md) using [setup mode](https://docs.stripe.com/payments/save-and-reuse.md?platform=checkout).5 Pre-authorized debit in Canada doesn’t support the [deferred intent](https://docs.stripe.com/payments/accept-a-payment-deferred.md) creation integration path.

### Bank redirects API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Bancontact](https://docs.stripe.com/payments/bancontact.md) | `bancontact` | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported 3 | Yes |
| [BLIK](https://docs.stripe.com/payments/blik.md)4 | `blik` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |
| [EPS](https://docs.stripe.com/payments/eps.md) | `eps` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [FPX](https://docs.stripe.com/payments/fpx.md) | `fpx` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [iDEAL | Wero](https://docs.stripe.com/payments/ideal.md) | `ideal` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | Yes |
| [P24](https://docs.stripe.com/payments/p24.md) | `p24` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank.md) | `pay_by_bank` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [TWINT](https://docs.stripe.com/payments/twint.md) | `twint` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | Yes |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.3 Bancontact supports `setup_future_usage` by saving the customer’s bank details as a SEPA Direct Debit payment method.4 BLIK doesn’t support the [deferred intent](https://docs.stripe.com/payments/accept-a-payment-deferred.md) creation integration path.

### Bank transfers API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| Bank transfers | `customer_balance` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.

### Buy now, pay later API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Affirm](https://docs.stripe.com/payments/affirm.md) | `affirm` | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | Yes |
| [Afterpay/Clearpay](https://docs.stripe.com/payments/afterpay-clearpay.md) | `afterpay_clearpay` | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | Yes |
| [Alma](https://docs.stripe.com/payments/alma.md) | `alma` | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | Yes |
| [Billie](https://docs.stripe.com/payments/billie.md) | `billie` | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | Yes |
| [Klarna](https://docs.stripe.com/payments/klarna.md) | `klarna` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | Yes |
| [Kriya](https://docs.stripe.com/payments/kriya.md) | `kriya` | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | Yes |
| [Mondu](https://docs.stripe.com/payments/mondu.md) | `mondu` | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | Yes |
| [SeQura](https://docs.stripe.com/payments/sequra.md) | `sequra` | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported | Yes |
| [Sunbit](https://docs.stripe.com/payments/sunbit.md) | `sunbit` | ✓ Supported | - Unsupported | - Unsupported | ✓ Supported | Yes |
| [Zip](https://docs.stripe.com/payments/zip.md) | `zip` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.

### Cards API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| Cards | `card` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | No |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe redirects your customer after they complete the payment.

### Link API support

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Link](https://docs.stripe.com/payments/link.md) | `link` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | No |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe redirects your customer after they complete the payment.

### Real-time payments API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Pay by Bank](https://docs.stripe.com/payments/pay-by-bank.md) | `pay_by_bank` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [PayNow](https://docs.stripe.com/payments/paynow.md) | `paynow` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |
| [PayTo](https://docs.stripe.com/payments/payto.md) | `payto` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |
| [PromptPay](https://docs.stripe.com/payments/promptpay.md) | `promptpay` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |
| [UPI](https://docs.stripe.com/payments/upi.md) | `upi` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | Yes |
| [Swish](https://docs.stripe.com/payments/swish.md) | `swish` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.

### Stablecoin payments API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Stablecoin payments](https://docs.stripe.com/payments/stablecoin-payments.md) | `crypto` | ✓ Supported | (Private preview) | ✓ Supported | (Private preview) | Yes |

### Vouchers API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Boleto](https://docs.stripe.com/payments/boleto.md) | `boleto` | ✓ Supported | ✓ Supported | - Unsupported | ✓ Supported | No |
| [Konbini](https://docs.stripe.com/payments/konbini.md) | `konbini` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |
| [Multibanco](https://docs.stripe.com/payments/multibanco.md) | `multibanco` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |
| [OXXO](https://docs.stripe.com/payments/oxxo.md) | `oxxo` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.

### Wallets API support 

| Payment method | API enum | [PaymentIntents](https://docs.stripe.com/payments/payment-intents.md) | [SetupIntents](https://docs.stripe.com/payments/setup-intents.md) | [Manual capture](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method.md) | [Setup future usage](https://docs.stripe.com/payments/save-during-payment.md?platform=web&ui=elements)1 | Requires redirect2 |
| --- | --- | --- | --- | --- | --- | --- |
| [Alipay](https://docs.stripe.com/payments/alipay.md) | `alipay` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [Amazon Pay](https://docs.stripe.com/payments/amazon-pay.md) | `amazon_pay` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | Yes |
| [Apple Pay](https://docs.stripe.com/apple-pay.md) | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | No |
| [Cash App Pay](https://docs.stripe.com/payments/cash-app-pay.md) | `cashapp` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | Yes |
| [Google Pay](https://docs.stripe.com/google-pay.md) | - Unsupported | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | No |
| [GrabPay](https://docs.stripe.com/payments/grabpay.md) | `grabpay` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [Link](https://docs.stripe.com/payments/wallets/link.md) | `link` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | No |
| [MB WAY](https://docs.stripe.com/payments/mb-way.md) | `mb_way` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |
| [MobilePay](https://docs.stripe.com/payments/mobilepay.md) | `mobilepay` | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | Yes |
| [PayPal](https://docs.stripe.com/payments/paypal.md) | `paypal` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | Yes |
| [PayPay](https://docs.stripe.com/payments/paypay.md) | `paypay` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [Revolut Pay](https://docs.stripe.com/payments/revolut-pay.md) | `revolut_pay` | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported | Yes |
| [Samsung Pay](https://docs.stripe.com/payments/samsung-pay/accept-a-payment.md) | `samsung_pay` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [Secure Remote Commerce](https://docs.stripe.com/secure-remote-commerce.md) | A comma-separated list of accepted card brands | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | Yes |
| [Stablecoins and crypto](https://docs.stripe.com/payments/stablecoin-payments.md) | `crypto` | ✓ Supported | Invite only | - Unsupported | Invite only | Yes |
| [Vipps](https://docs.stripe.com/payments/vipps.md) | `vipps` | ✓ Supported | - Unsupported | ✓ Supported | - Unsupported | Yes |
| [WeChat Pay](https://docs.stripe.com/payments/wechat-pay.md) | `wechat_pay` | ✓ Supported | - Unsupported | - Unsupported | - Unsupported | No |

1 Cards and bank debit methods including SEPA debit, AU BECS direct debit, and ACSS debit support both `on_session` and `off_session` with [setup future usage](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-setup_future_usage). All other payment method types either don’t support `setup_future_usage` or only support `off_session`.2 Payment methods might require confirmation with [return_url](https://docs.stripe.com/api/payment_intents/confirm.md#confirm_payment_intent-return_url) to indicate where Stripe should redirect your customer after they complete the payment.

## Custom payment methods 

If you need a payment method not offered on this page, consider [custom payment methods](https://docs.stripe.com/payments/payment-methods/custom-payment-methods.md).

| Payment method | Connect | Checkout | Payment Links | Payment Element | Express Checkout Element | Mobile Payment Element | Subscriptions | Invoicing | Customer Portal |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Custom payment methods | ✓ Supported | ✓ Supported 1 | - Unsupported | ✓ Supported | ✓ Supported 2 | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |

1 Not supported when using [manual approval](https://docs.stripe.com/payments/custom/manual-approval.md) (`approval_method=manual`).

2 In private preview.
