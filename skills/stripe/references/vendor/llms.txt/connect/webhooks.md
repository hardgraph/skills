# Connect webhooks

Learn how to use webhooks with Connect to be notified of Stripe activity.

Stripe uses *webhooks* (A webhook is a real-time push notification sent to your application as a JSON payload through HTTPS requests) to notify your application when an event happens in your account. All *Connect* (Connect is Stripe's solution for multi-party businesses, such as marketplace or software platforms, to route payments between sellers, customers, and other recipients) integrations should establish a [webhook endpoint](https://dashboard.stripe.com/account/webhooks) to listen for Connect events.

## Connect webhooks

Webhooks for Connect users listen for events in different scopes, depending on the source of the event:

- **Your account**: Most events triggered by resources that exist in your account. That includes most requests made using your API keys and without [authenticating as another Stripe account](https://docs.stripe.com/connect/authentication.md), such as:
  - `v2.core.account.*` events for v2 `Accounts` in your account
  - Events for `Customers` in your account
  - Events for direct charges on your account
  - Events for *indirect charges* (A charge type where customers transact directly with your platform instead of with a connected account. Indirect charges include destination charges and separate charges and transfers) on your account for your connected accounts
- **Connected accounts**: Events triggered by resources that exist in connected accounts and some resources that exist in your account, such as:
  - `v2.core.account.*` events for v2 `Accounts` representing customers and recipients of your connected accounts
  - v1 `account.updated` events for both v1 and v2 `Accounts` representing customers and recipients of your connected accounts
  - *Direct charges* (A charge type where customers transact directly with a connected account, which is always the merchant of record. With each payment, the connected account pays fees to Stripe and, optionally, to your platform) for customers of your connected accounts

> #### Event scopes for v2 Accounts representing connected accounts
> 
> v2 `Account` objects trigger both v1 and v2 `Events`, which can have different scopes. For events triggered by connected accounts, v2 `Events` use the **Your account** scope, while v1 `Events` use the **Connected accounts** scope, even when triggered by the same v2 `Account`.

When you create a webhook in [Workbench](https://dashboard.stripe.com/workbench/webhooks), assign the scope by setting **Events from** to **Your account** or **Connected accounts**. When you create a webhook using the API, assign the scope by setting the [connect](https://docs.stripe.com/api/webhook_endpoints/create.md#create_webhook_endpoint-connect) parameter to false (for **Your account**) or true (for **Connected accounts**).

For Connect webhooks, your development webhook URLs receive only test webhooks, but your production webhook URLs receive both live and test webhooks. This is because you can perform both live and test transactions under a production application. We recommend that you check the `livemode` value when receiving an event webhook to determine whether users need to take action.

You must define separate webhook endpoints for your [sandbox](https://docs.stripe.com/sandboxes.md) accounts to receive events for those accounts.

Each event for a connected account contains a top-level `account` property that identifies the connected account. Because the connected account owns [the object that triggered the event](https://docs.stripe.com/api/events/object.md#event_object-data-object), you must make API requests for that object [as the connected account](https://docs.stripe.com/connect/authentication.md).

```json
{
  "id": ""{{EVENT_ID}}"",
  "livemode": true,
  "object": "event",
  "type": "customer.created",
  "account": ""{{CONNECTED_ACCOUNT_ID}}"",
  "pending_webhooks": 2,
  "created": 1349654313,
  "data": {...}
}
```

The following table describes some of the most common and important events related to connected accounts:

| Event | data.object type | Description |
| --- | --- | --- |
| `account.application.deauthorized` | `application` | Occurs when a connected account disconnects from your platform. You can use it to trigger cleanup on your server. Available for connected accounts with access to the Stripe Dashboard, which includes [Standard accounts](https://docs.stripe.com/connect/standard-accounts.md). |
| `account.external_account.updated` | An external account, such as `card` or `bank_account` | Occurs when [a bank account or debit card attached to a connected account is updated](https://docs.stripe.com/connect/payouts-bank-accounts.md), which can impact payouts. Available for connected accounts that your platform controls, which includes Custom and Express accounts, and Standard accounts with [platform controls](https://docs.stripe.com/connect/platform-controls-for-stripe-dashboard-accounts.md) enabled. |
| `account.updated` | `account` | Allows you to monitor changes to connected account requirements and status changes. Available for all connected accounts. |
| `balance.available` | `balance` | Occurs when your Stripe balance has been updated. For example, when [funds you’ve added from your bank account](https://docs.stripe.com/connect/top-ups.md) are available for transfer to your connected account. |
| `payment_intent.succeeded` | `payment_intent` | Occurs when a payment intent results in a successful charge. Available for all payments, including [destination](https://docs.stripe.com/connect/destination-charges.md) and [direct](https://docs.stripe.com/connect/direct-charges.md) charges. |
| `payout.failed` | `payout` | Occurs when [a payout fails](https://docs.stripe.com/connect/payouts-connected-accounts.md#webhooks). When a payout fails, the external account involved is disabled, and no automatic or manual payouts can be processed until the external account is updated. |
| `person.updated` | `person` | Occurs when a `Person` associated with the `Account` is updated. If you [use the Persons API to handle requirements](https://docs.stripe.com/connect/handling-api-verification.md#verification-process), listen for this event to monitor changes to requirements and status changes for individuals. Available for connected accounts that your platform controls, which includes Custom and Express accounts, and Standard accounts with [platform controls](https://docs.stripe.com/connect/platform-controls-for-stripe-dashboard-accounts.md) enabled. |

#### Event - account.application.deauthorized

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you're testing your webhook locally with the Stripe CLI, you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'account.application.deauthorized'
    application = event['data']['object']
    connected_account_id = event['account']
    handle_deauthorization(connected_account_id, application)
  end

  status 200
end

def handle_deauthorization(connected_account_id, application)
  # Clean up account state.
  puts 'Connected account ID: ' + connected_account_id
  puts application.to_s
end
```

#### Event - account.updated

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you're testing your webhook locally with the Stripe CLI, you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'account.updated'
    account = event['data']['object']
    handle_account_update(account)
  end

  status 200
end

def handle_account_update(account)
  # Collect more required information
  puts account.to_s
end
```

#### Event - person.updated

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you're testing your webhook locally with the Stripe CLI, you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'person.updated'
    person = event['data']['object']
    connected_account_id = event['account']
    handle_person_update(connected_account_id, person)
  end

  status 200
end

def handle_person_update(connected_account_id, person)
  # Collect more required information
  puts 'Connected account ID: ' + connected_account_id
  puts person.to_s
end
```

#### Event - payment_intent.succeeded, direct charge

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you are testing your webhook locally with the Stripe CLI you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'payment_intent.succeeded'
    payment_intent = event['data']['object']
    connected_account_id = event['account']
    handle_successful_payment_intent(connected_account_id, payment_intent)
  end

  status 200
end

def handle_successful_payment_intent(connected_account_id, payment_intent)
  # Fulfill the purchase
  puts 'Connected account ID: ' + connected_account_id
  puts payment_intent.to_s
end
```

#### Event - payment_intent.succeeded, non-direct charge

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you are testing your webhook locally with the Stripe CLI you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'payment_intent.succeeded'
    payment_intent = event['data']['object']
    handle_successful_payment_intent(payment_intent)
  end

  status 200
end

def handle_successful_payment_intent(payment_intent)
  # Fulfill the purchase
  puts payment_intent.to_s
end
```

#### Event - balance.available

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you're testing your webhook locally with the Stripe CLI, you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'balance.available'
    balance = event['data']['object']
    handle_available_balance(balance)
  end

  status 200
end

def handle_available_balance(balance)
  # Transfer funds to a connected account
  puts balance.to_s
end
```

#### Event - account.external_account.updated

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you're testing your webhook locally with the Stripe CLI, you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'account.external_account.updated'
    external_account = event['data']['object']
    connected_account_id = event['account']
    handle_external_account_update(connected_account_id, external_account)
  end

  status 200
end

def handle_external_account_update(connected_account_id, external_account)
  # Transfer funds to a connected account
  puts 'Connected account ID: ' + connected_account_id
  puts external_account.to_s
end
```

#### Event - payout.failed

#### Ruby

```ruby
# Using Sinatra.
require 'sinatra'
require 'stripe'

set :port, 4242

# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
# Find your keys at https://dashboard.stripe.com/apikeys.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>')

# If you're testing your webhook locally with the Stripe CLI, you
# can find the endpoint's secret by running `stripe listen`
# Otherwise, find your endpoint's secret in your webhook settings in
# the Developer Dashboard
endpoint_secret = 'whsec_...'

post '/webhook' do
  payload = request.body.read
  sig_header = request.env['HTTP_STRIPE_SIGNATURE']

  event = nil

  # Verify webhook signature and extract the event.
  # See https://stripe.com/docs/webhooks#verify-events for more information.
  begin
    event = Stripe::Webhook.construct_event(
      payload, sig_header, endpoint_secret
    )
  rescue JSON::ParserError => e
    # Invalid payload.
    status 400
    return
  rescue Stripe::SignatureVerificationError => e
    # Invalid Signature.
    status 400
    return
  end

  if event['type'] == 'payout.failed'
    payout = event['data']['object']
    connected_account_id = event['account']
    handle_failed_payout(connected_account_id, payout)
  end

  status 200
end

def handle_failed_payout(connected_account_id, payout)
  # Re-collect bank account required information
  puts 'Connected account ID: ' + connected_account_id
  puts payout.to_s
end
```

## Test webhooks locally

To test event webhooks locally with the Stripe CLI:

1. If you haven’t already, [install the Stripe CLI](https://docs.stripe.com/stripe-cli/install.md) on your machine.

2. Log in to your Stripe account and set up the CLI by running `stripe login` on the command line.

3. Allow your local host to receive a simulated event by running [stripe listen](https://docs.stripe.com/cli/listen), depending on the scope of the event:

   - **Connected accounts**: `stripe listen --forward-connect-to localhost:{{PORT}}/{{CONNECT_WEBHOOK_ENDPOINT}}`
   - **Your account**: `stripe listen --forward-to localhost:{{PORT}}/{{WEBHOOK_ENDPOINT}}`

4. In another terminal window, trigger a simulated event by running [stripe trigger](https://docs.stripe.com/cli/trigger), depending on the scope of the event:

   - **Connected accounts**: `stripe trigger --stripe-account {{CONNECTED_ACCOUNT_ID}} {{EVENT_NAME}}`
   - **Your account**: `stripe trigger {{EVENT_NAME}}`

## See also

- [Webhook documentation](https://docs.stripe.com/webhooks.md)
- [Event object reference](https://docs.stripe.com/api.md#events)
