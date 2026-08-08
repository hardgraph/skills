> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Billing overview

> How Retell billing works: prepaid credits and auto recharge on credit-based accounts, end-of-month billing on legacy accounts, plus usage tracking and invoices.

The Billing tab in your dashboard is where you manage payments, buy credits, track usage, and download invoices. Retell has two billing versions, and this page covers both.

## Which billing version am I on?

Check the Billing tab to see which version applies to your workspace:

* **Credit-based billing** — your Billing page shows a **Credits Balance** with **Buy credits** and **Auto recharge** buttons. You pay for usage upfront with prepaid credits. This applies to newer accounts.
* **Monthly billing (legacy)** — your Billing page shows only monthly invoices, with no credit balance. Usage is billed at the end of each period. This applies to accounts created before credit-based billing was introduced.

<Note>
  Billing is scoped **per workspace**, not per account. Every workspace, including new ones you create later, has its own payment method, credit balance, invoices, and usage totals. Adding a card in one workspace does not carry it over to another workspace under the same login: switch into each new workspace and add a payment method there before running calls. See [Create and manage Retell workspaces](/accounts/workspace) for how to switch workspaces.
</Note>

Choose your billing version below.

<Tabs>
  <Tab title="Credit-based billing">
    ## How credit-based billing works

    <Frame caption="Billing page on a credit-based account, showing the Credits Balance with the Buy credits and Auto recharge buttons">
      <img src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/images/billing-page/billing-credit-overview.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=2290ca7facfde0c4b2d2006235aab9e3" alt="Billing page on a credit-based account showing a Credits Balance of $100 with Buy credits and Auto recharge buttons in the top right" width="2267" height="196" data-path="images/billing-page/billing-credit-overview.png" />
    </Frame>

    Credit-based accounts pay for usage with a prepaid credit balance:

    * **Usage costs** — everything metered, such as per-minute call costs (voice, LLM, telephony) — are deducted from your credit balance in real time.
    * **Subscription items** — phone numbers, knowledge bases beyond the free tier, calls per second (CPS), and purchased [concurrency](/deploy/concurrency) — are not paid with credits. They are billed to your payment method on file at the end of each billing cycle.

    The billing cycle runs from the 1st of the month to the end of the month. Subscription items purchased mid-month are prorated for the portion of the month they were active.

    New accounts start with **\$10 in free trial credits** so you can test Retell before adding a payment method.

    <Warning>
      When your credit balance reaches zero, new calls are blocked until you buy credits or auto recharge tops up your balance. Set up auto recharge to avoid interruptions.
    </Warning>

    ## Buy credits

    <Steps>
      <Step title="Open the Billing tab">
        Go to the **Billing** tab in your dashboard and click **Buy credits**.
      </Step>

      <Step title="Enter an amount and purchase">
        Enter the amount and click **Purchase**. The charge goes to your payment method on file, processed securely through Stripe (see [Add payment methods](/accounts/add-payment)).

        <Frame caption="Buy Credits dialog: enter an amount and click Purchase">
          <img height="400" src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/images/billing-page/billing-buy-credits.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=23fe9f4ef92254d7af35d655a30ace3c" alt="Buy Credits dialog with a dollar amount field set to 100 and Cancel and Purchase buttons" data-path="images/billing-page/billing-buy-credits.png" />
        </Frame>
      </Step>
    </Steps>

    Credits never expire, but they are non-refundable once purchased.

    ## Set up auto recharge

    Auto recharge buys credits automatically when your balance runs low, so calls are never blocked:

    <Steps>
      <Step title="Open auto recharge settings">
        Go to the **Billing** tab, click **Auto recharge**, and toggle **Auto Recharge** on.
      </Step>

      <Step title="Set the threshold and target">
        Set **When credits drop below** (the balance that triggers a recharge) and **Bring credits back to** (the balance restored on each recharge), then click **Save**.

        <Frame caption="Auto Recharge Setting dialog: set the trigger threshold and target balance, then save">
          <img height="400" src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/images/billing-page/billing-auto-recharge.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=9eb18e850f7d4767931bdb6366a97510" alt="Auto Recharge Setting dialog with the toggle on, When credits drop below set to $10, and Bring credits back to set to $100" data-path="images/billing-page/billing-auto-recharge.png" />
        </Frame>
      </Step>
    </Steps>

    Each recharge is charged to your payment method on file. If a recharge payment fails, follow [Handle failed payments](/accounts/fail-payment).

    ## Subscription items and invoices

    At the end of each calendar month, you receive an invoice for subscription items:

    * Phone numbers [purchased through Retell](/deploy/purchase-number)
    * Knowledge bases beyond the free tier
    * Calls per second (CPS) upgrades
    * Purchased [concurrency](/deploy/concurrency)

    These are charged to your payment method on file and prorated if purchased mid-cycle. Download invoices from the Billing page by clicking the "Invoice" button next to the respective period.

    ## Stop charges or close your account

    To stop accruing charges on a credit-based account:

    1. **Turn off auto recharge** so your balance is not topped up automatically.
    2. **Release any phone numbers you own.** Numbers are billed monthly until released. Remove the number from the Phone Numbers page, or call the [Delete Phone Number API](/api-references/delete-phone-number).
    3. **Delete knowledge bases beyond the free tier** and remove any CPS or concurrency upgrades. These are billed monthly until removed.
    4. **Settle any outstanding invoices** on the Billing page.

    Remaining credits are non-refundable, so spend down your balance before closing your account. To close your account entirely, follow [Delete your account](/accounts/account#delete-your-account) after completing the steps above.
  </Tab>

  <Tab title="Monthly billing (legacy accounts)">
    ## Billing overview

    The Billing tab allows you to manage payments, track expenses, and download invoices:

    1. Payment management: We use Stripe for secure and reliable payment processing. Update your payment methods by clicking the "Change payment methods" button.
    2. Billing history: Review your monthly expenses, including cost breakdowns by category (e.g., Voice Infra, LLM).
    3. Invoices: Download invoices by clicking the "Invoice" button next to the respective period.
    4. Current charges: View ongoing costs for the current billing period, including itemized amounts and usage details.

    <Note>
      Legacy accounts use **post-usage billing**, not a prepaid credit balance. You are charged at the end of each billing period based on actual usage on the payment method you have on file. There is no manual top-up or auto recharge to configure. Make sure a valid payment method is added (see [Add payment methods](/accounts/add-payment)) to avoid service interruptions. If a charge fails, follow [Handle failed payments](/accounts/fail-payment).
    </Note>

    <Frame caption="Billing page on a legacy account, with the Change payment methods button and per-period Invoice download buttons">
      <img height="700" src="https://mintcdn.com/retellai/YTmPqPUaDmLakDGZ/images/billing-page/billing.png?fit=max&auto=format&n=YTmPqPUaDmLakDGZ&q=85&s=373dad2b36aad679703c2a41b1da7d0b" alt="Billing page on a legacy account showing billing history with Change payment methods and Invoice buttons" data-path="images/billing-page/billing.png" />
    </Frame>

    ## Stop charges or close your account

    Because legacy accounts use post-usage billing, there is no recurring subscription to cancel and no fixed monthly fee on the pay-as-you-go plan. You are only charged for usage you actually incur (call minutes, phone numbers, knowledge bases, and other line items shown on the Billing page).

    To stop accruing charges:

    1. **Stop running calls.** Once no calls are made, no usage charges accrue for the next billing period.
    2. **Release any phone numbers you own.** Numbers purchased through Retell are billed monthly until released. Remove the number from the Phone Numbers page in the dashboard, or call the [Delete Phone Number API](/api-references/delete-phone-number).
    3. **Delete knowledge bases beyond the free tier.** Additional knowledge bases are billed monthly until deleted.
    4. **Settle any outstanding invoices** on the Billing page.

    If you also want to close your account entirely, follow [Delete your account](/accounts/account#delete-your-account) after completing the steps above. If you are on a custom/enterprise plan or need help confirming there are no remaining recurring charges, contact [Retell support](https://support.retellai.com/).
  </Tab>
</Tabs>

## View usage breakdown

The Usage tab on the Billing page provides a breakdown of your workspace's activity and costs, regardless of billing version:

1. Total cost: Your total expenses for the selected billing period.
2. Call minutes: The total number of call minutes used.
3. Average cost per minute: The average cost for each minute of calls.
4. Daily or weekly call costs, making it easy to identify high-cost periods and track spending trends over time.
5. Cost by provider: A breakdown of expenses across voice infra, large language models (LLMs), telephony services, and concurrency usage.

Certain call characteristics can adjust the billed duration; see [Exceptions to per-minute pricing](/accounts/billing-exceptions).

<Frame caption="Usage tab showing total cost, call minutes, average cost per minute, and cost by provider for the selected period">
  <img height="700" src="https://mintcdn.com/retellai/YTmPqPUaDmLakDGZ/images/billing-page/usage.png?fit=max&auto=format&n=YTmPqPUaDmLakDGZ&q=85&s=7009ba5cbcfa77975eef933bbfb9ea88" alt="Usage tab on the Billing page with total cost, call minutes, average cost per minute, and a cost-by-provider chart" data-path="images/billing-page/usage.png" />
</Frame>
