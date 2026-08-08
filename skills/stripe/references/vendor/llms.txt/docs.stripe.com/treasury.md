# Manage money with Stripe Treasury

Learn how to securely manage money with a financial account.
Available in: US, GB
With Stripe Treasury, you can securely store funds, open local accounts, [convert currencies](https://docs.stripe.com/instant-currency-conversion.md), send money, manage expenses, and [borrow money](https://docs.stripe.com/capital/how-stripe-capital-works.md) directly in the Stripe Dashboard.

Your financial account is on the [Balances](https://dashboard.stripe.com/balances) page in the Dashboard. On your Balances page you have a payments balance and a financial account, which serve different purposes. For eligible businesses, payment proceeds settle in your financial account.

| Balance type | Supported features |
| --- | --- |
| Payments balance | - Pay out earnings to your external bank account
- Track incoming transactions
- Manage transaction reconciliation
- Initiate refunds or handle disputes
- Instantly convert multi-currency settled funds |
| Financial account | - Add funds from an external bank account or transfer funds from your payments balance, and transfer funds back to your payments balance
- [Send money](https://docs.stripe.com/global-payouts.md) to anyone (using the Dashboard or API)
- Create a virtual or physical [card](https://docs.stripe.com/treasury/cards.md) to spend funds
- Instantly [convert currencies](https://docs.stripe.com/instant-currency-conversion.md)
- Receive, store, and send [stablecoins](https://docs.stripe.com/treasury/stablecoins.md) globally
- Access [business financing](https://docs.stripe.com/capital/overview.md)
- Integrate accounting software from the Stripe Dashboard with [Xero](https://marketplace.stripe.com/apps/xero) and [QuickBooks Sync by Acodei](https://marketplace.stripe.com/apps/quickbooks-sync-by-acodei) |

## Currency support 

Currency support differs depending on the location of your business or representative:

| Business and representative location | USD | USD stablecoins | GBP | EUR |
| --- | --- | --- | --- | --- |
| US legal entities with US representatives | ✓ Supported | ✓ Supported | ✓ Supported | ✓ Supported |
| US legal entities with international representatives | - Unsupported | ✓ Supported | - Unsupported | - Unsupported |
| UK legal entities with UK representatives | ✓ Supported | - Unsupported | ✓ Supported | ✓ Supported |
| International legal entities from [more than 100 countries](https://docs.stripe.com/treasury/stablecoins.md#regional-support) | - Unsupported | ✓ Supported | - Unsupported | - Unsupported |

## Add money to a financial account 

You can store funds in a financial account in the Dashboard. You can fund your financial account balance with external funds or from your Stripe payments balance. In the US, funds in your financial account balance are eligible for FDIC insurance up to 250,000 USD.

| Funding method | Funds available | Where to initiate |
| --- | --- | --- |
| Add money from your external bank account | 0–3 business days | From your bank |
| (US) Pull funds from your external bank account | 2–6 business days | Stripe Dashboard |
| Add money from your available Stripe payments balance | Immediately | Stripe Dashboard |
| [Add stablecoins](https://docs.stripe.com/treasury/stablecoins.md) through your preferred crypto network | Minutes | From your crypto wallet |

For pricing to fund your financial account, see the [Treasury pricing page](https://stripe.com/pricing#treasury).

### Add money from external bank account 

You can send funds from an external bank account to fund your financial account. Each currency in your financial account has its own local account details and supported inbound payment methods.

1. On the [Balances](https://dashboard.stripe.com/balances) page, click **Add funds**.
2. Select the financial account to add money to.
3. Select **Manually transfer from your bank** and click **Continue**.
4. Use the account details to send money through ACH, RTP, a wire, or other local payment methods from your bank.

### Add from a crypto wallet

1. On the [Balances](https://dashboard.stripe.com/balance/overview) overview, click **Add funds**.
2. Click **Crypto transfer**.
3. Select a crypto network.
4. Copy the wallet address or scan the QR code with your crypto wallet.
5. Click **Done**.

### Pull funds from an external bank account  (US only)

You can pull funds from an external bank account to fund your financial account. You initiate this transfer each time you want to add funds (it isn’t an automated, regular transaction).

You must use a verified bank account to add funds. Funds are typically available within 2–6 business days, and funding is subject to the following limits:

- 50,000 USD per day
- 100,000 USD per week

### Add or verify a bank account  (US only)

To add a verified bank account:

1. On the [Balances](https://dashboard.stripe.com/balances) page, click **Add funds**.
2. Select the financial account to add money to.
3. Select **Linked bank account** from payment method and click **Add new bank account**.

You have two options to add and verify your bank account:

- Instant with [Financial Connections](https://docs.stripe.com/financial-connections.md): Use your bank credentials with Financial Connections to verify and connect your bank account.
- Manually with micro-deposits: Manually enter your bank account and routing numbers. Stripe sends microdeposit amounts that appear on your online banking statement within 1–2 business days, with the `ACCTVERIFY` statement description. Use these amounts to verify your account in the Dashboard. This manual verification timing is separate to any settlement timing for your funding transaction.

### Pull funds into your financial account balance 

You can pull funds into your financial account directly from the Dashboard.

1. On the [Balances](https://dashboard.stripe.com/balances) page, click **Add funds**.
2. Select the financial account to add money to.
3. Enter the amount to add.
4. Select **Linked bank account**.
5. Choose the bank account you want to send money from.
6. Click **Continue** to review your selections, then click **Top up**.

### Transfer funds from Stripe payments balance (one time) 

You can use the available funds in your Stripe payments balance to fund your financial account. You can only transfer funds that have settled, so unsettled funds won’t be eligible for transfer.

Use the Stripe Dashboard to transfer money from your payments balance into your financial account.

1. On the [Balances](https://dashboard.stripe.com/balances) page, click **Transfer**.
2. Select the payments balance to transfer from.
3. Select the financial account to transfer to.
4. Enter the amount to move.
5. Click **Review** to review your selections, then click **Transfer**.

### Transfer funds from Stripe payments balance (recurring) 

You can set up a recurring transfer from your Stripe payments balance to your financial account. You can set a target amount to be transferred or you can transfer all of your earnings when they settle into your payments balance.

To configure a recurring transfer from your payments balance, you must be on an automatic payout schedule.

1. On the [Balances](https://dashboard.stripe.com/balances) page, click the **Payments balance** tile.
2. Click **Recurring transfers**.
3. Choose the currency that you want to set up.
4. Select whether you want to transfer a target amount or all of your payments balance.
5. Click **Save**.

## Transfer funds to your other Stripe balances 

You can transfer funds from your financial account to your Stripe payments balance. This lets you use funds stored in your financial account to fund refunds, disputes, and transfers to connected accounts.

1. On the [Balances](https://dashboard.stripe.com/balances) page, click **Transfer**.
2. For **From**, choose your financial account.
3. For **To**, select **Payments balance**.
4. Enter the amount to transfer.
5. Click **Continue** to review your selections, then click **Transfer**.

## Transfer money 

You can transfer money to your external bank account. There’s no fee to transfer funds to your bank accounts that are linked to your Stripe account.

1. On the [Balances](https://dashboard.stripe.com/balances) page, click **Transfer**.
2. Click **Pay out to external bank account**.
3. Choose the financial account to transfer from, and the bank account to transfer to.
4. Enter the amount to transfer.
5. Click **Review** to view the transfer details and cost breakdown, then click **Transfer**.

## View local account details 

You can share your local account details with others to receive one-time payments, such as investor funding, loans, or grants.

To find your local account details:

1. On the [Balances](https://dashboard.stripe.com/balances) page, click your financial account.
2. Click the relevant dropdown menu in the **Account details** section.

In this view, you can see the local account details for the selected currency.

## Add currencies 

Financial accounts support the following currencies:

- USD
- USDC (for US accounts)
- EUR
- GBP

To add different currencies to your financial account:

1. Go to **Settings** > **Business** > [Bank accounts and currencies](https://dashboard.stripe.com/settings/payouts).
2. In the **Manage financial account currencies** section, select the currencies to add to your financial account.
3. Click **Save**.

If you’re interested in USDC and don’t see it in the Dashboard, [request access](https://docs.stripe.com/treasury/stablecoins.md#request-access-privy).

## Debit your account (Private preview)
Available in: US
You can let external parties debit your financial account with ACH by sharing local account details for one-off payments.

To let others debit your financial account, find your local account details on the [Balances](https://dashboard.stripe.com/balances) page in the **Account details** section, and share them with the party initiating the debit.

When the debit settles, Stripe sends you an email notification that money was debited from your financial account. You can view the transaction on the [Balances](https://dashboard.stripe.com/balances) page.

### Request to join the preview for received debits.

Enter your email to request access.

```bash
curl https://docs.stripe.com/preview/register \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Referer: https://docs.stripe.com/treasury" \
  -d '{"email": "EMAIL", "preview": "received_debits_treasury_preview"}'
```

### Dispute a debit

If you don’t recognize a debit transaction, you can dispute it in the Dashboard within 1 business day of its settlement date.

To dispute a debit transaction:

1. On the [Balances](https://dashboard.stripe.com/balances) page, select the debit transaction.
2. Click **Dispute**.
3. Click **View dispute** to review the details of the disputed transaction.

To dispute a debit transaction later than 1 business day after its settlement date, [contact support at treasury-support@stripe.com](mailto:treasury-support@stripe.com).

## Financial accounts in the Dashboard mobile app 

You can access your financial account in the [Dashboard mobile app](https://docs.stripe.com/dashboard/mobile.md). The app is available for [iOS](https://apps.apple.com/app/apple-store/id978516833?pt=91215812&ct=stripe-docs-mobile&mt=8) and [Android](https://play.google.com/store/apps/details?id=com.stripe.android.dashboard&pli=1).

In the app, you can:

- View your balances
- View your transaction history
- View your financial account details
- View card details for your existing cards

## Request access

Enter your email address to learn about the following Treasury preview features:

- Expanded regional support
- Access to more currencies
- Stripe credits on Treasury balances (US)
- Interest and Financial Services Compensation Scheme (FSCS) protection on Treasury balances (UK)

### Request to join the preview for expanded Treasury features.

Enter your email to request access.

```bash
curl https://docs.stripe.com/preview/register \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Referer: https://docs.stripe.com/treasury" \
  -d '{"email": "EMAIL", "preview": "preview_features_treasury_preview"}'
```
