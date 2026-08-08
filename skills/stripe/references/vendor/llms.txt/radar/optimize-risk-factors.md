# Risk factors recommendations

Follow our recommendations to maximize the effectiveness of Stripe Radar.

Stripe *Radar* (Stripe Radar helps detect and block fraud for any type of business using machine learning that trains on data across millions of global companies. It’s built into Stripe and requires no additional setup to get started) uses AI models that evaluate many risk factors to distinguish fraudulent and legitimate payments. It computes some risk factors automatically, but many depend on the data your integration sends. When you provide more relevant data, fraud prevention improves across all [supported payment methods](https://docs.stripe.com/radar/supported-payment-methods.md). Try to collect enough information to support accurate risk assessment without overloading your checkout flow.

## Integrations and risk factor completeness 

Radar uses data from the Stripe network to detect and block fraudulent transactions across integrations. The integration you choose affects the completeness of the risk factors you send to Stripe—the more payment data you capture, the better Radar can detect and prevent fraud.

If your integration doesn’t send enough payment data, you can add *customer risk factors* (Customer risk factors refer to information such as user email, name, and billing address that are passed through the customer object in the API) (user email, name, and billing address) to the [Customer](https://docs.stripe.com/api/customers.md) object. You can also add *client risk factors* (Client risk factors refer to information such as IP address, User-Agent, and checkout URL that are used to help enhance fraud prevention) (IP address, user agent, and checkout URL) to the [PaymentIntent](https://docs.stripe.com/api/payment_intents/create.md#create_payment_intent-payment_method_data) object.

| Integration type | Risk factor completeness |
| --- | --- |
| [Payment Links](https://docs.stripe.com/payment-links.md) (Recommended) |  |
| [Checkout](https://docs.stripe.com/payments/checkout.md) (Recommended) |  |
| [Elements](https://docs.stripe.com/payments/elements.md) with customer risk factors (Recommended) |  |
| [Direct API](https://docs.stripe.com/api.md) integration with [Radar Sessions](https://docs.stripe.com/radar/radar-session.md) and customer risk factors. |  |
| Direct API integration with client and customer risk factors. |  |
| Direct API integration with client risk factors. |  |
| Direct API integration with customer risk factors. |  |
| Direct API integration with no additional risk factors. |  |

## Important risk factors to send to Stripe 

Include the following information with your payments to improve fraud detection. Our recommended integrations automatically collect this data for you, while direct integrations typically require you to send it explicitly.

| Data | Estimated fraud model improvement |
| --- | --- |
| *Advanced risk factors* (Advanced risk factors refer to device characteristics and activity indicators that are automatically captured by Stripe.js and our SDKs. You can also capture advanced risk factors with Radar Sessions) | 36% |
| IP address | 12% |
| Customer email | 11% |
| Customer name | 3% |
| Billing address | 1% |

## Best practices 

To make sure that your conversion rate remains high while maximizing the performance of our AI models, adhere to the following best practices.

### Collect advanced risk factors 

Stripe Payment Links, Checkout, Elements, and our mobile SDKs automatically collect important high-risk factor data, such as device information and IP addresses. If you’re not using one of our recommended payment integrations, consider using [Radar Sessions](https://docs.stripe.com/radar/radar-session.md) to automatically collect [advanced risk factors](https://docs.stripe.com/disputes/prevention/advanced-fraud-detection.md).

### Create payments using the Customer object 

Use [Customer objects](https://docs.stripe.com/api.md#customers) when creating payments to let Stripe track the payment patterns for each one of your customers over time. This increases our ability to identify irregularities in purchasing behavior. To do this:

- [Set up payment methods for future use](https://docs.stripe.com/payments/save-and-reuse.md) and add a [billing address](https://docs.stripe.com/api/customers/object.md#customer_object-address) to `Customer` objects, using them to create subsequent payments.
- Provide your customer’s [email address](https://docs.stripe.com/api.md#customer_object-email) when creating a `Customer` object.
- Provide your customer’s [name](https://docs.stripe.com/api/.md#customer_object-name) when you tokenize their payment information.
- Collect the customer’s [shipping address](https://docs.stripe.com/api.md#customer_object-shipping), saving it to their associated `Customer` object if you ship physical goods.

Each `Customer` object can also store multiple payment methods, enhancing the checkout flow by letting your customers save multiple payment methods. Stripe continues to track payment patterns for each customer, regardless of which one they use. This cross-payment-method history can help with detecting fraud, even when a customer switches to a different payment method.

If you’re manually creating a [PaymentIntent](https://docs.stripe.com/api/payment_intents.md), make sure to handle [declines](https://docs.stripe.com/declines.md). If you reuse the PaymentIntent, track repeated attempts to help counter [card testing](https://docs.stripe.com/disputes/prevention/card-testing.md).

### Include Stripe.js 

Include [Stripe.js](https://docs.stripe.com/payments/elements.md) on every page of your site, not just the checkout page where your customer enters their payment information. This helps Stripe detect anomalous behavior as customers browse, and provides [additional risk factors](https://docs.stripe.com/disputes/prevention/advanced-fraud-detection.md) that can help improve fraud detection.

```html
<script async src="https://js.stripe.com/dahlia/stripe.js"></script>
```

Always load Stripe.js directly from **https://js.stripe.com/dahlia/stripe.js**. We don’t support using a local copy of Stripe.js, as it can result in user-visible errors, and reduces the effectiveness of our fraud detection.

### Update your privacy policy 

Radar collects information on anomalous device or user behavior that might be indicative of fraud. Make sure that your own privacy policy tells your customers about this type of collection. If your policy doesn’t include such a disclosure, consider adding the following paragraph:

> We use Stripe for payment, analytics, and other business services. Stripe collects identifying information about the devices that connect to its services. Stripe uses this information to operate and improve the services it provides to us, including for fraud detection. You can learn more about Stripe and read its privacy policy at **https://stripe.com/privacy**.

### Enable Radar for future use 

Radar operates on a per-charge level, which means that during a [PaymentIntent lifecycle](https://docs.stripe.com/payments/paymentintents/lifecycle.md), Radar might scan multiple charges if the payment has retries. By default, Radar doesn’t scan if you set up a Payment Method for [future use](https://docs.stripe.com/payments/save-and-reuse.md) *without* a charge. If you want to scan [SetupIntents](https://docs.stripe.com/api/setup_intents.md), go to your [Radar settings](https://dashboard.stripe.com/settings/radar) and enable **Use Radar on payment methods saved for future use**.
