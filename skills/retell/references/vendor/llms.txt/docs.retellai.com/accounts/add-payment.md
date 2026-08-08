> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add payment methods

> Add a Stripe payment method to your Retell workspace to buy credits, keep calling after the free trial, purchase phone numbers, and avoid service interruptions.

Adding a payment method is required before you can purchase phone numbers or use Retell services beyond the free trial. We use Stripe to securely process all payments.

Your payment method on file is used for:

* **Credit-based accounts** — purchasing credits, auto-recharge top-ups, and the end-of-month invoice for subscription items (phone numbers, knowledge bases, CPS, and concurrency).
* **Legacy monthly-billing accounts** — the end-of-period charge for your usage and recurring items.

See [Billing overview](/accounts/billing) to check which billing version your account uses.

<Note>
  Payment methods are scoped **per workspace**. Adding a card in one workspace does not carry it over to another workspace, even under the same login — each workspace has its own billing setup, invoices, and usage totals. If you create a second workspace, switch into it and repeat the steps below before starting calls there. See [Create and manage Retell workspaces](/accounts/workspace) for how to switch workspaces.
</Note>

## Adding Your Payment Method

<Steps>
  <Step title="Navigate to Billing">
    Go to the Billing tab in your dashboard

    <Frame caption="Access billing settings from your dashboard">
      <img height="700" src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/b1.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=2abd92af92600ecf0bbf8d4fff1f04e3" alt="Billing tab in the dashboard" data-path="images/b1.png" />
    </Frame>
  </Step>

  <Step title="Access Payment Settings">
    Click "Change payment methods" to open payment settings

    <Frame caption="Open payment method settings">
      <img height="700" src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/b2.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=2a59e5d7d4d78bc8b53508c76aa29d46" alt="Change payment methods button" data-path="images/b2.png" />
    </Frame>
  </Step>

  <Step title="Add New Payment Method">
    In the Stripe portal, click "Add payment method" and enter your payment details. Your payment information is securely handled by Stripe.

    <Frame caption="Add your payment details in Stripe">
      <img height="700" src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/b3.png?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=af4154efef98bddd76e5103938fdb6e7" alt="Stripe payment details form" data-path="images/b3.png" />
    </Frame>
  </Step>
</Steps>
