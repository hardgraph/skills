# Build a checkout page with the Checkout Sessions API

Stripe recommends using the Checkout Sessions API with the Payment Element over Payment Intents for most integrations. Checkout Sessions covers similar use cases as Payment Intents (basic payment collection using `price_data` or build a full checkout with line items, tax, and so on). Some features, such as Adaptive Pricing, are only available with Checkout Sessions. Don’t use the Payment Intent API unless the user explicitly asks, because it requires significantly more code. Learn more about Elements with the Checkout Sessions API: https://docs.stripe.com/payments/quickstart-checkout-sessions. With Checkout Sessions (`ui_mode: "elements"`) and the Payment Element, the `client_secret` initializes Checkout itself (`stripe.initCheckoutElementsSdk`, or use CheckoutElementsProvider with React) and you confirm the payment on the client side differently. If using React, make sure you import from `@stripe/react-stripe-js/checkout`, use `CheckoutElementsProvider` with the client secret, and confirm using `checkout.confirm`. If using HTML, make sure you use `checkout = stripe.initCheckoutElementsSdk({clientSecret: promise}); checkout.createPaymentElement(); paymentElement = checkout.createPaymentElement(); paymentElement.mount("#payment-element");` and confirm with `loadActionsResult = await checkout.loadActions(); actions = loadActionsResult.actions; error = await actions.confirm();`.

Coding agents should install the Stripe CLI (`npm i -g @stripe/cli`) and run the command `stripe sandbox create --help` to provision an anonymous Stripe sandbox with working API keys. No account registration required.

# Build a checkout page with Checkout Sessions API 

Build a checkout page on your website using Stripe Elements and the Checkout Sessions API, a front-end SDK that manages tax, discounts, shipping rates, and more.

### Install the Stripe Node library

Install the package and import it in your code. Alternatively, if you’re starting from scratch and need a package.json file, download the project files using the Download link in the code editor.

#### npm

Install the library:

```bash
npm install --save stripe
```

#### GitHub

Or download the stripe-node library source code directly [from GitHub](https://github.com/stripe/stripe-node).

### Install the Stripe Ruby library

Install the Stripe ruby gem and require it in your code. Alternatively, if you’re starting from scratch and need a Gemfile, download the project files using the link in the code editor.

#### Terminal

Install the gem:

```bash
gem install stripe
```

#### Bundler

Add this line to your Gemfile:

```bash
gem 'stripe'
```

#### GitHub

Or download the stripe-ruby gem source code directly [from GitHub](https://github.com/stripe/stripe-ruby).

### Install the Stripe Java library

Add the dependency to your build and import the library. Alternatively, if you’re starting from scratch and need a sample pom.xml file (for Maven), download the project files using the link in the code editor.

#### Maven

Add the following dependency to your POM and replace {VERSION} with the version number you want to use.

```bash
<dependency>\n<groupId>com.stripe</groupId>\n<artifactId>stripe-java</artifactId>\n<version>{VERSION}</version>\n</dependency>
```

#### Gradle

Add the dependency to your build.gradle file and replace {VERSION} with the version number you want to use.

```bash
implementation "com.stripe:stripe-java:{VERSION}"
```

#### GitHub

Download the JAR directly [from GitHub](https://github.com/stripe/stripe-java/releases/latest).

### Install the Stripe Python package

Install the Stripe package and import it in your code. Alternatively, if you’re starting from scratch and need a requirements.txt file, download the project files using the link in the code editor.

#### pip

Install the package through pip:

```bash
pip3 install stripe
```

#### GitHub

Download the stripe-python library source code directly [from GitHub](https://github.com/stripe/stripe-python).

### Install the Stripe PHP library

Install the library with composer and initialize with your secret API key. Alternatively, if you’re starting from scratch and need a composer.json file, download the files using the link in the code editor.

#### Composer

Install the library:

```bash
composer require stripe/stripe-php
```

#### GitHub

Or download the stripe-php library source code directly [from GitHub](https://github.com/stripe/stripe-php).

### Set up your server

Add the dependency to your build and import the library. Alternatively, if you’re starting from scratch and need a go.mod file, download the project files using the link in the code editor.

#### Go

Make sure to initialize with Go Modules:

```bash
go get -u github.com/stripe/stripe-go/v86
```

#### GitHub

Or download the stripe-go module source code directly [from GitHub](https://github.com/stripe/stripe-go).

### Install the Stripe.net library

Install the package with .NET or NuGet. Alternatively, if you’re starting from scratch, download the files which contains a configured .csproj file.

#### dotnet

Install the library:

```bash
dotnet add package Stripe.net
```

#### NuGet

Install the library:

```bash
Install-Package Stripe.net
```

#### GitHub

Or download the Stripe.net library source code directly [from GitHub](https://github.com/stripe/stripe-dotnet).

### Install the Stripe libraries

Install the packages and import them in your code. Alternatively, if you’re starting from scratch and need a `package.json` file, download the project files using the link in the code editor.

Install the libraries:

```bash
npm install --save stripe @stripe/stripe-js next
```

### Create a Checkout Session

Add an endpoint on your server that creates a *Checkout Session* (A Checkout Session represents your customer's session as they pay for one-time purchases or subscriptions through Checkout. After a successful payment, the Checkout Session contains a reference to the Customer, and either the successful PaymentIntent or an active Subscription), setting the [ui_mode](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-ui_mode) to `elements`. You must create the Checkout Session server-side because it requires your [secret or restricted API key](https://docs.stripe.com/keys.md#obtain-api-keys), which you can’t safely expose in client-side code.

The Checkout Session response includes a [client_secret](https://docs.stripe.com/api/checkout/sessions/object.md#checkout_session_object-client_secret), which the client uses to complete the payment. Return the client secret in your response.

### Supply a return URL

To define where Stripe redirects your customer after payment, specify the URL of the return page in the [return_url](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-return_url) parameter when creating the Checkout Session. After the payment attempt, Stripe redirects your customer to the return page hosted on your website.

Include the `{CHECKOUT_SESSION_ID}` template variable in the URL. Before redirecting your customer, Checkout replaces the variable with the Checkout Session ID. You’re responsible for creating and hosting the return page on your website.

### Define a product to sell

Define the [products and prices](https://docs.stripe.com/products-prices/manage-prices.md) for your Checkout Session. Typically, that means using a predefined product [Price ID](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-line_items-price). However, if you need dynamic pricing that can’t be known ahead of time, use [price_data](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-line_items-price_data) to define prices inline.

### Handle different transaction types

To handle different transaction types, adjust the [mode](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-mode) parameter:

- For one-time payments, use `payment`.
- If you have one or more [recurring prices](https://docs.stripe.com/payments/subscriptions.md), use `subscription`.
- If you aren’t collecting an initial payment from a customer but want to save their payment details to [charge them later](https://docs.stripe.com/payments/checkout/save-and-reuse.md?payment-ui=embedded-components), use `setup`.

### Add Stripe to your React app

To stay *PCI-compliant* (Any party involved in processing, transmitting, or storing credit card data must comply with the rules specified in the Payment Card Industry (PCI) Data Security Standards. PCI compliance is a shared responsibility and applies to both Stripe and your business) by ensuring that payment details go directly to Stripe and never reach your server, install [React Stripe.js](https://docs.stripe.com/sdks/stripejs-react.md).

```bash
npm install --save @stripe/react-stripe-js @stripe/stripe-js
```

### Load Stripe.js

To configure the Stripe library, call `loadStripe()` with your Stripe publishable API key.

### Load Stripe.js

Use *Stripe.js* (Use Stripe.js’ APIs to tokenize customer information, collect sensitive card data, and accept payments with browser payment APIs) to remain *PCI compliant* (Any party involved in processing, transmitting, or storing credit card data must comply with the rules specified in the Payment Card Industry (PCI) Data Security Standards. PCI compliance is a shared responsibility and applies to both Stripe and your business) by sending payment details directly to Stripe without hitting your server. Always load Stripe.js from *js.stripe.com* to remain compliant. Don’t include the script in a bundle or host it yourself.

You can load Stripe.js by including the script in your HTML file or using [loadStripe](https://github.com/stripe/stripe-js/blob/master/README.md#loadstripe).

### Define the payment form

To securely collect the customer’s information, create empty placeholder `div` elements. Stripe injects iframes into each `div`.

### Initialize Stripe.js

Initialize Stripe.js with your [publishable API key](https://docs.stripe.com/keys.md#obtain-api-keys).

### Fetch a Checkout Session client secret

Make a request to your server to [create a Checkout Session](https://docs.stripe.com/api/checkout/sessions/create.md) and retrieve the client secret.

### Initialize Checkout

Use `clientSecret` to initialize Checkout, passing a client secret string or a Promise that resolves to it. The [Checkout](https://docs.stripe.com/js/custom_checkout) object forms the backbone of your checkout page and contains data from the Checkout Session and methods to update it.

### Initialize Checkout

Render the [CheckoutElementsProvider](https://docs.stripe.com/js/react_stripe_js/checkout/checkout_provider), passing `clientSecret`—the client secret string or Promise that resolves to the client secret.

### Set up the state

Initialize some state to keep track of the payment, show errors, and manage the user interface.

### Store a reference to Checkout

Access the [checkout](https://docs.stripe.com/js/custom_checkout) object in your CheckoutForm component by using the `useCheckoutElements()` hook. The `checkout` object acts as the backbone of your checkout page, containing data from the Checkout Session and methods to update it.

### Collect the customer’s email address

Use the [Contact Details Element](https://docs.stripe.com/js/custom_checkout/create_contact_details_element) to collect your customer’s email address. It handles email collection and validation for you, and helps customers sign in to Link.

Alternatively, you can skip this step if you provide the customer’s email in one of the following ways:

- Pass in [customer_email](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_email) or [customer](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer) when creating the Checkout Session. Stripe validates emails provided this way.
  - These prefill the session with an email that customers can’t edit on the checkout page. If you want to prefill with an editable email, use [defaultValues.email](https://docs.stripe.com/js/custom_checkout/init#custom_checkout_init-options-defaultValues-email) when initializing Checkout.
- Pass in an email you already validated on [updateEmail](https://docs.stripe.com/js/custom_checkout/update_email) or [checkout.confirm](https://docs.stripe.com/js/custom_checkout/confirm).

### Collect the customer’s email address

Use the [ContactDetailsElement](https://docs.stripe.com/js/react_stripe_js/checkout/contact_details_element) to collect your customer’s email address. It handles email collection and validation for you, and helps customers sign in to Link.

Alternatively, you can skip this step if you provide the customer’s email in one of the following ways:

- Pass in [customer_email](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_email) or [customer](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer) when creating the Checkout Session. Stripe validates emails provided this way.
  - These prefill the session with an email that customers can’t edit on the checkout page. If you want to prefill with an editable email, use [defaultValues.email](https://docs.stripe.com/js/custom_checkout/init#custom_checkout_init-options-defaultValues-email) when initializing Checkout.
- Pass in an email you already validated on [updateEmail](https://docs.stripe.com/js/react_stripe_js/checkout/update_email) or [confirm](https://docs.stripe.com/js/react_stripe_js/checkout/confirm).

### Add the Payment Element

Add the [Payment Element](https://docs.stripe.com/js/custom_checkout/create_payment_element) to your payment form. It embeds an iframe with a dynamic form that collects payment details for a variety of payment methods. Your customer can pick a payment method type, and the form automatically collects all necessary payments details for their selection.

### Create the Payment Element

Create a [Payment Element](https://docs.stripe.com/js/custom_checkout/create_payment_element) and mount it to the placeholder `<div>` in your payment form. This embeds an iframe with a dynamic form that displays configured payment method types, allowing your customer to select a payment method. The form automatically collects the associated payment details for the selected payment method type.

### (Optional) Style the Payment Element

Customize the Payment Element UI by creating an [Appearance object](https://docs.stripe.com/elements/appearance-api.md) and passing it as an option to the CheckoutElementsProvider. Use your company’s color scheme and font to make it match with the rest of your checkout page. Use custom fonts (for example, from Google Fonts) by initializing Checkout with a [font set](https://docs.stripe.com/js/react_stripe_js/checkout/checkout_provider#react_checkout_provider-options-elementsOptions-fonts).

Make sure to open the preview on the right to see your changes live.

Parts of this preview demo might not match your actual checkout page. These settings represent only a subset of the [Appearance object’s](https://docs.stripe.com/payments/checkout/customization/appearance.md?payment-ui=embedded-components) variables, and the [Appearance object](https://docs.stripe.com/payments/checkout/customization/appearance.md?payment-ui=embedded-components) only controls certain attributes of Stripe Elements. You’re responsible for styling the rest of your checkout page.

### (Optional) Style the Payment Element

Customize the Payment Element UI by creating an [Appearance object](https://docs.stripe.com/elements/appearance-api.md) and initializing Checkout with it. Use your company’s color scheme and font to make it match with the rest of your checkout page. Use custom fonts (for example, from Google Fonts) by initializing Checkout with a [font set](https://docs.stripe.com/js/custom_checkout/init#custom_checkout_init-options-elementsOptions-fonts).

Make sure to open the preview on the right to see your changes live.

Parts of this preview demo might not match your actual checkout page. These settings represent only a subset of the [Appearance object’s](https://docs.stripe.com/payments/checkout/customization/appearance.md?payment-ui=embedded-components) variables, and the [Appearance object](https://docs.stripe.com/payments/checkout/customization/appearance.md?payment-ui=embedded-components) only controls certain attributes of Stripe Elements. You’re responsible for styling the rest of your checkout page.

### Handle the submit event

Listen to the form’s submit event to know when to confirm the payment through the Stripe API.

### Complete the payment

Call [confirm](https://docs.stripe.com/js/custom_checkout/confirm) when the customer is ready to complete checkout, such as in response to clicking a pay button.

### Handle errors

If there are any immediate [errors](https://docs.stripe.com/error-codes.md) (for example, your customer’s card is declined), Stripe.js returns an error. Show that error message to your customer so they can try again.

### Create an endpoint to retrieve a Checkout Session

Add an endpoint to retrieve a Checkout Session status.

### Add a return page

To display order information to your customer, create a return page for the URL you provided as the Checkout Session `return_url`. Stripe redirects to this page after the customer completes the checkout.

### Add a return component

To display order information to your customer, add a new route and return component for the URL you provided as the Checkout Session `return_url`. Stripe redirects to this page after the customer completes the checkout.

### Retrieve a Checkout session

As soon as your return page loads, immediately make a request to the endpoint on your server. Use the Checkout Session ID in the URL to retrieve the status of the Checkout Session.

### Handle session

Handle the result of the session by using its status:

- `complete`: The payment succeeded. Use the information from the Checkout Session to render a success page.
- `open`: The payment failed or was canceled. Remount Checkout so that your customer can try again.

### Run the application

Start your server and go to <http://localhost:4242/checkout.html>.

### Run the application

Start your server and go to <http://localhost:3000/checkout>.

### Run the server application

Start your server.

### Run the client application

Start your React app and go to <http://localhost:3000/checkout>.

```bash
npm start
```

### Try it out

Click the pay button to complete the payment, which redirects you to the specified return page.

If you see the return page, and the payment in the list of [successful payments](https://dashboard.stripe.com/test/payments?status%5B0%5D=successful) in the Dashboard, your integration is successfully working. Use any of the following test cards to simulate a payment:

| Scenario | Card Number |
| --- | --- |
| Payment succeeds | 4242424242424242 |
| Payment requires 3DS authentication | 4000002500003155 |
| Payment is declined | 4000000000009995 |

To learn more, see [Testing](https://docs.stripe.com/testing.md).

## Congratulations! 

You have a basic Checkout integration working. Now learn how to customize the appearance of your checkout page, automate tax collection, create customers, save payment methods, and localize currencies.

### Activate additional features

Use [Checkout Sessions](https://docs.stripe.com/api/checkout/sessions/create.md) to activate additional features, like collecting addresses and prefilling customer data.

### Prefill customer data

You can prefill the customer’s email address in two ways:

- **Immutable prefill**: Pass [customer_email](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_email), [customer_account](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_account) (a customer-configured `Account` ID), or [customer](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer) (a `Customer` ID) when creating the Checkout Session. These prefill the session with an email that customers can’t edit on the checkout page, so you can omit the Contact Details Element from your form if you want. Learn more about the [difference between using Customers and Accounts](https://docs.stripe.com/accounts-v2/use-accounts-as-customers.md).
- **Mutable prefill**: Pass [defaultValues.email](https://docs.stripe.com/js/custom_checkout/init#custom_checkout_init-options-defaultValues-email) when initializing Checkout. This prefills the email field with a value that customers can edit on the checkout page. Use this for scenarios where the customer provided an email earlier in your flow but might want to change it during checkout.

### Require billing and shipping details

Use [billing_address_collection](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-billing_address_collection) and [shipping_address_collection](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-shipping_address_collection) to require your customer’s address. [shipping_address_collection](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-shipping_address_collection) requires a list of `allowed_countries`, which Checkout displays in a dropdown menu on the page. The sample uses `US` and `CA` as an example. Replace these values with the two-letter ISO country codes for the countries you ship to.

### Collect billing and shipping details

Use the Address Element to collect your customer’s address.

### Automate tax collection

Calculate and collect the right amount of tax on your Stripe transactions. Learn more about [Stripe Tax](https://docs.stripe.com/tax.md) and [how to add it to Checkout](https://docs.stripe.com/tax/checkout.md).

### Set up Stripe Tax in the Dashboard

[Activate Stripe Tax](https://dashboard.stripe.com/settings/tax/activate) to monitor your tax obligations, automatically collect tax, and access the reports you need to file returns.

### Add the automatic tax parameter

Set the `automatic_tax` parameter to `enabled: true`.

### Collect billing details

Use the Address Element to collect your customer’s address.

### Create a customer during checkout

Create a customer for each Checkout Session to track payment history, associate purchases, and prefill fields for them. Learn more about [managing customers](https://docs.stripe.com/customer-management.md).

### Add the customer creation parameter

Set the `customer_creation` parameter to `always` to create a new customer for each completed Checkout Session.

### Specify returning customers

To associate a Checkout Session with a customer that already exists, provide the [customer](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer) when creating a session. If you model customers using Accounts v2, you can also pass an Account ID to the [customer_account](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_account) field to prefill the associated email address. Learn more about the [difference between using v1 Customers and v2 Accounts](https://docs.stripe.com/accounts-v2/use-accounts-as-customers.md).

### Save a customer’s payment method

Allow a customer to save their payment method during checkout for future purchases. Learn more about [saving payment methods during payment](https://docs.stripe.com/payments/save-during-payment.md).

### Add saved payment method parameters

Set `payment_method_save` to `enabled` in `saved_payment_method_options`. You must either provide the ID of an existing customer (in [customer](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer) if using `Customer` objects, [customer_account](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_account) if using customer-configured `Account` objects) or set [customer_creation](https://docs.stripe.com/api/checkout/sessions/create.md#create_checkout_session-customer_creation) to `always`.

### Localize currencies

Adaptive Pricing lets your customers pay in their local currency in more than 150 countries.

### Enable Adaptive Pricing

[Enable Adaptive Pricing](https://dashboard.stripe.com/settings/adaptive-pricing) in your Dashboard settings.

### Localize and format prices

Display localized and formatted amounts from the [Session object](https://docs.stripe.com/js/custom_checkout/session_object).

### Localize and format prices

Display localized and formatted amounts from the [useCheckoutElements](https://docs.stripe.com/js/react_stripe_js/checkout/use_checkout_elements) hook.

### Render the Currency Selector Element

Mount the Currency Selector Element to give your customer a choice of currencies and disclose the exchange rate.

### Mark your integration ready for Adaptive Pricing

Set the `adaptivePricing.allowed` parameter to `true`.

// This is a public sample test API key.
// Don't submit any personally identifiable information in requests made with this key.
// To see your own test secret API key embedded in code samples, sign in to your Stripe account.
// You can also find your test secret API key at https://dashboard.stripe.com/test/apikeys.
// Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
const stripe = require("stripe")("<<YOUR_SECRET_KEY>>");
const YOUR_DOMAIN = "http://localhost:4242";
const YOUR_DOMAIN = "http://localhost:3000";
  const session = await stripe.checkout.sessions.create({

    ui_mode: "elements",
    customer_email: "customer@example.com",
    billing_address_collection: "auto",
    shipping_address_collection: {
      allowed_countries: ["US", "CA"],
    },
    line_items: [
      {
        // Provide the exact Price ID (for example, price_1234) of the product you want to sell
        price: "{{PRICE_ID}}",
        quantity: 1,
      },
    ],
    line_items: [{
      price_data: {
        product_data: {
          name: "{{PRICE_DATA_NAME}}",
        },
        currency: "{{PRICE_DATA_CURRENCY}}",
        unit_amount: {{PRICE_DATA_UNIT_AMOUNT}},
      },
      quantity: 1,
    }],
    line_items: [{
      price_data: {
        product_data: {
          name: "{{PRICE_DATA_NAME}}",
        },
        currency: "{{PRICE_DATA_CURRENCY}}",
        unit_amount: {{PRICE_DATA_UNIT_AMOUNT}},
        recurring: {
          interval: "{{PRICE_DATA_INTERVAL}}",
          interval_count: {{PRICE_DATA_INTERVAL_COUNT}},
        },
      },
      quantity: 1,
    }],
    mode: {{CHECKOUT_MODE}},
    return_url: `${YOUR_DOMAIN}/complete?session_id={CHECKOUT_SESSION_ID}`,
    return_url: `${YOUR_DOMAIN}/complete.html?session_id={CHECKOUT_SESSION_ID}`,
    automatic_tax: {enabled: true},
    customer_creation: 'always',
    saved_payment_method_options: {
      payment_method_save: 'enabled',
    },
  });

  res.send({ clientSecret: session.client_secret });
app.get("/session-status", async (req, res) => {
  const session = await stripe.checkout.sessions.retrieve(req.query.session_id, {expand: ["payment_intent", "subscription"]});

   res.send({
    status: session.status,
    payment_status: session.payment_status,
    payment_intent_id: session.payment_intent?.id,
    payment_intent_status: session.payment_intent?.status,
    subscription_id: session.payment_intent ? null : session.subscription?.id,
    subscription_status: session.payment_intent ? null : session.subscription?.status
  });
});
{
  "name": "stripe-sample",
  "version": "1.0.0",
  "description": "A sample Stripe implementation",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "author": "stripe-samples",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1",
    "stripe": "^22.4.0"
  }
}
{
  "name": "stripe-sample",
  "version": "0.1.0",
  "dependencies": {
    "@stripe/react-stripe-js": "^3.7.0",
    "@stripe/stripe-js": "^7.3.0",
    "express": "^4.17.1",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "^3.4.0",
    "stripe": "22.4.0"
  },
  "devDependencies": {
    "concurrently": "4.1.2"
  },
  "homepage": "http://localhost:3000/checkout",
  "proxy": "http://localhost:4242",
  "scripts": {
    "start-client": "react-scripts start",
    "start-server": "node server.js",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "start": "concurrently \"npm run start-client\" \"npm run start-server\""
  },
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
\# This is a public sample test API key.
# Don't submit any personally identifiable information in requests made with this key.
# To see your own test secret API key embedded in code samples, sign in to your Stripe account.
# You can also find your test secret API key at https://dashboard.stripe.com/test/apikeys.
\# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
client = Stripe::StripeClient.new('<<YOUR_SECRET_KEY>>', stripe_version: '2026-07-29.dahlia')
YOUR_DOMAIN = 'http://localhost:4242'
YOUR_DOMAIN = 'http://localhost:3000'
  session = client.v1.checkout.sessions.create({

    ui_mode: 'elements',
    customer_email: 'customer@example.com',
    billing_address_collection: 'required',
    shipping_address_collection: {
      allowed_countries: ['US', 'CA'],
    },
    line_items: [{
      \# Provide the exact Price ID (for example, price_1234) of the product you want to sell
      price: '{{PRICE_ID}}',
      quantity: 1,
    }],
    line_items: [{
      price_data: {
        product_data: {
          name: '{{PRICE_DATA_NAME}}',
        },
        currency: '{{PRICE_DATA_CURRENCY}}',
        unit_amount: {{PRICE_DATA_UNIT_AMOUNT}},
      },
      quantity: 1,
    }],
    line_items: [{
      price_data: {
        product_data: {
          name: '{{PRICE_DATA_NAME}}',
        },
        currency: '{{PRICE_DATA_CURRENCY}}',
        unit_amount: {{PRICE_DATA_UNIT_AMOUNT}},
        recurring: {
          interval: '{{PRICE_DATA_INTERVAL}}',
          interval_count: {{PRICE_DATA_INTERVAL_COUNT}},
        },
      },
      quantity: 1,
    }],
    mode: {{CHECKOUT_MODE}},
    return_url: YOUR_DOMAIN + '/complete.html?session_id={CHECKOUT_SESSION_ID}',
    return_url: YOUR_DOMAIN + '/complete?session_id={CHECKOUT_SESSION_ID}',
    automatic_tax: {enabled: true},
    customer_creation: 'always',
    saved_payment_method_options: {
      payment_method_save: 'enabled',
    },
  })

  { clientSecret: session.client_secret }.to_json
get '/session-status' do
  session = client.v1.checkout.sessions.retrieve(params[:session_id], {expand: ["payment_intent", "subscription"]})

  { status: session.status, payment_status: session.payment_status, payment_intent_id: session.payment_intent&.id, payment_intent_status: session.payment_intent&.status, subscription_id: session.payment_intent ? nil : session.subscription&.id, subscription_status: session.payment_intent ? nil : session.subscription&.status }.to_json
end
import stripe
\# This is a public sample test API key.
# Don't submit any personally identifiable information in requests made with this key.
# To see your own test secret API key embedded in code samples, sign in to your Stripe account.
# You can also find your test secret API key at https://dashboard.stripe.com/test/apikeys.
client = stripe.StripeClient(
    \# Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
    '<<YOUR_SECRET_KEY>>',
    stripe_version='2026-07-29.dahlia',
)
YOUR_DOMAIN = 'http://localhost:4242'
YOUR_DOMAIN = 'http://localhost:3000'
        session = client.v1.checkout.sessions.create(params={
            'ui_mode': 'elements',
            'customer_email': 'customer@example.com',
            'billing_address_collection': 'auto',
            'shipping_address_collection': {
              'allowed_countries': ['US', 'CA'],
            },
            'line_items': [
                {
                    \# Provide the exact Price ID (for example, price_1234) of the product you want to sell
                    'price': '{{PRICE_ID}}',
                    'quantity': 1,
                },
            ],
            'line_items': [
                {
                    'price_data': {
                      'product_data': {
                        'name': '{{PRICE_DATA_NAME}}',
                      },
                      'currency': '{{PRICE_DATA_CURRENCY}}',
                      'unit_amount': {{PRICE_DATA_UNIT_AMOUNT}},
                    },
                    'quantity': 1,
                },
            ],
            'line_items': [
                {
                    'price_data': {
                      'product_data': {
                        'name': '{{PRICE_DATA_NAME}}',
                      },
                      'currency': '{{PRICE_DATA_CURRENCY}}',
                      'unit_amount': {{PRICE_DATA_UNIT_AMOUNT}},
                      'recurring': {
                        'interval': '{{PRICE_DATA_INTERVAL}}',
                        'interval_count': {{PRICE_DATA_INTERVAL_COUNT}},
                      },
                    },
                    'quantity': 1,
                },
            ],
            'mode': {{CHECKOUT_MODE}},
            'return_url': YOUR_DOMAIN + '/complete.html?session_id={CHECKOUT_SESSION_ID}',
            'return_url': YOUR_DOMAIN + '/complete?session_id={CHECKOUT_SESSION_ID}',
            'automatic_tax': {'enabled': True},
            'customer_creation': 'always',
            'saved_payment_method_options': {
              'payment_method_save': 'enabled',
            },
        })
    return jsonify(clientSecret=session.client_secret)
@app.route('/session-status', methods=['GET'])
def session_status():
  session = client.v1.checkout.sessions.retrieve(request.args.get('session_id'), params={'expand': ["payment_intent", "subscription"]})

  return jsonify(status=session.status, payment_status=session.payment_status, payment_intent_id=session.payment_intent.id if session.payment_intent else None, payment_intent_status=session.payment_intent.status if session.payment_intent else None, subscription_id=None if session.payment_intent else (session.subscription.id if session.subscription else None), subscription_status=None if session.payment_intent else (session.subscription.status if session.subscription else None))
certifi==2026.1.4
chardet==5.2.0
click==8.3.1
Flask==3.1.2
idna==3.11
itsdangerous==2.2.0
Jinja2==3.1.6
MarkupSafe==3.0.3
requests==2.32.5
stripe==15.4.0
toml==0.10.2
Werkzeug==3.1.5
$stripe = new \Stripe\StripeClient([
  "api_key" => $stripeSecretKey,
  "stripe_version" => "2026-07-29.dahlia"
]);
$YOUR_DOMAIN = 'http://localhost:4242';
$YOUR_DOMAIN = 'http://localhost:3000';

$checkout_session = $stripe->checkout->sessions->create([

  'ui_mode' => 'elements',
  'customer_email' => 'customer@example.com',
  'billing_address_collection' => 'required',
  'shipping_address_collection' => [
    'allowed_countries' => ['US', 'CA'],
  ],
  'line_items' => [[
    \# Provide the exact Price ID (for example, price_1234) of the product you want to sell
    'price' => '{{PRICE_ID}}',
    'quantity' => 1,
  ]],
  'line_items' => [[
      'price_data' => [
        'product_data' => [
          'name' => '{{PRICE_DATA_NAME}}',
        ],
        'currency' => '{{PRICE_DATA_CURRENCY}}',
        'unit_amount' => {{PRICE_DATA_UNIT_AMOUNT}},
      ],
    'quantity' => 1,
  ]],
  'line_items' => [[
      'price_data' => [
        'product_data' => [
          'name' => '{{PRICE_DATA_NAME}}',
        ],
        'currency' => '{{PRICE_DATA_CURRENCY}}',
        'unit_amount' => {{PRICE_DATA_UNIT_AMOUNT}},
        'recurring' => [
          'interval' => '{{PRICE_DATA_INTERVAL}}',
          'interval_count' => {{PRICE_DATA_INTERVAL_COUNT}},
        ],
      ],
    'quantity' => 1,
  ]],
  'mode' => {{CHECKOUT_MODE}},
  'return_url' => $YOUR_DOMAIN . '/complete.html?session_id={CHECKOUT_SESSION_ID}',
  'return_url' => $YOUR_DOMAIN . '/complete?session_id={CHECKOUT_SESSION_ID}',
  'automatic_tax' => [
    'enabled' => true,
  ],
  'customer_creation' => 'always',
  'saved_payment_method_options' => [
    'payment_method_save' => 'enabled',
  ],
]);

echo json_encode(array('clientSecret' => $checkout_session->client_secret));
try {
  // retrieve JSON from POST body
  $jsonStr = file_get_contents('php://input');
  $jsonObj = json_decode($jsonStr);

  $session = $stripe->checkout->sessions->retrieve($jsonObj->session_id, ['expand' => ['payment_intent', 'subscription']]);

  echo json_encode(['status' => $session->status, 'payment_status' => $session->payment_status, 'payment_intent_id' => $session->payment_intent?->id, 'payment_intent_status' => $session->payment_intent?->status, 'subscription_id' => $session->payment_intent ? null : $session->subscription?->id, 'subscription_status' => $session->payment_intent ? null : $session->subscription?->status]);
  http_response_code(200);
} catch (Error $e) {
  http_response_code(500);
  echo json_encode(['error' => $e->getMessage()]);
}
// Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
$stripeSecretKey = '<<YOUR_SECRET_KEY>>';
          // This is a public sample test API key.
          // Don't submit any personally identifiable information in requests made with this key.
          // To see your own test secret API key embedded in code samples, sign in to your Stripe account.
          // You can also find your test secret API key at https://dashboard.stripe.com/test/apikeys.
          // Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
          services.AddSingleton(new StripeClient("<<YOUR_SECRET_KEY>>"));
            var domain = "http://localhost:4242";
            var domain = "http://localhost:3000";

                UiMode = "elements",
                CustomerEmail = "customer@example.com",
                BillingAddressCollection = "auto",
                ShippingAddressCollection = new SessionShippingAddressCollectionOptions
                {
                  AllowedCountries = new List<string>
                  {
                    "US",
                    "CA",
                  },
                },
                LineItems = new List<SessionLineItemOptions>
                {
                  new SessionLineItemOptions
                  {
                    // Provide the exact Price ID (for example, price_1234) of the product you want to sell
                    Price = "{{PRICE_ID}}",
                    Quantity = 1,
                  },
                },
                LineItems = new List<SessionLineItemOptions>
                {
                  new SessionLineItemOptions
                  {
                    PriceData = new SessionLineItemPriceDataOptions
                    {
                      ProductData = new SessionLineItemPriceDataProductDataOptions
                      {
                        Name = "{{PRICE_DATA_NAME}}",
                      },
                      Currency = "{{PRICE_DATA_CURRENCY}}",
                      UnitAmount = {{PRICE_DATA_UNIT_AMOUNT}},
                    },
                    Quantity = 1,
                  },
                },
                LineItems = new List<SessionLineItemOptions>
                {
                  new SessionLineItemOptions
                  {
                    PriceData = new SessionLineItemPriceDataOptions
                    {
                      ProductData = new SessionLineItemPriceDataProductDataOptions
                      {
                        Name = "{{PRICE_DATA_NAME}}",
                      },
                      Currency = "{{PRICE_DATA_CURRENCY}}",
                      UnitAmount = {{PRICE_DATA_UNIT_AMOUNT}},
                      Recurring = new SessionLineItemPriceDataRecurringOptions
                      {
                        Interval = "{{PRICE_DATA_INTERVAL}}",
                        IntervalCount = {{PRICE_DATA_INTERVAL_COUNT}},
                      },
                    },
                    Quantity = 1,
                  },
                },
                Mode = {{CHECKOUT_MODE}},
                ReturnUrl = domain + "/complete.html?session_id={CHECKOUT_SESSION_ID}",
                ReturnUrl = domain + "/complete?session_id={CHECKOUT_SESSION_ID}",
                AutomaticTax = new SessionAutomaticTaxOptions { Enabled = true },
                CustomerCreation = "always",
                SavedPaymentMethodOptions = new SessionSavedPaymentMethodOptionsOptions
                {
                    PaymentMethodSave = "enabled",
                },
            Session session = _client.V1.Checkout.Sessions.Create(options);

            return Json(new {clientSecret = session.ClientSecret});
    [Route("session-status")]
    [ApiController]
    public class SessionStatusController : Controller
    {
        private readonly StripeClient _client;

        public SessionStatusController(StripeClient client)
        {
            _client = client;
        }

        [HttpGet]
        public ActionResult SessionStatus([FromQuery] string session_id)
        {
            var options = new SessionGetOptions();
            options.AddExpand("payment_intent");
            options.AddExpand("subscription");
            Session session = _client.V1.Checkout.Sessions.Get(session_id, options);

            return Json(new {status = session.Status, payment_status = session.PaymentStatus, payment_intent_id = session.PaymentIntent?.Id, payment_intent_status = session.PaymentIntent?.Status, subscription_id = session.PaymentIntent != null ? null : session.Subscription?.Id, subscription_status = session.PaymentIntent != null ? null : session.Subscription?.Status});
        }
    }
    "github.com/stripe/stripe-go/v86"
  // Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
  sc := stripe.NewClient("<<YOUR_SECRET_KEY>>")
  domain := "http://localhost:4242"
  domain := "http://localhost:3000"
  params := &stripe.CheckoutSessionCreateParams{

    UIMode: stripe.String("elements"),
    ReturnURL: stripe.String(domain + "/complete.html?session_id={CHECKOUT_SESSION_ID}"),
    ReturnURL: stripe.String(domain + "/complete?session_id={CHECKOUT_SESSION_ID}"),
    CustomerEmail: stripe.String("customer@example.com"),
    BillingAddressCollection: stripe.String("auto"),
    ShippingAddressCollection: &stripe.CheckoutSessionCreateShippingAddressCollectionParams{
      AllowedCountries: stripe.StringSlice([]string{
        "US",
        "CA",
      }),
    },
    LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
      {
        // Provide the exact Price ID (for example, price_1234) of the product you want to sell
        Price: stripe.String("{{PRICE_ID}}"),
        Quantity: stripe.Int64(1),
      },
    },
    LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
      {
        PriceData: &stripe.CheckoutSessionCreateLineItemPriceDataParams{
          ProductData: &stripe.CheckoutSessionCreateLineItemPriceDataProductDataParams{
            Name: stripe.String("{{PRICE_DATA_NAME}}"),
          },
          Currency: stripe.String("{{PRICE_DATA_CURRENCY}}"),
          UnitAmount: stripe.Int64({{PRICE_DATA_UNIT_AMOUNT}}),
        },
        Quantity: stripe.Int64(1),
      },
    },
    LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
      {
        PriceData: &stripe.CheckoutSessionCreateLineItemPriceDataParams{
          ProductData: &stripe.CheckoutSessionCreateLineItemPriceDataProductDataParams{
            Name: stripe.String("{{PRICE_DATA_NAME}}"),
          },
          Currency: stripe.String("{{PRICE_DATA_CURRENCY}}"),
          UnitAmount: stripe.Int64({{PRICE_DATA_UNIT_AMOUNT}}),
          Recurring: &stripe.CheckoutSessionCreateLineItemPriceDataRecurringParams{
            Interval: stripe.String("{{PRICE_DATA_INTERVAL}}"),
            IntervalCount: stripe.Int64({{PRICE_DATA_INTERVAL_COUNT}}),
          },
        },
        Quantity: stripe.Int64(1),
      },
    },
    Mode: {{CHECKOUT_MODE}},
    AutomaticTax: &stripe.CheckoutSessionCreateAutomaticTaxParams{Enabled: stripe.Bool(true)},
    CustomerCreation: stripe.String("always"),
    SavedPaymentMethodOptions: &stripe.CheckoutSessionCreateSavedPaymentMethodOptionsParams{
      PaymentMethodSave: stripe.String("enabled"),
    },
  }

  s, err := sc.V1CheckoutSessions.Create(context.TODO(), params)

  if err != nil {
    log.Printf("sc.V1CheckoutSessions.Create: %v", err)
  }

  writeJSON(w, struct {
    ClientSecret string `json:"clientSecret"`
  }{
    ClientSecret: s.ClientSecret,
  })
func retrieveCheckoutSession(sc *stripe.Client, w http.ResponseWriter, r *http.Request) {
  params := &stripe.CheckoutSessionRetrieveParams{}
  params.AddExpand("payment_intent")
  params.AddExpand("subscription")
  s, _ := sc.V1CheckoutSessions.Retrieve(context.TODO(), r.URL.Query().Get("session_id"), params)

  var paymentIntentId, paymentIntentStatus string
  var subscriptionId, subscriptionStatus string
  if s.PaymentIntent != nil {
    paymentIntentId = string(s.PaymentIntent.ID)
    paymentIntentStatus = string(s.PaymentIntent.Status)
  } else if s.Subscription != nil {
    subscriptionId = string(s.Subscription.ID)
    subscriptionStatus = string(s.Subscription.Status)
  }

  writeJSON(w, struct {
    Status string `json:"status"`
    PaymentStatus string `json:"payment_status"`
    PaymentIntentId string `json:"payment_intent_id"`
    PaymentIntentStatus string `json:"payment_intent_status"`
    SubscriptionId string `json:"subscription_id"`
    SubscriptionStatus string `json:"subscription_status"`
  }{
    Status: string(s.Status),
    PaymentStatus: string(s.PaymentStatus),
    PaymentIntentId: paymentIntentId,
    PaymentIntentStatus: paymentIntentStatus,
    SubscriptionId: subscriptionId,
    SubscriptionStatus: subscriptionStatus,
  })
}
require github.com/stripe/stripe-go/v86 v86.2.0
    // This is a public sample test API key.
    // Don't submit any personally identifiable information in requests made with this key.
    // To see your own test secret API key embedded in code samples, sign in to your Stripe account.
    // You can also find your test secret API key at https://dashboard.stripe.com/test/apikeys.
    // Don't put any keys in code. See https://docs.stripe.com/keys-best-practices.
    StripeClient client = new StripeClient("<<YOUR_SECRET_KEY>>");
        String YOUR_DOMAIN = "http://localhost:4242";
        String YOUR_DOMAIN = "http://localhost:3000";
        SessionCreateParams params =
          SessionCreateParams.builder()

            .setUiMode(SessionCreateParams.UiMode.ELEMENTS)
            .setCustomerEmail("customer@example.com")
            .setBillingAddressCollection(SessionCreateParams.BillingAddressCollection.REQUIRED)
            .setShippingAddressCollection(
              SessionCreateParams.ShippingAddressCollection.builder()
                .addAllowedCountry(SessionCreateParams.ShippingAddressCollection.AllowedCountry.CA)
                .addAllowedCountry(SessionCreateParams.ShippingAddressCollection.AllowedCountry.US)
                .build())
            .setMode({{CHECKOUT_MODE}})
            .setReturnUrl(YOUR_DOMAIN + "/complete?session_id={CHECKOUT_SESSION_ID}")
            .setReturnUrl(YOUR_DOMAIN + "/complete.html?session_id={CHECKOUT_SESSION_ID}")
            .setAutomaticTax(
              SessionCreateParams.AutomaticTax.builder()
                .setEnabled(true)
                .build())
            .setCustomerCreation(SessionCreateParams.CustomerCreation.ALWAYS)
            .setSavedPaymentMethodOptions(
              SessionCreateParams.SavedPaymentMethodOptions.builder()
                .setPaymentMethodSave(SessionCreateParams.SavedPaymentMethodOptions.PaymentMethodSave.ENABLED)
                .build())
            .addLineItem(
              SessionCreateParams.LineItem.builder()
                .setQuantity(1L)
                // Provide the exact Price ID (for example, price_1234) of the product you want to sell
                .setPrice("{{PRICE_ID}}")
                .build())
            .addLineItem(
              SessionCreateParams.LineItem.builder()
                .setQuantity(1L)
                .setPriceData(
                  SessionCreateParams.LineItem.PriceData.builder()
                  .setProductData(
                    SessionCreateParams.LineItem.PriceData.ProductData.builder()
                    .setName("{{PRICE_DATA_NAME}}")
                    .build())
                  .setCurrency("{{PRICE_DATA_CURRENCY}}")
                  .setUnitAmount({{PRICE_DATA_UNIT_AMOUNT}}L)
                  .build())
                .build())
            .addLineItem(
              SessionCreateParams.LineItem.builder()
                .setQuantity(1L)
                .setPriceData(
                  SessionCreateParams.LineItem.PriceData.builder()
                  .setProductData(
                    SessionCreateParams.LineItem.PriceData.ProductData.builder()
                    .setName("{{PRICE_DATA_NAME}}")
                    .build())
                  .setCurrency("{{PRICE_DATA_CURRENCY}}")
                  .setUnitAmount({{PRICE_DATA_UNIT_AMOUNT}}L)
                  .setRecurring(
                    SessionCreateParams.LineItem.PriceData.Recurring.builder()
                    .setInterval(SessionCreateParams.LineItem.PriceData.Recurring.Interval.{{PRICE_DATA_INTERVAL}})
                    .setIntervalCount({{PRICE_DATA_INTERVAL_COUNT}}L)
                    .build())
                  .build())
                .build())
            .build();
      Map<String, String> map = new HashMap();
      map.put("clientSecret", session.getRawJsonObject().getAsJsonPrimitive("client_secret").getAsString());


      return map;
    get("/session-status", (request, response) -> {
      RequestOptions options = RequestOptions.builder().build();
      SessionRetrieveParams params =
        SessionRetrieveParams.builder().addExpand("payment_intent").addExpand("subscription").build();
      Session session = client.v1().checkout().sessions().retrieve(request.queryParams("session_id"), params, options);

      Map<String, String> map = new HashMap();
      map.put("status", session.getRawJsonObject().getAsJsonPrimitive("status").getAsString());
      map.put("payment_status", session.getRawJsonObject().getAsJsonPrimitive("payment_status").getAsString());
      var paymentIntentJson = session.getRawJsonObject().get("payment_intent");
      if (paymentIntentJson != null && paymentIntentJson.isJsonObject()) {
        map.put("payment_intent_id", paymentIntentJson.getAsJsonObject().getAsJsonPrimitive("id").getAsString());
        map.put("payment_intent_status", paymentIntentJson.getAsJsonObject().getAsJsonPrimitive("status").getAsString());
      } else {
        var subscriptionJson = session.getRawJsonObject().get("subscription");
        if (subscriptionJson != null && subscriptionJson.isJsonObject()) {
          map.put("subscription_id", subscriptionJson.getAsJsonObject().getAsJsonPrimitive("id").getAsString());
          map.put("subscription_status", subscriptionJson.getAsJsonObject().getAsJsonPrimitive("status").getAsString());
        }
      }

      return map;
    }, gson::toJson);
    <script src="https://js.stripe.com/dahlia/stripe.js"></script>
    <form id="payment-form">
      <div id="contact-details-element">
        <!--Stripe.js injects the Contact Details Element-->
      </div>
      <h4>Billing Address</h4>
      <div id="billing-address-element">
        <!--Stripe.js injects the Address Element-->
      </div>
      <h4>Billing Address</h4>
      <div id="billing-address-element">
        <!--Stripe.js injects the Address Element-->
      </div>
      <h4>Shipping Address</h4>
      <div id="shipping-address-element">
        <!--Stripe.js injects the Address Element-->
      </div>
      <h4>Payment</h4>
      <div id="currency-selector-element">
        <!--Stripe.js injects the Currency Selector Element-->
      </div>
      <div id="payment-element">
        <!--Stripe.js injects the Payment Element-->
      </div>
      <button id="submit">
        <div class="spinner hidden" id="spinner"></div>
        <span id="button-text">Pay now</span>
      </button>
      <div id="payment-message" class="hidden"></div>
    </form>
const stripe = Stripe("<<YOUR_PUBLISHABLE_KEY>>");
  const promise = fetch("/create-checkout-session", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
  })
    .then((r) => r.json())
    .then((r) => r.clientSecret);
  const promise = fetch('/create.php', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
  })
    .then((r) => r.json())
    .then((r) => r.clientSecret);
  const appearance = {
    {{APPEARANCE}}
  };

  checkout = stripe.initCheckoutElementsSdk({
    clientSecret: promise,
    elementsOptions: { appearance },
    adaptivePricing: { allowed: true },
  });
  checkout.on('change', (session) => {
    // Handle changes to the checkout session
    document.getElementById('submit').disabled = !session.canConfirm;
    document.querySelector("#button-text").textContent = `Pay ${
      session.total.total.amount
    } now`;
  });
  const contactDetailsElement = checkout.createContactDetailsElement();
  contactDetailsElement.mount("#contact-details-element");
  const paymentElement = checkout.createPaymentElement();
  paymentElement.mount("#payment-element");
  const billingAddressElement = checkout.createBillingAddressElement();
  billingAddressElement.mount("#billing-address-element");
  const billingAddressElement = checkout.createBillingAddressElement();
  billingAddressElement.mount("#billing-address-element");
  const shippingAddressElement = checkout.createShippingAddressElement();
  shippingAddressElement.mount("#shipping-address-element");
  const currencySelectorElement = checkout.createCurrencySelectorElement();
  currencySelectorElement.mount("#currency-selector-element");
async function handleSubmit(e) {
  e.preventDefault();
  setLoading(true);

  const confirmResult = await actions.confirm();

  // This point will only be reached if there is an immediate error when
  // confirming the payment. Otherwise, your customer will be redirected to
  // your `return_url`. For some payment methods like iDEAL, your customer will
  // be redirected to an intermediate site first to authorize the payment, then
  // redirected to the `return_url`.
  if (confirmResult.type === 'error') {
    showMessage(confirmResult.error.message);
  }

  setLoading(false);
}
    <script src="https://js.stripe.com/dahlia/stripe.js"></script>
    <script src="complete.js" defer></script>
  const queryString = window.location.search;
  const urlParams = new URLSearchParams(queryString);
  const sessionId = urlParams.get("session_id");
  if (!sessionId) {
    console.log("No session ID found");
    setErrorState();
    return;
  }
  const response = await fetch(`/session-status?session_id=${sessionId}`);
  const response = await fetch("/status.php", {
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json",
    },
    method: "POST",
    body: JSON.stringify({ session_id: sessionId }),
  });
  const session = await response.json();
  setSessionDetails(session);
{
  "name": "stripe-sample",
  "version": "0.1.0",
  "dependencies": {
    "@stripe/react-stripe-js": "^6.0.0",
    "@stripe/stripe-js": "^9.0.0",
    "express": "^4.17.1",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.16.0",
    "stripe": "^8.202.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.0",
    "concurrently": "^8.2.0",
    "vite": "^5.2.11"
  },
  "scripts": {
    "start-client": "vite",
    "start-server": "node server.js",
    "build": "vite build",
    "start": "concurrently \"yarn start-client\" \"yarn start-server\""
  }
}
{
  "name": "client",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@stripe/react-stripe-js": "^6.0.0",
    "@stripe/stripe-js": "^9.0.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.16.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.0",
    "vite": "^5.2.11"
  },
  "scripts": {
    "start": "vite",
    "build": "vite build"
  }
}
import React, { useMemo } from "react";
import {loadStripe} from '@stripe/stripe-js';

import {
  CheckoutElementsProvider
} from '@stripe/react-stripe-js/checkout';
// Make sure to call `loadStripe` outside of a component's render to avoid
// recreating the `Stripe` object on every render.
// This is a public sample test API key.
// Don't submit any personally identifiable information in requests made with this key.
// Sign in to see your own test publishable API key embedded in code samples.
// You can also find your test publishable API key at https://dashboard.stripe.com/test/apikeys.
const stripePromise = loadStripe("<<YOUR_PUBLISHABLE_KEY>>");
  const clientSecret = useMemo(() => {
    return fetch('/create-checkout-session', {
      method: 'POST',
    })
      .then((res) => res.json())
      .then((data) => data.clientSecret);
  }, []);
  const clientSecret = useMemo(() => {
    return fetch('/create.php', {
      method: 'POST',
    })
      .then((res) => res.json())
      .then((data) => data.clientSecret);
  }, []);
  const appearance = {
    {{APPEARANCE}}
  };

        <CheckoutElementsProvider
          stripe={stripePromise}
          options={{
            clientSecret,
            elementsOptions: {appearance},
            adaptivePricing: { allowed: true },
          }}
        >
          <Routes>
            <Route path="/checkout" element={<CheckoutForm />} />
            <Route path="/complete" element={<Complete />} />
          </Routes>
        </CheckoutElementsProvider>
  ContactDetailsElement,
  BillingAddressElement,
  BillingAddressElement,
  ShippingAddressElement,
  CurrencySelectorElement,
  const [message, setMessage] = useState(null);
  const [isSubmitting, setIsSubmitting] = useState(false);
  const checkoutState = useCheckoutElements();

  if (checkoutState.type === 'loading') {
    return (
      <div>Loading...</div>
    );
  }

  if (checkoutState.type === 'error') {
    return (
      <div>Error: {checkoutState.error.message}</div>
    );
  }
  const handleSubmit = async (e) => {
    e.preventDefault();

    const {checkout} = checkoutState;
    setIsSubmitting(true);

    const confirmResult = await checkout.confirm();

    // This point will only be reached if there is an immediate error when
    // confirming the payment. Otherwise, your customer will be redirected to
    // your `return_url`. For some payment methods like iDEAL, your customer will
    // be redirected to an intermediate site first to authorize the payment, then
    // redirected to the `return_url`.
    if (confirmResult.type === 'error') {
      setMessage(confirmResult.error.message);
    }

    setIsSubmitting(false);
  };
      <h4>Contact Details</h4>
      <ContactDetailsElement/>
      <h4>Billing Address</h4>
      <BillingAddressElement/>
      <h4>Billing Address</h4>
      <BillingAddressElement/>
      <h4>Shipping Address</h4>
      <ShippingAddressElement/>
      <CurrencySelectorElement/>
      <PaymentElement id="payment-element" />
          `Pay ${checkoutState.checkout.total.total.amount} now`
  const [status, setStatus] = useState(null);
  const [paymentIntentId, setPaymentIntentId] = useState('');
  const [paymentStatus, setPaymentStatus] = useState('');
  const [paymentIntentStatus, setPaymentIntentStatus] = useState('');
  const [subscriptionId, setSubscriptionId] = useState('');
  const [subscriptionStatus, setSubscriptionStatus] = useState('');
  const [iconColor, setIconColor] = useState('');
  const [icon, setIcon] = useState('');
  const [text, setText] = useState('');

  useEffect(() => {
    const SuccessIcon =
      <svg width="16" height="14" viewBox="0 0 16 14" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fillRule="evenodd" clipRule="evenodd" d="M15.4695 0.232963C15.8241 0.561287 15.8454 1.1149 15.5171 1.46949L6.14206 11.5945C5.97228 11.7778 5.73221 11.8799 5.48237 11.8748C5.23253 11.8698 4.99677 11.7582 4.83452 11.5681L0.459523 6.44311C0.145767 6.07557 0.18937 5.52327 0.556912 5.20951C0.924454 4.89575 1.47676 4.93936 1.79051 5.3069L5.52658 9.68343L14.233 0.280522C14.5613 -0.0740672 15.1149 -0.0953599 15.4695 0.232963Z" fill="white"/>
      </svg>;
    const ErrorIcon =
      <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path fillRule="evenodd" clipRule="evenodd" d="M1.25628 1.25628C1.59799 0.914573 2.15201 0.914573 2.49372 1.25628L8 6.76256L13.5063 1.25628C13.848 0.914573 14.402 0.914573 14.7437 1.25628C15.0854 1.59799 15.0854 2.15201 14.7437 2.49372L9.23744 8L14.7437 13.5063C15.0854 13.848 15.0854 14.402 14.7437 14.7437C14.402 15.0854 13.848 15.0854 13.5063 14.7437L8 9.23744L2.49372 14.7437C2.15201 15.0854 1.59799 15.0854 1.25628 14.7437C0.914573 14.402 0.914573 13.848 1.25628 13.5063L6.76256 8L1.25628 2.49372C0.914573 2.15201 0.914573 1.59799 1.25628 1.25628Z" fill="white"/>
      </svg>;

    const queryString = window.location.search;
    const urlParams = new URLSearchParams(queryString);
    const sessionId = urlParams.get('session_id');

    fetch("/status.php", {
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      method: "POST",
      body: JSON.stringify({ session_id: sessionId }),
    })
    fetch(`/session-status?session_id=${sessionId}`)
      .then((res) => res.json())
      .then((data) => {
        setStatus(data.status);
        setPaymentIntentId(data.payment_intent_id);
        setPaymentStatus(data.payment_status);
        setPaymentIntentStatus(data.payment_intent_status);
        setSubscriptionId(data.subscription_id);
        setSubscriptionStatus(data.subscription_status);

        if (data.status === 'complete') {
          setIconColor('#30B130');
          setIcon(SuccessIcon);
          setText('Payment succeeded');
        } else {
          setIconColor('#DF1B41');
          setIcon(ErrorIcon);
          setText('Something went wrong, please try again.');
        }
      });
  }, []);

    return (
      <div id="payment-status">
        <div id="status-icon" style={{backgroundColor: iconColor}}>
        {icon}
      </div>
      <h2 id="status-text">{text}</h2>
      <div id="details-table">
        <table>
          <tbody>
            {paymentIntentId && (
              <tr>
                <td className="TableLabel">Payment Intent ID</td>
                <td id="intent-id" className="TableContent">{paymentIntentId}</td>
              </tr>
            )}
            {subscriptionId && (
              <tr>
                <td className="TableLabel">Subscription ID</td>
                <td id="intent-id" className="TableContent">{subscriptionId}</td>
              </tr>
            )}
            <tr>
              <td className="TableLabel">Status</td>
              <td id="intent-status" className="TableContent">{status}</td>
            </tr>
            <tr>
              <td className="TableLabel">Payment status</td>
              <td id="session-status" className="TableContent">{paymentStatus}</td>
            </tr>
            {paymentIntentId && (
              <tr>
                <td className="TableLabel">Payment Intent status</td>
                <td id="payment-intent-status" className="TableContent">{paymentIntentStatus}</td>
              </tr>
            )}
            {subscriptionId && (
              <tr>
                <td className="TableLabel">Subscription status</td>
                <td id="payment-intent-status" className="TableContent">{subscriptionStatus}</td>
              </tr>
            )}
          </tbody>
        </table>
      </div>
      {(paymentIntentId || subscriptionId) && (
        <div className="sample-key-banner" role="note">
          You're using a public sample test key.{" "}
          <a
            id="view-details"
            href="https://dashboard.stripe.com/login"
            rel="noopener noreferrer"
            target="_blank"
          >
            Sign in or create an account
          </a>{" "}
          to use your own test API key.
        </div>
      )}
      <a id="retry-button" href="/checkout">Test another payment</a>
  </div>
    )
\## Set Price ID

In the server code, replace `{{PRICE_ID}}` with the Price ID (`price_xxx`) of the product you created in the quickstart guide. Find it in your [Stripe test Dashboard under Products](https://dashboard.stripe.com/test/products).

1. Build the server

~~~
pip3 install -r requirements.txt
~~~
1. Build the server

~~~
bundle install
~~~
1. Build the server

~~~
composer install
~~~
1. Build the server

~~~
dotnet restore
~~~
1. Build the server

~~~
mvn package
~~~

2. Run the server

~~~
export FLASK_APP=server.py
python3 -m flask run --port=4242
~~~

2. Run the server

~~~
ruby server.rb -o 0.0.0.0
~~~

2. Run the server

~~~
php -S 127.0.0.1:4242 --docroot=public
~~~

2. Run the server

~~~
dotnet run
~~~

2. Run the server

~~~
java -cp target/sample-jar-with-dependencies.jar com.stripe.sample.Server
~~~

3. Build the client app

~~~
npm install
~~~

4. Run the client app

~~~
npm start
~~~

5. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)

3. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)

3. Build the client app

~~~
npm install
~~~

4. Run the client app

~~~
npm start
~~~

5. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)

3. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)

3. Build the client app

~~~
npm install
~~~

4. Run the client app

~~~
npm start
~~~

5. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)

3. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)

3. Build the client app

~~~
npm install
~~~

4. Run the client app

~~~
npm start
~~~

5. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)

3. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)

3. Build the client app

~~~
npm install
~~~

4. Run the client app

~~~
npm start
~~~

5. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)

3. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)
1. Run the server

~~~
go run server.go
~~~

2. Build the client app

~~~
npm install
~~~

3. Run the client app

~~~
npm start
~~~

4. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)
1. Run the server

~~~
go run server.go
~~~

2. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)
1. Build the application

~~~
npm install
~~~

2. Run the application

~~~
npm start
~~~

3. Go to [http://localhost:3000/checkout](http://localhost:3000/checkout)
1. Build the server

~~~
npm install
~~~

2. Run the server

~~~
npm start
~~~

3. Go to [http://localhost:4242/checkout.html](http://localhost:4242/checkout.html)
\### Development

1. Build the application
~~~shell
$ npm install
~~~

2. _Optional_: download and run the [Stripe CLI](https://stripe.com/docs/stripe-cli)
~~~shell
$ stripe listen --forward-to localhost:3000/api/webhooks
~~~

3. Run the application
~~~shell
$ STRIPE_WEBHOOK_SECRET=$(stripe listen --print-secret) npm run dev
~~~

4. Go to [localhost:3000](http://localhost:3000)

### Production

1. Build the application
~~~shell
$ npm install

$ npm build
~~~

2. Run the application
~~~shell
$ npm start
~~~
## Next steps

#### [Dynamically update line items](https://docs.stripe.com/payments/advanced/dynamically-update-line-items.md)

Learn how to dynamically modify line items in your Checkout Session as users add, remove, or change products.

#### [Fulfill orders](https://docs.stripe.com/checkout/fulfillment.md?payment-ui=embedded-components)

Set up a webhook to fulfill orders after a payment succeeds. Webhooks are the most reliable way to handle business-critical events.

#### [Receive payouts](https://docs.stripe.com/payouts.md)

Learn how to move funds out of your Stripe account into your bank account.

#### [Refund and cancel payments](https://docs.stripe.com/refunds.md)

Handle requests for refunds by using the Stripe API or Dashboard.

#### [Customer management](https://docs.stripe.com/customer-management.md)

Let your customers self-manage their payment details, invoices, and subscriptions.

#### [One-click payment buttons](https://docs.stripe.com/elements/express-checkout-element/accept-a-payment.md?payment-ui=embedded-components#create-and-mount)

Use the Express Checkout Element to accept payments through one-click payment method buttons.

#### [Checkout studio](https://docs.stripe.com/payments/checkout-studio.md)

Configure and monitor your Checkout integration from the Dashboard without writing code.
