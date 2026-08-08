# Risk insights

Understand risk factors and details about a particular payment.

Using its AI models, Stripe Radar determines the risk score and risk level for a payment and uses them to decide when to block or mark payments for review. The system evaluates hundreds of risk factors about each payment, using data from the Stripe network across millions of businesses. Risk insights shows you the risk factors that power Stripe Radar.

If you want to see more of Radar’s risk factors, click **Show all insights** from the risk insights section.
![](https://b.stripecdn.com/docs-statics-srv/assets/risk-insights-card.ef788006b7b5d6acbb4d237386a3c4ed.png)

Risk insights for payments

> We store risk insights data for up to 6 months. If a transaction is older than this, you won’t be able to access the risk insights interface.

If you don’t see the customer information or locations that you expect, check that your integration follows the Radar [best practices](https://docs.stripe.com/radar/optimize-risk-factors.md).

If your integration doesn’t provide important details such as the cardholder’s email address, IP address, or shipping address, Radar can’t compute all of the data it needs to accurately evaluate each payment.

Risk insights also includes information about the customer, such as matching the cardholder’s name with the provided email, and the success rate of transactions on the Stripe network associated with the email address. A low authorization rate might indicate suspicious behavior, because previous declines sometimes suggest past attempts at fraudulent transactions.

We also highlight geography-based information, including the billing, shipping, and IP address locations associated with this payment.

## Related payments 

You can also view the network of related payments, which includes any other payments made to your business using the same customer ID, IP address, or card number as the payment you’re currently viewing. This can help identify common fraud patterns, such as [card testing](https://docs.stripe.com/disputes/prevention/fraud-types.md#card-testing) (many different cards sharing a single IP address) or trial abuse (many “customers” share the same card).
![](https://b.stripecdn.com/docs-statics-srv/assets/related-payments-highlight.f0668ec4db4273e04eb4f8f3b8910e08.png)

Related payments

## See also

- [Review](https://docs.stripe.com/radar/transaction-reviews.md)
- [Integration checklist](https://docs.stripe.com/radar/optimize-risk-factors.md)
