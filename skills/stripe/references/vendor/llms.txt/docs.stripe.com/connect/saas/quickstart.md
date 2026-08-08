# SaaS platform quickstart

# Build a SaaS platform integration 

> You can customize this guide by selecting integration options in the [Interactive platform guide](https://docs.stripe.com/connect/interactive-platform-guide.md).

Use this working code sample to integrate Stripe Hosted Checkout for payments with Stripe Connect. With this approach, Stripe hosts the checkout page for you and payments go directly to your connected accounts.

const SuccessDisplay = ({ sessionId }) => {
    return (
        <section>
            <div className="product Box-root">
                <div className="description Box-root">
                    <h3>Subscription to Starter Plan successful!</h3>
                </div>
            </div>
            <form action="/api/create-portal-session" method="POST">
              <input
                type="hidden"
                id="session-id"
                name="session_id"
                value={sessionId}
              />
              <button id="checkout-and-portal-button" className="button" type="submit">
                Manage your billing information
              </button>
            </form>
        </section>
    );
};
const Message = ({ message }) => (
    <section>
        <p>{message}</p>
    </section>
);
const SubscribeBtn = ({ accountId }) => {
    const handleSubscribe = async () => {
        if (!accountId) return;
        const response = await fetch(`/api/subscribe-to-platform`, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ accountId }),
        });

        if (!response.ok) {
            console.error("Subscription failed:", data);
            return;
        }
        const data = await response.json();
        const checkoutSessionUrl = data.url;
        window.location.href = checkoutSessionUrl;
    };

    return (
        <button onClick={handleSubscribe} className="button">
            Subscribe to platform
        </button>
    );
};

const SubscriptionToPlatformStatus = ({ accountId }) => {
    let [message, setMessage] = useState("");
    let [success, setSuccess] = useState(false);
    let [sessionId, setSessionId] = useState("");

    useEffect(() => {
        // Check to see if this is a redirect back from Checkout
        const query = new URLSearchParams(window.location.search);

        if (query.get("success")) {
            setSuccess(true);
            setSessionId(query.get("session_id"));
        }

        if (query.get("canceled")) {
            setSuccess(false);
            setMessage(
                "Order canceled -- continue to shop around and checkout when you're ready."
            );
        }
    }, [sessionId]);

    if (!success && message === "") {
        return <SubscribeBtn accountId={accountId} />;
    } else if (success && sessionId !== "") {
        return <SuccessDisplay sessionId={sessionId} />;
    } else {
        return <Message message={message} />;
    }
};
  const handleCreateProduct = async (formData) => {
    if (!accountId) return;
    if (needsOnboarding) return;

    const response = await fetch("/api/create-product", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ ...formData, accountId }),
    });

    const data = await response.json();
    setShowForm(false);
  };
         <SubscriptionToPlatformStatus accountId={accountId} />
export default function Page() {
  // Get the checkout session ID from the URL
  const [searchParams] = useSearchParams();
  const sessionId = searchParams.get("session_id");
  const [accountId] = useState(localStorage.getItem("accountId"));

  return (
    <div className="container">
      <p className="message">Your payment was successful</p>

    <a
      href={`https://dashboard.stripe.com/${accountId}`}
      className="button"
      target="_blank"
      rel="noopener noreferrer"
    >
      Go to Connected Account dashboard
    </a>
      {/* Show manage billing button */}
      <form action="/api/create-portal-session" method="POST">
        <input type="hidden" name="session_id" value={sessionId} />
        <button className="button" type="submit">
          Manage billing information
        </button>
      </form>
      <Link to="/" className="button">
        Back to products
      </Link>
    </div>
  );
}
  const handleCreateAccount = async (e) => {
    e.preventDefault();

    try {
      const response = await fetch("/api/create-connect-account", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ email }),
      });

      if (!response.ok) {
        throw new Error("Failed to create account");
      }

      const data = await response.json();
      // Update the account ID in the provider
      setAccountId(data.accountId);
    } catch (error) {
      console.error("Error creating account:", error);
    }
  };
  const handleStartOnboarding = async () => {
    try {
      const response = await fetch("/api/create-account-link", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ accountId }),
      });

      if (!response.ok) {
        throw new Error("Failed to create account link");
      }

      const data = await response.json();
      window.location.href = data.url;
    } catch (error) {
      console.error("Error creating account link:", error);
    }
  };
const Product = ({ name, price, priceId, period, image }) => {
  const { accountId } = useAccount();

  return (
    <div className="product round-border">
      <div className="product-info">
        <img src={image} alt={name} />
        <div className="description">
          <h3>{name}</h3>
          <h5>{price} {period && `/ ${period}`}</h5>
        </div>
      </div>

      <form action="/api/create-checkout-session" method="POST">
        <input type="hidden" name="priceId" value={priceId} />
        <input type="hidden" name="accountId" value={accountId} />
        <button className="button" type="submit">
          Checkout
        </button>
      </form>
    </div>
  );
};
  const fetchProducts = async () => {
    if (!accountId) return;
    if (needsOnboarding) return;
    const response = await fetch(`/api/products/${accountId}`);
    const data = await response.json();
    setProducts(data);
  };
const { Stripe } = require("stripe");
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY);
// Create a sample product and return a price for it
router.post("/create-product", async (req, res) => {
  const productName = req.body.productName;
  const productDescription = req.body.productDescription;
  const productPrice = req.body.productPrice;
  const accountId = req.body.accountId; // Get the connected account ID

  try {
    // Create the product on the connected account
    const product = await stripe.products.create(
      {
        name: productName,
        description: productDescription,
      },
      { stripeAccount: accountId }
    );

    // Create a price for the product on the connected account
    const price = await stripe.prices.create(
      {
        product: product.id,
        unit_amount: productPrice,
        currency: "usd",
      },
      { stripeAccount: accountId }
    );

    res.json({
      productName: productName,
      productDescription: productDescription,
      productPrice: productPrice,
      priceId: price.id,
    });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
// Create a Connected Account
router.post("/create-connect-account", async (req, res) => {
  try {
    // Create a Connect account with the specified controller properties
    const account = await stripe.v2.core.accounts.create({
      display_name: req.body.email,
      contact_email: req.body.email,
      // Set to "full" for the Stripe Dashboard or "express" for the Express Dashboard
      dashboard: "full",
      defaults: {
        responsibilities: {
          fees_collector: "stripe",
          losses_collector: "stripe",
        },
      },
      identity: {
        country: "US",
        entity_type: "company",
      },
      configuration: {
        merchant: {
          capabilities: {
            card_payments: { requested: true },
          },
        },
        customer: {},
      },
    });

    res.json({ accountId: account.id });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
// Create Account Link for onboarding
router.post("/create-account-link", async (req, res) => {
  const accountId = req.body.accountId;
  try {
    const accountLink = await stripe.v2.core.accountLinks.create({
      account: accountId,
      use_case: {
        type: 'account_onboarding',
        account_onboarding: {
          configurations: ['merchant', 'customer'],
          refresh_url: 'https://example.com',
          return_url: `https://example.com?accountId=${accountId}`,
        },
      },
    });
    res.json({ url: accountLink.url });
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});
// Fetch products for a specific account
router.get("/products/:accountId", async (req, res) => {
  const { accountId } = req.params;

  try {
    const options = {};
    // If accountId is not 'platform', fetch from connected account
    if (accountId !== "platform") {
      options.stripeAccount = accountId;
    }

    const prices = await stripe.prices.list(
      {
        expand: ["data.product"],
        active: true,
        limit: 100,
      },
      options
    );

    res.json(
      prices.data.map((price) => ({
        id: price.product.id,
        name: price.product.name,
        description: price.product.description,
        price: price.unit_amount,
        priceId: price.id,
        period: price.recurring ? price.recurring.interval : null,
        image: "https://i.imgur.com/6Mvijcm.png",
      }))
    );
  } catch (err) {
    console.error("Error fetching prices:", err);
    res.status(500).json({ error: err.message });
  }
});
// Create a subscription from the connected account to the platform
router.post("/subscribe-to-platform", async (req, res) => {
    const { accountId } = req.body;
    const priceId = process.env.PLATFORM_PRICE_ID; // Price ID created on the platform account
    const session = await stripe.checkout.sessions.create({
        mode: "subscription",
        line_items: [
            {
                price: priceId,
                quantity: 1,
            },
        ],
        // Pass the V2 Account ID
        customer_account: accountId,
        // Defines where Stripe will redirect a customer after successful payment
        success_url: `${process.env.DOMAIN}?session_id={CHECKOUT_SESSION_ID}&success=true`,
        // Defines where Stripe will redirect if a customer cancels payment
        cancel_url: `${process.env.DOMAIN}?canceled=true`,
    });
    return res.json({ url: session.url });
});
  const session = await stripe.checkout.sessions.create({
    line_items: [
      {
        price: priceId,
        quantity: 1,
      },
    ],
    mode: mode,
    // Defines where Stripe will redirect a customer after successful payment
    success_url: `${process.env.DOMAIN}/done?session_id={CHECKOUT_SESSION_ID}`,
    // Defines where Stripe will redirect if a customer cancels payment
    cancel_url: `${process.env.DOMAIN}`,
    ...(mode === 'subscription' ? {
      subscription_data: {
        application_fee_amount: 123,
      },
    } : {
      payment_intent_data: {
        application_fee_amount: 123,
      },
    }),
  }, {
    stripeAccount: accountId
  });
// Create a billing portal session
router.post("/create-portal-session", async (req, res) => {
  const { session_id } = req.body;
  // Get the Stripe customer we previously created
  // Normally you'd fetch this from your database based on the authenticated user
  const session = await stripe.checkout.sessions
    .retrieve(session_id);
   const portalSession = await stripe.billingPortal.sessions.create({
        // Set the customer_account to the V2 Account's ID
        customer_account: session.customer_account,
        return_url: `${process.env.DOMAIN}/?session_id=${session_id}`,
    });

  // Redirect to the billing portal
  res.redirect(303, portalSession.url);
});
router.post(
  "/webhook",
  express.raw({ type: "application/json" }),
  (request, response) => {
    let event = request.body;
    // Replace this endpoint secret with your endpoint's unique secret
    // If you are testing with the CLI, find the secret by running 'stripe listen'
    // If you are using an endpoint defined with the API or dashboard, look in your webhook settings
    // at https://dashboard.stripe.com/webhooks
    const endpointSecret = "";

    // Only verify the event if you have an endpoint secret defined.
    // Otherwise use the basic event deserialized with JSON.parse
    if (endpointSecret) {
      const signature = request.headers["stripe-signature"];
      try {
        event = stripe.webhooks.constructEvent(
          request.body,
          signature,
          endpointSecret
        );
      } catch (err) {
        console.log(`⚠️  Webhook signature verification failed.`, err.message);
        return response.sendStatus(400);
      }
    }

    let stripeObject;
    let status;
    // Handle the event
    switch (event.type) {
      case "customer.subscription.trial_will_end":
        stripeObject = event.data.object;
        status = stripeObject.status;
        console.log(`Subscription status is ${status}.`);
        // Then define and call a method to handle the subscription trial ending.
        // handleSubscriptionTrialEnding(stripeObject);
        break;
      case "customer.subscription.deleted":
        stripeObject = event.data.object;
        status = stripeObject.status;
        console.log(`Subscription status is ${status}.`);
        // Then define and call a method to handle the subscription deleted.
        // handleSubscriptionDeleted(stripeObject);
        break;
      case "checkout.session.completed":
        stripeObject = event.data.object;
        status = stripeObject.status;
        console.log(`Checkout Session status is ${status}.`);
        // Then define and call a method to handle the subscription deleted.
        // handleCheckoutSessionCompleted(stripeObject);
        break;
      case "checkout.session.async_payment_failed":
        stripeObject = event.data.object;
        status = stripeObject.status;
        console.log(`Checkout Session status is ${status}.`);
        // Then define and call a method to handle the subscription deleted.
        // handleCheckoutSessionFailed(stripeObject);
        break;

      default:
        // Unexpected event type
        console.log(`Unhandled event type ${event.type}.`);
    }
    // Return a 200 response to acknowledge receipt of the event
    response.send();
  }
);
Stripe.api_key = ENV["STRIPE_SECRET_KEY"]
stripe_client = Stripe::StripeClient.new(ENV["STRIPE_SECRET_KEY"])
\# Create a sample product and return a price for it
post "/api/create-product" do
  data = parse_request_body
  product_name = data['productName']
  product_description = data['productDescription']
  product_price = data['productPrice']
  account_id = data['accountId']

  begin
    # Create the product on the connected account
    product = Stripe::Product.create({
      name: product_name,
      description: product_description,
    }, { stripe_account: account_id })

    # Create a price for the product on the connected account
    price = Stripe::Price.create({
      product: product.id,
      unit_amount: product_price,
      currency: 'usd',
    }, { stripe_account: account_id })

    content_type :json
    {
      productName: product_name,
      productDescription: product_description,
      productPrice: product_price,
      priceId: price.id
    }.to_json
  rescue Stripe::StripeError => e
    status 500
    { error: e.message }.to_json
  end
end
\# Create a Connected Account
post "/api/create-connect-account" do
  data = parse_request_body

  begin
    account = stripe_client.v2.core.accounts.create({
      display_name: data['email'],
      contact_email: data['email'],
      \# Set to "full" for the Stripe Dashboard or "express" for the Express Dashboard
      dashboard: 'full',
      defaults: {
        responsibilities: {
          fees_collector: 'stripe',
          losses_collector: 'stripe',
        },
      },
      identity: {
        country: 'US',
        entity_type: 'company',
      },
      configuration: {
        customer: {},
        merchant: {
          capabilities: {
            card_payments: { requested: true },
          },
        },
      },
    })

    content_type :json
    { accountId: account.id }.to_json
  rescue Stripe::StripeError => e
    status 500
    { error: e.message }.to_json
  end
end
\# Create Account Link for onboarding
post "/api/create-account-link" do
  data = parse_request_body
  account_id = data['accountId']

  begin
    account_link = stripe_client.v2.core.account_links.create({
      account: account_id,
      use_case: {
        type: 'account_onboarding',
        account_onboarding: {
          configurations: ['merchant', 'customer'],
          refresh_url: 'https://example.com',
          return_url: "https://example.com?accountId=#{account_id}",
        },
      },
    })

    content_type :json
    { url: account_link.url }.to_json
  rescue Stripe::StripeError => e
    status 500
    { error: e.message }.to_json
  end
end
\# Fetch products for a specific account
get "/api/products/:account_id" do
  account_id = params[:account_id]

  begin
    options = {}
    if account_id != 'platform'
      options[:stripe_account] = account_id
    end

    prices = Stripe::Price.list({
      expand: ['data.product'],
      active: true,
      limit: 100,
    }, options)

    products = prices.data.map do |price|
      {
        id: price.product.id,
        name: price.product.name,
        description: price.product.description,
        price: price.unit_amount,
        priceId: price.id,
        image: 'https://i.imgur.com/6Mvijcm.png'
      }
    end

    content_type :json
    products.to_json
  rescue Stripe::StripeError => e
    status 500
    { error: e.message }.to_json
  end
end
\# Create a subscription from the connected account to the platform
post "/api/subscribe-to-platform" do
  data = parse_request_body
  account_id = data['accountId']
  price_id = ENV['PLATFORM_PRICE_ID'] # Price ID created on the platform account

  session = Stripe::Checkout::Session.create({
    mode: 'subscription',
    line_items: [{
      price: price_id,
      quantity: 1,
    }],
    # Pass the V2 Account ID
    customer_account: account_id,
    # Defines where Stripe will redirect a customer after successful payment
    success_url: "#{ENV['DOMAIN']}?session_id={CHECKOUT_SESSION_ID}&success=true",
    # Defines where Stripe will redirect if a customer cancels payment
    cancel_url: "#{ENV['DOMAIN']}?canceled=true",
  })

  content_type :json
  { url: session.url }.to_json
end
  session_params = {
    line_items: [
      {
        price: price_id,
        quantity: 1,
      },
    ],
    mode: mode,
    \# Defines where Stripe will redirect a customer after successful payment
    success_url: "#{ENV['DOMAIN']}/done?session_id={CHECKOUT_SESSION_ID}",
    # Defines where Stripe will redirect if a customer cancels payment
    cancel_url: "#{ENV['DOMAIN']}",
  }

  # Add Connect-specific parameters based on payment mode
  if mode == 'subscription'
    session_params[:subscription_data] = { application_fee_amount: 123 }
  else
    session_params[:payment_intent_data] = { application_fee_amount: 123 }
  end

  session = Stripe::Checkout::Session.create(session_params, {
    stripe_account: account_id
  })
\# Create a billing portal session
post "/api/create-portal-session" do
  # Get the Stripe customer we previously created
  # Normally you'd fetch this from your database based on the authenticated user
  data = parse_request_body
  session_id = data['session_id']
  checkout_session = Stripe::Checkout::Session.retrieve(session_id)
  portal_session = Stripe::BillingPortal::Session.create({
    # Set the customer_account to the V2 Account's ID
    customer_account: checkout_session.customer_account,
    return_url: "#{ENV['DOMAIN']}/?session_id=#{session_id}",
  })

  # Redirect to the billing portal
  redirect portal_session.url, 303
end
post "/api/webhook" do
  request.body.rewind
  payload = request.body.read

  \# Replace this endpoint secret with your endpoint's unique secret
  # If you are testing with the CLI, find the secret by running 'stripe listen'
  # If you are using an endpoint defined with the API or dashboard, look in your webhook settings
  # at https://dashboard.stripe.com/webhooks
  endpoint_secret = ""

  # Only verify the event if you have an endpoint secret defined.
  # Otherwise use the basic event deserialized directly.
  if endpoint_secret != ""
    signature = request.env["HTTP_STRIPE_SIGNATURE"]
    begin
      event = Stripe::Webhook.construct_event(payload, signature, endpoint_secret)
    rescue => e
      puts "⚠️  Webhook signature verification failed. #{e.message}"
      halt 400
    end
  else
    event = JSON.parse(payload, symbolize_names: true)
  end

  case event[:type]
  when "customer.subscription.trial_will_end"
    stripe_object = event[:data][:object]
    status = stripe_object[:status]
    puts "Subscription status is #{status}."
    \# handle_subscription_trial_ending(stripe_object)
  when "customer.subscription.deleted"
    stripe_object = event[:data][:object]
    status = stripe_object[:status]
    puts "Subscription status is #{status}."
    # handle_subscription_deleted(stripe_object)
  when "checkout.session.completed"
    stripe_object = event[:data][:object]
    status = stripe_object[:status]
    puts "Checkout Session status is #{status}."
    \# handle_checkout_session_completed(stripe_object)
  when "checkout.session.async_payment_failed"
    stripe_object = event[:data][:object]
    status = stripe_object[:status]
    puts "Checkout Session status is #{status}."
    # handle_checkout_session_failed(stripe_object)
  else
    # Unexpected event type
    puts "Unhandled event type #{event[:type]}."
  end

  status 200
end

post "/api/thin-webhook" do
  request.body.rewind
  payload = request.body.read

  # Replace this endpoint secret with your endpoint's unique secret
  # If you are testing with the CLI, find the secret by running 'stripe listen'
  # If you are using an endpoint defined with the API or dashboard, look in your webhook settings
  # at https://dashboard.stripe.com/webhooks
  thin_endpoint_secret = nil
  signature = request.env["HTTP_STRIPE_SIGNATURE"]
  begin
    event_notif = stripe_client.parse_event_notification(payload, signature, thin_endpoint_secret)
  rescue => e
    puts "⚠️  Webhook signature verification failed. #{e.message}"
    halt 400
  end

  if event_notif.type == "v2.account.created"
    stripe_object = event_notif.fetch_related_object
    event = event_notif.fetch_event
    # handle_v2_account_created(stripe_object)
  else
    puts "Unhandled event type #{event_notif.type}."
  end
  status 200
end
import stripe

from stripe import StripeClient
stripe.api_key = os.getenv('STRIPE_SECRET_KEY')
@app.route('/api/create-product', methods=['POST'])
def create_product():
    data = parse_request_body()
    product_name = data['productName']
    product_description = data['productDescription']
    product_price = data['productPrice']
    account_id = data['accountId']

    try:
        \# Create the product on the connected account
        product = stripe.Product.create(
            name=product_name,
            description=product_description,
            stripe_account=account_id
        )

        # Create a price for the product on the connected account
        price = stripe.Price.create(
            product=product.id,
            unit_amount=product_price,
            currency='usd',
            stripe_account=account_id
        )

        return jsonify({
            'productName': product_name,
            'productDescription': product_description,
            'productPrice': product_price,
            'priceId': price.id
        })
    except Exception as e:
        return jsonify({'error': str(e)}), 500
@app.route('/api/create-connect-account', methods=['POST'])
def create_connect_account():
    data = parse_request_body()

    try:
        account = stripe_client.v2.core.accounts.create({
            "display_name": data.get("email"),
            "contact_email": data.get("email"),
            \# Set to "full" for the Stripe Dashboard or "express" for the Express Dashboard
            "dashboard": "full",
            "defaults": {
                "responsibilities": {
                    "fees_collector": "stripe",
                    "losses_collector": "stripe",
                }
            },
            "identity": {
                "country": "US",
                "entity_type": "company",
            },
            "configuration": {
                "customer": {},
                "merchant": {
                    "capabilities": {
                        "card_payments": {"requested": True},
                    }
                },
            },
        })

        return jsonify({'accountId': account.id})
    except Exception as e:
        return jsonify({'error': str(e)}), 500
@app.route('/api/create-account-link', methods=['POST'])
def create_account_link():
    data = parse_request_body()
    account_id = data['accountId']

    try:
        account_link = stripe_client.v2.core.account_links.create({
            "account": account_id,
            "use_case": {
                "type": "account_onboarding",
                "account_onboarding": {
                    "configurations": ["merchant", "customer"],
                    "refresh_url": "https://example.com",
                    "return_url": f"https://example.com?accountId={account_id}",
                },
            },
        })

        return jsonify({'url': account_link.url})
    except Exception as e:
        return jsonify({'error': str(e)}), 500
@app.route('/api/products/<account_id>', methods=['GET'])
def get_products(account_id):
    try:
        if account_id != 'platform':
            prices = stripe.Price.list(
                expand=['data.product'],
                active=True,
                limit=100,
                stripe_account=account_id
            )
        else:
            prices = stripe.Price.list(
                expand=['data.product'],
                active=True,
                limit=100
            )

        products = []
        for price in prices.data:
            products.append({
                'id': price.product.id,
                'name': price.product.name,
                'description': price.product.description,
                'price': price.unit_amount,
                'priceId': price.id,
                'image': 'https://i.imgur.com/6Mvijcm.png'
            })

        return jsonify(products)
    except Exception as e:
        return jsonify({'error': str(e)}), 500
@app.route('/api/subscribe-to-platform', methods=['POST'])
def subscribe_to_platform():
    data = parse_request_body()
    account_id = data['accountId']
    price_id = os.getenv('PLATFORM_PRICE_ID')  \# Price ID created on the platform account

    session = stripe.checkout.Session.create(
        mode='subscription',
        line_items=[{
            'price': price_id,
            'quantity': 1,
        }],
        # Pass the V2 Account ID
        customer_account=account_id,
        # Defines where Stripe will redirect a customer after successful payment
        success_url=f"{os.getenv('DOMAIN')}?session_id={{CHECKOUT_SESSION_ID}}&success=true",
        # Defines where Stripe will redirect if a customer cancels payment
        cancel_url=f"{os.getenv('DOMAIN')}?canceled=true",
    )
    return jsonify({'url': session.url})
    checkout_session = stripe.checkout.Session.create(
      line_items=[
        {
          'price': price_id,
          'quantity': 1
        }
      ],
      mode=mode,
      \# Defines where Stripe will redirect a customer after successful payment
      success_url=f"{os.getenv('DOMAIN')}/done?session_id={{CHECKOUT_SESSION_ID}}",
      # Defines where Stripe will redirect if a customer cancels payment
      cancel_url=f"{os.getenv('DOMAIN')}",
      **(
        {
          'subscription_data': {
            'application_fee_amount': 123,
          }
        } if mode == 'subscription' else {
          'payment_intent_data': {
            'application_fee_amount': 123,
          }
        }
      ),
      stripe_account=account_id
    )
@app.route('/api/create-portal-session', methods=['POST'])
\# Create a billing portal session
def create_portal_session():
    # Get the Stripe customer we previously created
    # Normally you'd fetch this from your database based on the authenticated user
    data = parse_request_body()
    session_id = data['session_id']
    checkout_session = stripe.checkout.Session.retrieve(session_id)
    portal_session = stripe.billing_portal.Session.create(
        # Set the customer_account to the V2 Account's ID
        customer_account=checkout_session.customer_account,
        return_url=f"{os.getenv('DOMAIN')}/?session_id={session_id}",
    )

    # Redirect to the billing portal
    return redirect(portal_session.url, code=303)
@app.route('/api/webhook', methods=['POST'])
def webhook_received():
    \# Replace this endpoint secret with your endpoint's unique secret
    # If you are testing with the CLI, find the secret by running 'stripe listen'
    # If you are using an endpoint defined with the API or dashboard, look in your webhook settings
    # at https://dashboard.stripe.com/webhooks
    endpoint_secret = ''
    request_data = json.loads(request.data)

    # Only verify the event if you have an endpoint secret defined.
    # Otherwise use the basic event deserialized with JSON.parse
    if endpoint_secret:
        sig_header = request.headers.get('stripe-signature')
        try:
            event = stripe.Webhook.construct_event(
                request.data, sig_header, endpoint_secret
            )
        except stripe.error.SignatureVerificationError as e:
            app.logger.info('⚠️  Webhook signature verification failed.')
            return jsonify({'error': str(e)}), 400
    else:
        event = request_data

    # Handle the event
    match event['type']:
        case 'customer.subscription.trial_will_end':
            subscription = event['data']['object']
            status = subscription['status']
            app.logger.info(f'Subscription status is {status}.')
            \# Then define and call a method to handle the subscription trial ending.
            # handle_subscription_trial_ending(subscription);
        case 'customer.subscription.deleted':
            subscription = event['data']['object']
            status = subscription['status']
            app.logger.info(f'Subscription status is {status}.')
            # Then define and call a method to handle the subscription deleted.
            # handle_subscription_deleted(subscription);
        case 'checkout.session.completed':
            session = event['data']['object']
            status = session['status']
            app.logger.info(f'Checkout Session status is {status}.')
            \# Then define and call a method to handle the checkout session completed.
            # handle_checkout_session_completed(session);
        case 'checkout.session.async_payment_failed':
            session = event['data']['object']
            status = session['status']
            app.logger.info(f'Checkout Session status is {status}.')
            # Then define and call a method to handle the checkout session failed.
            # handle_checkout_session_failed(session);
        case _:
            # Unexpected event type
            app.logger.info(f'Unhandled event type {event["type"]}')

    # Return a 200 response to acknowledge receipt of the event
    return jsonify({'status': 'success'})
	stripe.Key = os.Getenv("STRIPE_SECRET_KEY")
	http.HandleFunc("/api/subscribe-to-platform", subscribeToPlatform)
	http.HandleFunc("/api/create-portal-session", createPortalSession)
	http.HandleFunc("/api/webhook", handleWebhook)
	http.HandleFunc("/api/thin-webhook", handleThinWebhook)
// Create a sample product and return a price for it
func createProduct(w http.ResponseWriter, r *http.Request) {
	data := parseRequestBody(r)
	productName := data["productName"].(string)
	productDescription := data["productDescription"].(string)
	productPrice := int64(data["productPrice"].(float64))
	accountId := data["accountId"].(string)

	var p *stripe.Product
	var pr *stripe.Price
	var err error

	// Create the product on the connected account
	productParams := &stripe.ProductParams{
		Name:        stripe.String(productName),
		Description: stripe.String(productDescription),
	}
	productParams.SetStripeAccount(accountId)
	p, err = product.New(productParams)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	// Create a price for the product on the connected account
	priceParams := &stripe.PriceParams{
		Product:    stripe.String(p.ID),
		UnitAmount: stripe.Int64(productPrice),
		Currency:   stripe.String("usd"),
	}
	priceParams.SetStripeAccount(accountId)
	pr, err = price.New(priceParams)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	// Return the product and price information
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(map[string]interface{}{
		"productName":        productName,
		"productDescription": productDescription,
		"productPrice":       productPrice,
		"priceId":            pr.ID,
	})
}
// Create a Connected Account
func createConnectAccount(w http.ResponseWriter, r *http.Request) {
	data := parseRequestBody(r)
	email := data["email"].(string)

	sc := stripe.NewClient(os.Getenv("STRIPE_SECRET_KEY"))
	displayName := email
	if displayName == "" {
		displayName = "Sample Store"
	}
	params := &stripe.V2CoreAccountCreateParams{
		ContactEmail: stripe.String(email),
		DisplayName:  stripe.String(displayName),
		Identity: &stripe.V2CoreAccountCreateIdentityParams{
			Country:     stripe.String("US"),
			EntityType:  stripe.String("company"),
		},
		Configuration: &stripe.V2CoreAccountCreateConfigurationParams{
		Customer: &stripe.V2CoreAccountCreateConfigurationCustomerParams{},
			Merchant: &stripe.V2CoreAccountCreateConfigurationMerchantParams{
				Capabilities: &stripe.V2CoreAccountCreateConfigurationMerchantCapabilitiesParams{
					CardPayments: &stripe.V2CoreAccountCreateConfigurationMerchantCapabilitiesCardPaymentsParams{
						Requested: stripe.Bool(true),
					},
				},
			},
		},
		Defaults: &stripe.V2CoreAccountCreateDefaultsParams{
			Responsibilities: &stripe.V2CoreAccountCreateDefaultsResponsibilitiesParams{
				FeesCollector:   stripe.String("stripe"),
				LossesCollector: stripe.String("stripe"),
			},
		},
    // Set to "full" for the Stripe Dashboard or "express" for the Express Dashboard
		Dashboard: stripe.String("full"),
	}
	result, err := sc.V2CoreAccounts.Create(context.TODO(), params)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(map[string]string{"accountId": result.ID})
}
// Create Account Link for onboarding
func createAccountLink(w http.ResponseWriter, r *http.Request) {
	data := parseRequestBody(r)
	accountId := data["accountId"].(string)

	sc := stripe.NewClient(os.Getenv("STRIPE_SECRET_KEY"))
	params := &stripe.V2CoreAccountLinkCreateParams{
		Account: stripe.String(accountId),
		UseCase: &stripe.V2CoreAccountLinkCreateUseCaseParams{
			Type: stripe.String("account_onboarding"),
			AccountOnboarding: &stripe.V2CoreAccountLinkCreateUseCaseAccountOnboardingParams{
				Configurations: []*string{
					stripe.String("merchant"),
					stripe.String("customer"),
				},
				RefreshURL: stripe.String("https://example.com"),
				ReturnURL:  stripe.String("https://example.com?accountId=" + accountId),
			},
		},
	}

	link, err := sc.V2CoreAccountLinks.Create(context.TODO(), params)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(map[string]string{"url": link.URL})
}
// Fetch products for a specific account
func getProducts(w http.ResponseWriter, r *http.Request) {
	accountId := r.URL.Path[len("/api/products/"):]


	params := &stripe.PriceListParams{
		Active: stripe.Bool(true),
	}
  params.Limit = stripe.Int64(100)
	params.AddExpand("data.product")

	if accountId != "platform" {
		params.SetStripeAccount(accountId)
	}

	var priceList = price.List(params).PriceList()

	var products []map[string]interface{}
	for _, p := range priceList.Data {
		products = append(products, map[string]interface{}{
			"id":          p.Product.ID,
			"name":        p.Product.Name,
			"description": p.Product.Description,
			"price":       p.UnitAmount,
			"priceId":     p.ID,
      "image":       "https://i.imgur.com/6Mvijcm.png",
		})
	}

	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(products)
}
// Create a subscription from the connected account to the platform
func subscribeToPlatform(w http.ResponseWriter, r *http.Request) {
  data := parseRequestBody(r)
  accountId := data["accountId"].(string)
  priceId := os.Getenv("PLATFORM_PRICE_ID") // Price ID created on the platform account

  params := &stripe.CheckoutSessionParams{
    Mode: stripe.String("subscription"),
    LineItems: []*stripe.CheckoutSessionLineItemParams{
      {
        Price: stripe.String(priceId),
        Quantity: stripe.Int64(1),
      },
    },
    // Pass the V2 Account ID
    CustomerAccount: stripe.String(accountId),
    // Defines where Stripe will redirect a customer after successful payment
    SuccessURL: stripe.String(os.Getenv("DOMAIN") + "?session_id={CHECKOUT_SESSION_ID}&success=true"),
    // Defines where Stripe will redirect if a customer cancels payment
    CancelURL: stripe.String(os.Getenv("DOMAIN") + "?canceled=true"),
  }

  session, err := checkoutSession.New(params)
  if err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
    return
  }

  w.Header().Set("Content-Type", "application/json")
  json.NewEncoder(w).Encode(map[string]string{"url": session.URL})
}
// Create checkout session
func createCheckoutSession(w http.ResponseWriter, r *http.Request) {
  data := parseRequestBody(r)
  accountId := data["accountId"].(string)
  priceId := data["priceId"].(string)

  // Get the price's type from Stripe
  var p *stripe.Price
  params := &stripe.PriceParams{}
  params.SetStripeAccount(accountId)
  p, _ = price.Get(priceId, params)
  priceType := p.Type
  var mode string
  if priceType == "recurring" {
    mode = "subscription"
  } else {
    mode = "payment"
  }

	// Build the basic checkout session params
	checkoutSessionParams := &stripe.CheckoutSessionParams{
		LineItems: []*stripe.CheckoutSessionLineItemParams{
			{
				Price:    stripe.String(priceId),
				Quantity: stripe.Int64(1),
			},
		},
		Mode: stripe.String(mode),
	}

	// Defines where Stripe will redirect a customer after successful payment
	checkoutSessionParams.SuccessURL = stripe.String(os.Getenv("DOMAIN") + "/done?session_id={CHECKOUT_SESSION_ID}")
	// Defines where Stripe will redirect if a customer cancels payment
	checkoutSessionParams.CancelURL = stripe.String(os.Getenv("DOMAIN"))

	// Add Connect-specific parameters based on payment mode
	if mode == "subscription" {
		checkoutSessionParams.SubscriptionData = &stripe.CheckoutSessionSubscriptionDataParams{
			ApplicationFeeAmount: stripe.Int64(123),
		}
	} else {
		checkoutSessionParams.PaymentIntentData = &stripe.CheckoutSessionPaymentIntentDataParams{
			ApplicationFeeAmount: stripe.Int64(123),
		}
	}
	checkoutSessionParams.SetStripeAccount(accountId)
	s, err := checkoutSession.New(checkoutSessionParams)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	// Redirect to the Stripe hosted checkout URL
	http.Redirect(w, r, s.URL, http.StatusSeeOther)
}
// Create a billing portal session
func createPortalSession(w http.ResponseWriter, r *http.Request) {
  sessionId := parseRequestBody(r)["session_id"].(string)
  sess, _ := checkoutSession.Get(sessionId, nil)
  // Create a billing portal session
  params := &stripe.BillingPortalSessionParams{
    // Set the customer_account to the V2 Account's ID
    CustomerAccount: stripe.String(sess.CustomerAccount),
    ReturnURL: stripe.String(os.Getenv("DOMAIN") + "/?session_id=" + sessionId),
  }
	portal, _ := portalSession.New(params)

	// Redirect to the billing portal
	http.Redirect(w, r, portal.URL, http.StatusSeeOther)
}
// Stripe webhook endpoint
func handleWebhook(w http.ResponseWriter, r *http.Request) {
	payload, _ := io.ReadAll(r.Body)

	// Replace this endpoint secret with your endpoint's unique secret
	// If you are testing with the CLI, find the secret by running 'stripe listen'
	// If you are using an endpoint defined with the API or dashboard, look in your webhook settings
	// at https://dashboard.stripe.com/webhooks
	endpointSecret := ""

	var event stripe.Event
	if endpointSecret != "" {
		signature := r.Header.Get("Stripe-Signature")
		e, err := webhook.ConstructEvent(payload, signature, endpointSecret)
		if err != nil {
			return
		}
		event = e
	} else {
		json.Unmarshal(payload, &event)
	}

	// Handle the event
	switch event.Type {
	case "customer.subscription.trial_will_end":
		var subscription stripe.Subscription
		json.Unmarshal(event.Data.Raw, &subscription)
		status := subscription.Status
		log.Printf("Subscription status is %s.\n", status)
		// Then define and call a method to handle the subscription trial ending.
		// handleSubscriptionTrialEnding(subscription);

	case "customer.subscription.deleted":
		var subscription stripe.Subscription
		json.Unmarshal(event.Data.Raw, &subscription)
		status := subscription.Status
		log.Printf("Subscription status is %s.\n", status)
		// Then define and call a method to handle the subscription deleted.
		// handleSubscriptionDeleted(subscription);
	case "checkout.session.completed":
		var session stripe.CheckoutSession
		json.Unmarshal(event.Data.Raw, &session)
		status := session.Status
		log.Printf("Checkout Session status is %s.\n", status)
		// Then define and call a method to handle the checkout session completed.
		// handleCheckoutSessionCompleted(session);

	case "checkout.session.async_payment_failed":
		var session stripe.CheckoutSession
		json.Unmarshal(event.Data.Raw, &session)
		status := session.Status
		log.Printf("Checkout Session status is %s.\n", status)
		// Then define and call a method to handle the checkout session failed.
		// handleCheckoutSessionFailed(session);
	default:
		// Unexpected event type
		log.Printf("Unhandled event type %s.\n", event.Type)
	}

	// Return a 200 response to acknowledge receipt of the event
	w.WriteHeader(http.StatusOK)
}
$stripe = new \Stripe\StripeClient([
  "api_key" => $_ENV['STRIPE_SECRET_KEY'],
]);
// Create a sample product and return a price for it
if ($_SERVER['REQUEST_URI'] === '/api/create-product' && $_SERVER['REQUEST_METHOD'] === 'POST') {
  $data = parseRequestBody();
  $productName = $data['productName'];
  $productDescription = $data['productDescription'];
  $productPrice = $data['productPrice'];
  $accountId = $data['accountId'];

  try {
    // Create the product on the connected account
    $product = $stripe->products->create([
      'name' => $productName,
      'description' => $productDescription,
    ], ['stripe_account' => $accountId]);

    // Create a price for the product on the connected account
    $price = $stripe->prices->create([
      'product' => $product->id,
      'unit_amount' => $productPrice,
      'currency' => 'usd',
    ], ['stripe_account' => $accountId]);

    echo json_encode([
      'productName' => $productName,
      'productDescription' => $productDescription,
      'productPrice' => $productPrice,
      'priceId' => $price->id,
    ]);
  } catch (Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => $e->getMessage()]);
  }
  exit;
}
// Create a Connected Account
if ($_SERVER['REQUEST_URI'] === '/api/create-connect-account' && $_SERVER['REQUEST_METHOD'] === 'POST') {
  $data = parseRequestBody();

  try {
    $account = $stripe->v2->core->accounts->create([
      'display_name' => $data['email'],
      'contact_email' => $data['email'],
      // Set to "full" for the Stripe Dashboard or "express" for the Express Dashboard
      'dashboard' => 'full',
      'defaults' => [
        'responsibilities' => [
          'fees_collector' => 'stripe',
          'losses_collector' => 'stripe',
        ],
      ],
      'identity' => [
        'country' => 'US',
        'entity_type' => 'company',
      ],
      'configuration' => [
        'customer' => (object)[],
        'merchant' => [
          'capabilities' => [
            'card_payments' => ['requested' => true],
          ],
        ],
      ],
    ]);

    echo json_encode(['accountId' => $account->id]);
  } catch (Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => $e->getMessage()]);
  }
  exit;
}
// Create Account Link for onboarding
if ($_SERVER['REQUEST_URI'] === '/api/create-account-link' && $_SERVER['REQUEST_METHOD'] === 'POST') {
  $data = parseRequestBody();
  $accountId = $data['accountId'];

  try {
    $accountLink = $stripe->v2->core->accountLinks->create([
      'account' => $accountId,
      'use_case' => [
        'type' => 'account_onboarding',
        'account_onboarding' => [
          'configurations' => ['merchant', 'customer'],
          'refresh_url' => 'https://example.com',
          'return_url' => 'https://example.com?accountId=' . $accountId,
        ],
      ],
    ]);

    echo json_encode(['url' => $accountLink->url]);
  } catch (Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => $e->getMessage()]);
  }
  exit;
}
// Fetch products for a specific account
if (preg_match('/^\/api\/products\/(.+)$/', $_SERVER['REQUEST_URI'], $matches) && $_SERVER['REQUEST_METHOD'] === 'GET') {
  $accountId = $matches[1];

  try {
    $options = [];
    if ($accountId !== 'platform') {
      $options['stripe_account'] = $accountId;
    }

    $prices = $stripe->prices->all([
      'expand' => ['data.product'],
      'active' => true,
      'limit' => 100,
    ], $options);

    $products = [];
    foreach ($prices->data as $price) {
      $products[] = [
        'id' => $price->product->id,
        'name' => $price->product->name,
        'description' => $price->product->description,
        'price' => $price->unit_amount,
        'priceId' => $price->id,
        'image' => 'https://i.imgur.com/6Mvijcm.png',
      ];
    }

    echo json_encode($products);
  } catch (Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => $e->getMessage()]);
  }
  exit;
}
// Create a subscription from the connected account to the platform
if ($_SERVER['REQUEST_URI'] === '/api/subscribe-to-platform' && $_SERVER['REQUEST_METHOD'] === 'POST') {
  $data = parseRequestBody();
  $accountId = $data['accountId'];
  $priceId = $_ENV['PLATFORM_PRICE_ID']; // Price ID created on the platform account

  try {
    $session = $stripe->checkout->sessions->create([
      'mode' => 'subscription',
      'line_items' => [[
        'price' => $priceId,
        'quantity' => 1,
      ]],
      // Pass the V2 Account ID
      'customer_account' => $accountId,
      // Defines where Stripe will redirect a customer after successful payment
      'success_url' => $_ENV['DOMAIN'] . '?session_id={CHECKOUT_SESSION_ID}&success=true',
      // Defines where Stripe will redirect if a customer cancels payment
      'cancel_url' => $_ENV['DOMAIN'] . '?canceled=true',
    ]);

    echo json_encode(['url' => $session->url]);
  } catch (Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => $e->getMessage()]);
  }
  exit;
}
  $checkout_params = [
    'line_items' => [[
      'price' => $priceId,
      'quantity' => 1,
    ]],
    'mode' => $mode,
    // Defines where Stripe will redirect a customer after successful payment
    'success_url' => $_ENV['DOMAIN'] . '/done?session_id={CHECKOUT_SESSION_ID}',
    // Defines where Stripe will redirect if a customer cancels payment
    'cancel_url' => $_ENV['DOMAIN'],
  ];

  // Add Connect-specific parameters based on payment mode
  if ($mode === 'subscription') {
    $checkout_params['subscription_data'] = ['application_fee_amount' => 123];
  } else {
    $checkout_params['payment_intent_data'] = ['application_fee_amount' => 123];
  }

  $checkout_session = $stripe->checkout->sessions->create(
    $checkout_params,
    ['stripe_account' => $accountId]
  );
// Create a billing portal session
if ($_SERVER['REQUEST_URI'] === '/api/create-portal-session' && $_SERVER['REQUEST_METHOD'] === 'POST') {
  // Get the Stripe customer we previously created
  // Normally you'd fetch this from your database based on the authenticated user
  $data = parseRequestBody();
  $sessionId = $data['session_id'];
  $session = $stripe->checkout->sessions->retrieve($sessionId);
  // Create a billing portal session
  $portal_session = $stripe->billingPortal->sessions->create([
    // Set the customer_account to the V2 Account's ID
    'customer_account' => $session->customer_account,
    'return_url' => $_ENV['DOMAIN'] . '/?session_id=' . $sessionId,
  ]);

  // Redirect to the billing portal
  header('Location: ' . $portal_session->url, true, 303);
  exit;
}
if ($_SERVER['REQUEST_URI'] === '/api/webhook' && $_SERVER['REQUEST_METHOD'] === 'POST') {
  $payload = @file_get_contents('php://input');
  $event = null;

  // Replace this endpoint secret with your endpoint's unique secret
  // If you are testing with the CLI, find the secret by running 'stripe listen'
  // If you are using an endpoint defined with the API or dashboard, look in your webhook settings
  // at https://dashboard.stripe.com/webhooks
  $endpoint_secret = '';

  // Only verify the event if you have an endpoint secret defined.
  // Otherwise use the basic event deserialized with json_decode
  if ($endpoint_secret) {
    try {
      $sig_header = $_SERVER['HTTP_STRIPE_SIGNATURE'];
      $event = $stripe->webhooks->constructEvent(
        $payload, $sig_header, $endpoint_secret
      );
    } catch(\UnexpectedValueException $e) {
      http_response_code(400);
      exit();
    } catch(\Stripe\Exception\SignatureVerificationException $e) {
      http_response_code(400);
      exit();
    }
  } else {
    $event = json_decode($payload);
  }

  $stripeObject = null;
  $status = null;

  // Handle the event
  switch ($event->type) {
    case 'customer.subscription.trial_will_end':
      $stripeObject = $event->data->object;
      $status = $stripeObject->status;
      error_log("Subscription status is " . $status);
      // Then define and call a method to handle the subscription trial ending.
      // handleSubscriptionTrialEnding($stripeObject);
      break;
    case 'customer.subscription.deleted':
      $stripeObject = $event->data->object;
      $status = $stripeObject->status;
      error_log("Subscription status is " . $status);
      // Then define and call a method to handle the subscription deleted.
      // handleSubscriptionDeleted($stripeObject);
      break;
    case 'checkout.session.completed':
      $stripeObject = $event->data->object;
      $status = $stripeObject->status;
      error_log("Checkout Session status is " . $status);
      // Then define and call a method to handle the checkout session completed.
      // handleCheckoutSessionCompleted($stripeObject);
      break;
    case 'checkout.session.async_payment_failed':
      $stripeObject = $event->data->object;
      $status = $stripeObject->status;
      error_log("Checkout Session status is " . $status);
      // Then define and call a method to handle the checkout session failed.
      // handleCheckoutSessionFailed($stripeObject);
      break;
    default:
      error_log('Unhandled event type ' . $event->type);
  }

  // Return a 200 response to acknowledge receipt of the event
  http_response_code(200);
  exit();
}
        Stripe.apiKey = dotenv.get("STRIPE_SECRET_KEY");
        // v2 API client
        StripeClient v2Client = new StripeClient(dotenv.get("STRIPE_SECRET_KEY"));
        // Create a sample product and return a price for it
        post("/api/create-product", (request, response) -> {
            String productName = parseRequestBody(request, "productName");
            String productDescription = parseRequestBody(request, "productDescription");
            Long productPrice = Long.parseLong(parseRequestBody(request, "productPrice"));
            String accountId = parseRequestBody(request, "accountId");

            try {
                Product product;
                Price price;

                // Set the request to be made on the connected account
                RequestOptions requestOptions = RequestOptions.builder()
                    .setStripeAccount(accountId)
                    .build();

                // Create the product on the connected account
                ProductCreateParams productParams = ProductCreateParams.builder()
                    .setName(productName)
                    .setDescription(productDescription)
                    .build();
                product = Product.create(productParams, requestOptions);

                // Create a price for the product on the connected account
                PriceCreateParams priceParams = PriceCreateParams.builder()
                    .setProduct(product.getId())
                    .setUnitAmount(productPrice)
                    .setCurrency("usd")
                    .build();
                price = Price.create(priceParams, requestOptions);

                return new Gson().toJson(Map.of(
                    "productName", productName,
                    "productDescription", productDescription,
                    "productPrice", productPrice,
                    "priceId", price.getId()
                ));
            } catch (StripeException e) {
                response.status(500);
                return new Gson().toJson(Map.of("error", e.getMessage()));
            }
        });
        // Create a Connected Account
        post("/api/create-connect-account", (request, response) -> {
            String email = parseRequestBody(request, "email");

            try {
                // Create v2 Account
                AccountCreateParams params =
                    AccountCreateParams.builder()
                        .setContactEmail(email)
                        .setDisplayName(email)
                        .setIdentity(
                            AccountCreateParams.Identity.builder()
                                .setCountry(AccountCreateParams.Identity.Country.US)
                                .setEntityType(AccountCreateParams.Identity.EntityType.COMPANY)
                                .build()
                        )
                        .setConfiguration(
                            AccountCreateParams.Configuration.builder()
                        .setCustomer(
                                    AccountCreateParams.Configuration.Customer.builder()
                                        .build()
                                )
                                .setMerchant(
                                    AccountCreateParams.Configuration.Merchant.builder()
                                        .setCapabilities(
                                            AccountCreateParams.Configuration.Merchant.Capabilities.builder()
                                                .setCardPayments(
                                                    AccountCreateParams.Configuration.Merchant.Capabilities.CardPayments.builder()
                                                        .setRequested(true)
                                                        .build()
                                                )
                                                .build()
                                        )
                                        .build()
                                )
                                .build()
                        )
                        .setDefaults(
                            AccountCreateParams.Defaults.builder()
                                .setResponsibilities(
                                    AccountCreateParams.Defaults.Responsibilities.builder()
                                        .setFeesCollector(AccountCreateParams.Defaults.Responsibilities.FeesCollector.STRIPE)
                                        .setLossesCollector(AccountCreateParams.Defaults.Responsibilities.LossesCollector.STRIPE)
                                        .build()
                              )
                              .build()
                        )
                        // Set to FULL for the Stripe Dashboard or EXPRESS for the Express Dashboard
                        .setDashboard(AccountCreateParams.Dashboard.FULL)
                        .build();

                Account account = v2Client.v2().core().accounts().create(params);
                return new Gson().toJson(Map.of("accountId", account.getId()));
            } catch (StripeException e) {
                response.status(500);
                return new Gson().toJson(Map.of("error", e.getMessage()));
            }
        });
        // Create Account Link for onboarding
        post("/api/create-account-link", (request, response) -> {
            String accountId = parseRequestBody(request, "accountId");

            try {

                com.stripe.param.v2.core.AccountLinkCreateParams params =
                    com.stripe.param.v2.core.AccountLinkCreateParams.builder()
                        .setAccount(accountId)
                        .setUseCase(
                            com.stripe.param.v2.core.AccountLinkCreateParams.UseCase.builder()
                                .setType(com.stripe.param.v2.core.AccountLinkCreateParams.UseCase.Type.ACCOUNT_ONBOARDING)
                                .setAccountOnboarding(
                                    com.stripe.param.v2.core.AccountLinkCreateParams.UseCase.AccountOnboarding.builder()
                                        .addConfiguration(com.stripe.param.v2.core.AccountLinkCreateParams.UseCase.AccountOnboarding.Configuration.MERCHANT)
                                        .addConfiguration(com.stripe.param.v2.core.AccountLinkCreateParams.UseCase.AccountOnboarding.Configuration.CUSTOMER)
                                        // Due to a bug with V2 account links localhost is currently not supported. You will need to manually update the URL in your browser until the bug is fixed
                                        .setRefreshUrl("https://example.com")
                                        .setReturnUrl("https://example.com?accountId=" + accountId)
                                        .build()
                                )
                                .build()
                        )
                        .build();

                com.stripe.model.v2.core.AccountLink accountLink = v2Client.v2().core().accountLinks().create(params);
                return new Gson().toJson(Map.of("url", accountLink.getUrl()));
            } catch (StripeException e) {
                response.status(500);
                return new Gson().toJson(Map.of("error", e.getMessage()));
            }
        });
        // Fetch products for a specific account
        get("/api/products/:accountId", (request, response) -> {
            String accountId = request.params(":accountId");

            try {
                List<Map<String, Object>> productsList = new ArrayList<>();
                PriceListParams params = PriceListParams.builder()
                    .setActive(true)
                    .setLimit(100L)
                    .addExpand("data.product")
                    .build();

                RequestOptions requestOptions = null;
                if (!accountId.equals("platform")) {
                    requestOptions = RequestOptions.builder()
                        .setStripeAccount(accountId)
                        .build();
                }

                StripeCollection<Price> prices = requestOptions != null
                    ? Price.list(params, requestOptions)
                    : Price.list(params);

                for (Price price : prices.getData()) {
                    Product product = (Product) price.getProductObject();
                    productsList.add(Map.of(
                        "id", product.getId(),
                        "name", product.getName(),
                        "description", product.getDescription(),
                        "price", price.getUnitAmount(),
                        "priceId", price.getId(),
                        "image", "https://i.imgur.com/6Mvijcm.png"
                    ));
                }

                return new Gson().toJson(productsList);
            } catch (StripeException e) {
                response.status(500);
                return new Gson().toJson(Map.of("error", e.getMessage()));
            }
        });
        // Create a subscription from the connected account to the platform
        post("/api/subscribe-to-platform", (request, response) -> {
            String accountId = parseRequestBody(request, "accountId");
            String priceId = dotenv.get("PLATFORM_PRICE_ID"); // Price ID created on the platform account

            SessionCreateParams params = SessionCreateParams.builder()
                .setMode(SessionCreateParams.Mode.SUBSCRIPTION)
                .addLineItem(
                    SessionCreateParams.LineItem.builder()
                        .setPrice(priceId)
                        .setQuantity(1L)
                        .build()
                )
                // Pass the V2 Account ID
                .setCustomerAccount(accountId)
                // Defines where Stripe will redirect a customer after successful payment
                .setSuccessUrl(dotenv.get("DOMAIN") + "?session_id={CHECKOUT_SESSION_ID}&success=true")
                // Defines where Stripe will redirect if a customer cancels payment
                .setCancelUrl(dotenv.get("DOMAIN") + "?canceled=true")
                .build();

            Session session = Session.create(params);
            return new Gson().toJson(Map.of("url", session.getUrl()));
        });
            SessionCreateParams.Builder paramsBuilder = SessionCreateParams.builder()
                .addLineItem(
                    SessionCreateParams.LineItem.builder()
                        .setPrice(priceId)
                        .setQuantity(1L)
                        .build()
                )
                .setMode(mode)
                // Defines where Stripe will redirect a customer after successful payment
                .setSuccessUrl(dotenv.get("DOMAIN") + "/done?session_id={CHECKOUT_SESSION_ID}")
                // Defines where Stripe will redirect if a customer cancels payment
                .setCancelUrl(dotenv.get("DOMAIN"))
                ;

            // Add Connect-specific parameters based on payment mode
            if (mode == SessionCreateParams.Mode.SUBSCRIPTION) {
                paramsBuilder.setSubscriptionData(
                    SessionCreateParams.SubscriptionData.builder()
                        .setApplicationFeePercent(java.math.BigDecimal.valueOf(10))
                        .build()
                );
            } else {
                paramsBuilder.setPaymentIntentData(
                    SessionCreateParams.PaymentIntentData.builder()
                        .setApplicationFeeAmount(123L)
                        .build()
                );
            }

            requestOptions = RequestOptions.builder()
                .setStripeAccount(accountId)
                .build();

            Session session = Session.create(paramsBuilder.build(), requestOptions);
        post("/api/create-portal-session", (request, response) -> {
            // Get the Stripe customer we previously created
            // Normally you'd fetch this from your database based on the authenticated user
            String sessionId = parseRequestBody(request, "session_id");
            Session checkoutSession = Session.retrieve(sessionId);
            com.stripe.param.billingportal.SessionCreateParams portalParams =
                com.stripe.param.billingportal.SessionCreateParams.builder()
                // Set the customer_account to the V2 Account's ID
                .setCustomerAccount(checkoutSession.getCustomerAccount())
                .setReturnUrl(dotenv.get("DOMAIN") + "/?session_id=" + sessionId)
                .build();

            com.stripe.model.billingportal.Session portalSession =
                com.stripe.model.billingportal.Session.create(portalParams);

            response.redirect(portalSession.getUrl(), 303);
            return "";
        });
        post("/api/webhook", (request, response) -> {
            String payload = request.body();
            String sigHeader = request.headers("Stripe-Signature");
            Event event = null;

            // Replace this endpoint secret with your endpoint's unique secret
            // If you are testing with the CLI, find the secret by running 'stripe listen'
            // If you are using an endpoint defined with the API or dashboard, look in your webhook settings
            // at https://dashboard.stripe.com/webhooks
            String endpointSecret = "";

            // Only verify the event if you have an endpoint secret defined.
            // Otherwise use the basic event deserialized with GSON.fromJson
            if (endpointSecret != null && !endpointSecret.isEmpty()) {
                try {
                    event = v2Client.constructEvent(payload, sigHeader, endpointSecret);
                } catch (Exception e) {
                    response.status(400);
                    return "";
                }
            } else {
                // For testing without a real signature
                try {
                    event = Event.GSON.fromJson(payload, Event.class);
                } catch (Exception e) {
                    response.status(400);
                    return "";
                }
            }

            // Handle the event
            StripeObject stripeObject;
            String status;
            EventDataObjectDeserializer dataObjectDeserializer;

            switch (event.getType()) {
                case "customer.subscription.trial_will_end":
                    dataObjectDeserializer = event.getDataObjectDeserializer();
                    if (dataObjectDeserializer.getObject().isPresent()) {
                        stripeObject = dataObjectDeserializer.getObject().get();
                        Subscription subscription = (Subscription) stripeObject;
                        status = subscription.getStatus();
                        System.out.println("Subscription status is " + status);
                        // Then define and call a method to handle the subscription trial ending.
                        // handleSubscriptionTrialEnding(subscription);
                    }
                    break;
                case "customer.subscription.deleted":
                    dataObjectDeserializer = event.getDataObjectDeserializer();
                    if (dataObjectDeserializer.getObject().isPresent()) {
                        stripeObject = dataObjectDeserializer.getObject().get();
                        Subscription subscription = (Subscription) stripeObject;
                        status = subscription.getStatus();
                        System.out.println("Subscription status is " + status);
                        // Then define and call a method to handle the subscription deleted.
                        // handleSubscriptionDeleted(subscription);
                    }
                    break;
                case "checkout.session.completed":
                    dataObjectDeserializer = event.getDataObjectDeserializer();
                    if (dataObjectDeserializer.getObject().isPresent()) {
                        stripeObject = dataObjectDeserializer.getObject().get();
                        Session session = (Session) stripeObject;
                        status = session.getStatus();
                        System.out.println("Checkout Session status is " + status);
                        // Then define and call a method to handle the checkout session completed.
                        // handleCheckoutSessionCompleted(session);
                    }
                    break;
                case "checkout.session.async_payment_failed":
                    dataObjectDeserializer = event.getDataObjectDeserializer();
                    if (dataObjectDeserializer.getObject().isPresent()) {
                        stripeObject = dataObjectDeserializer.getObject().get();
                        Session session = (Session) stripeObject;
                        status = session.getStatus();
                        System.out.println("Checkout Session status is " + status);
                        // Then define and call a method to handle the checkout session failed.
                        // handleCheckoutSessionFailed(session);
                    }
                    break;
                default:
                    System.out.println("Unhandled event type: " + event.getType());
            }

            response.status(200);
            return "";
        });
StripeConfiguration.ApiKey = System.Environment.GetEnvironmentVariable("STRIPE_SECRET_KEY");
// Create a sample product and return a price for it
app.MapPost("/api/create-product", async (HttpContext context) =>
{
    var requestData = await ParseRequestBody(context);
    string productName = requestData.GetProperty("productName").GetString();
    string productDescription = requestData.GetProperty("productDescription").GetString();
    long productPrice = requestData.GetProperty("productPrice").GetInt64();
    string accountId = requestData.GetProperty("accountId").GetString();

    try
    {
        var productService = new ProductService();
        var priceService = new PriceService();
        Product product;
        Price price;

        // Set the request to be made on the connected account
        var requestOptions = new RequestOptions
        {
            StripeAccount = accountId
        };

        // Create the product on the connected account
        var productOptions = new ProductCreateOptions
        {
            Name = productName,
            Description = productDescription,
        };
        product = await productService.CreateAsync(productOptions, requestOptions);

        // Create a price for the product on the connected account
        var priceOptions = new PriceCreateOptions
        {
            Product = product.Id,
            UnitAmount = productPrice,
            Currency = "usd",
        };
        price = await priceService.CreateAsync(priceOptions, requestOptions);

        await context.Response.WriteAsJsonAsync(new
        {
            productName,
            productDescription,
            productPrice,
            priceId = price.Id
        });
    }
    catch (Exception e)
    {
        context.Response.StatusCode = 500;
        await context.Response.WriteAsJsonAsync(new { error = e.Message });
    }
});
// Create a Connected Account
app.MapPost("/api/create-connect-account", async (HttpContext context) =>
{
    var requestData = await ParseRequestBody(context);
    string email = requestData.GetProperty("email").GetString();

    try
    {
        // Create v2 Connect account via typed client
        var options = new Stripe.V2.Core.AccountCreateOptions
        {
            ContactEmail = email,
            DisplayName = email,
            Identity = new Stripe.V2.Core.AccountCreateIdentityOptions
            {
                Country = "US",
                EntityType = "company",
            },
            Configuration = new Stripe.V2.Core.AccountCreateConfigurationOptions
            {
            Customer = new Stripe.V2.Core.AccountCreateConfigurationCustomerOptions
                {
                },
                Merchant = new Stripe.V2.Core.AccountCreateConfigurationMerchantOptions
                {
                    Capabilities = new Stripe.V2.Core.AccountCreateConfigurationMerchantCapabilitiesOptions
                    {
                        CardPayments = new Stripe.V2.Core.AccountCreateConfigurationMerchantCapabilitiesCardPaymentsOptions
                        {
                            Requested = true,
                        },
                    },
                },
            },
            Defaults = new Stripe.V2.Core.AccountCreateDefaultsOptions
            {
                Responsibilities = new Stripe.V2.Core.AccountCreateDefaultsResponsibilitiesOptions
                {
                    FeesCollector = "stripe",
                    LossesCollector = "stripe",
                },
            },
            // Set to "full" for the Stripe Dashboard or "express" for the Express Dashboard
            Dashboard = "full",
            Include = new List<string>
            {
                "configuration.merchant",
                "requirements",
            },
        };
        var client = new StripeClient(System.Environment.GetEnvironmentVariable("STRIPE_SECRET_KEY"));
        var service = client.V2.Core.Accounts;
        Stripe.V2.Core.Account account = service.Create(options);
        await context.Response.WriteAsJsonAsync(new { accountId = account.Id });
    }
    catch (Exception e)
    {
        context.Response.StatusCode = 500;
        await context.Response.WriteAsJsonAsync(new { error = e.Message });
    }
});
// Create Account Link for onboarding
app.MapPost("/api/create-account-link", async (HttpContext context) =>
{
    var requestData = await ParseRequestBody(context);
    string accountId = requestData.GetProperty("accountId").GetString();

    try
    {
        var client = new StripeClient(System.Environment.GetEnvironmentVariable("STRIPE_SECRET_KEY"));
        var service = client.V2.Core.AccountLinks;
        var options = new Stripe.V2.Core.AccountLinkCreateOptions
        {
            Account = accountId,
            UseCase = new Stripe.V2.Core.AccountLinkCreateUseCaseOptions
            {
                Type = "account_onboarding",
                AccountOnboarding = new Stripe.V2.Core.AccountLinkCreateUseCaseAccountOnboardingOptions
                {
                    Configurations = new List<string> { "merchant", "customer" },
                    RefreshUrl = "https://example.com",
                    ReturnUrl = $"https://example.com?accountId={accountId}",
                },
            },
        };

        var accountLink = service.Create(options);
        await context.Response.WriteAsJsonAsync(new { url = accountLink.Url });
    }
    catch (Exception e)
    {
        context.Response.StatusCode = 500;
        await context.Response.WriteAsJsonAsync(new { error = e.Message });
    }
});
// Fetch products for a specific account
app.MapGet("/api/products/{accountId}", async (HttpContext context) =>
{
    string accountId = context.Request.RouteValues["accountId"].ToString();

    try
    {
        var priceService = new PriceService();
        IEnumerable<Price> prices;

        var listOptions = new PriceListOptions
        {
            Active = true,
            Expand = new List<string> { "data.product" },
            Limit = 100,
        };

        RequestOptions requestOptions = null;
        if (accountId != "platform")
        {
            requestOptions = new RequestOptions
            {
                StripeAccount = accountId
            };
        }

        prices = (await priceService.ListAsync(listOptions, requestOptions)).Data;

        var products = prices.Select(price => new
        {
            id = price.ProductId,
            name = price.Product.Name,
            description = price.Product.Description,
            price = price.UnitAmount,
            priceId = price.Id,
            image = "https://i.imgur.com/6Mvijcm.png"
        });

        await context.Response.WriteAsJsonAsync(products);
    }
    catch (Exception e)
    {
        context.Response.StatusCode = 500;
        await context.Response.WriteAsJsonAsync(new { error = e.Message });
    }
});
// Create a subscription from the connected account to the platform
app.MapPost("/api/subscribe-to-platform", async (HttpContext context) =>
{
    var requestData = await ParseRequestBody(context);
    string accountId = requestData.GetProperty("accountId").GetString();
    string priceId = System.Environment.GetEnvironmentVariable("PLATFORM_PRICE_ID"); // Price ID created on the platform account

    var sessionService = new Stripe.Checkout.SessionService();
    var options = new Stripe.Checkout.SessionCreateOptions
    {
        Mode = "subscription",
        LineItems = new List<SessionLineItemOptions>
        {
            new SessionLineItemOptions
            {
                Price = priceId,
                Quantity = 1,
            },
        },
        // Pass the V2 Account ID
        CustomerAccount = accountId,
        // Defines where Stripe will redirect a customer after successful payment
        SuccessUrl = $"{System.Environment.GetEnvironmentVariable("DOMAIN")}?session_id={{CHECKOUT_SESSION_ID}}&success=true",
        // Defines where Stripe will redirect if a customer cancels payment
        CancelUrl = $"{System.Environment.GetEnvironmentVariable("DOMAIN")}?canceled=true",
    };

    var session = await sessionService.CreateAsync(options);
    await context.Response.WriteAsJsonAsync(new { url = session.Url });
});
    var options = new Stripe.Checkout.SessionCreateOptions
    {
        LineItems = new List<SessionLineItemOptions>
        {
            new SessionLineItemOptions
            {
                Price = priceId,
                Quantity = 1,
            },
        },
        Mode = mode,

        // Defines where Stripe will redirect a customer after successful payment
        SuccessUrl = $"{System.Environment.GetEnvironmentVariable("DOMAIN")}/done?session_id={{CHECKOUT_SESSION_ID}}",
        // Defines where Stripe will redirect if a customer cancels payment
        CancelUrl = $"{System.Environment.GetEnvironmentVariable("DOMAIN")}",
    };

    // Add Connect-specific parameters based on payment mode
    if (mode == "subscription")
    {
        options.SubscriptionData = new SessionSubscriptionDataOptions
        {
            ApplicationFeePercent = 5m,
        };
    }
    else
    {
        options.PaymentIntentData = new SessionPaymentIntentDataOptions
        {
            ApplicationFeeAmount = 123,
        };
    }

    var session = await sessionService.CreateAsync(options, requestOptions);
// Create a billing portal session
app.MapPost("/api/create-portal-session", async (HttpContext context) =>
{
    // Get the Stripe customer we previously created
    // Normally you'd fetch this from your database based on the authenticated user
    var requestData = await ParseRequestBody(context);
    string sessionId = requestData.GetProperty("session_id").GetString();
    var sessionService = new Stripe.Checkout.SessionService();
    var session = await sessionService.GetAsync(sessionId);
    // Create a billing portal session
    var portalService = new Stripe.BillingPortal.SessionService();
    var portalOptions = new Stripe.BillingPortal.SessionCreateOptions
    {
        // Set the customer_account to the V2 Account's ID
        CustomerAccount = session.CustomerAccount,
        ReturnUrl = $"{System.Environment.GetEnvironmentVariable("DOMAIN")}/?session_id={sessionId}",
    };
    var portalSession = await portalService.CreateAsync(portalOptions);

    // Redirect to the billing portal
    context.Response.StatusCode = 303; // See Other
    context.Response.Headers["Location"] = portalSession.Url;
});
app.MapPost("/api/webhook", async (HttpContext context) =>
{
    using var reader = new StreamReader(context.Request.Body);
    var json = await reader.ReadToEndAsync();

    // Replace this endpoint secret with your endpoint's unique secret
    // If you are testing with the CLI, find the secret by running 'stripe listen'
    // If you are using an endpoint defined with the API or dashboard, look in your webhook settings
    // at https://dashboard.stripe.com/webhooks
    var endpointSecret = "";
    Event stripeEvent;

    // Only verify the event if you have an endpoint secret defined.
    // Otherwise use the basic event from the raw JSON.
    try
    {
        if (!string.IsNullOrEmpty(endpointSecret))
        {
            var signature = context.Request.Headers["Stripe-Signature"];
            stripeEvent = EventUtility.ConstructEvent(json, signature, endpointSecret);
        }
        else
        {
            stripeEvent = EventUtility.ParseEvent(json);
        }
    }
    catch (Exception e)
    {
        Console.WriteLine($"⚠️  Webhook signature verification failed. {e.Message}");
        context.Response.StatusCode = 400;
        return;
    }

    // Handle the event
    switch (stripeEvent.Type)
    {
        case "customer.subscription.trial_will_end":
        {
            var subscription = stripeEvent.Data.Object as Stripe.Subscription;
            var status = subscription?.Status;
            Console.WriteLine($"Subscription status is {status}.");
            // Then define and call a method to handle the subscription trial ending.
            break;
        }
        case "customer.subscription.deleted":
        {
            var subscription = stripeEvent.Data.Object as Stripe.Subscription;
            var status = subscription?.Status;
            Console.WriteLine($"Subscription status is {status}.");
            // Then define and call a method to handle the subscription deleted.
            break;
        }
        case "checkout.session.completed":
        {
            var session = stripeEvent.Data.Object as Stripe.Checkout.Session;
            var status = session?.Status;
            Console.WriteLine($"Checkout Session status is {status}.");
            // Then define and call a method to handle the checkout session completed.
            break;
        }
        case "checkout.session.async_payment_failed":
        {
            var session = stripeEvent.Data.Object as Stripe.Checkout.Session;
            var status = session?.Status;
            Console.WriteLine($"Checkout Session status is {status}.");
            // Then define and call a method to handle the checkout session failed.
            break;
        }
        default:
            // Unexpected event type
            Console.WriteLine($"Unhandled event type {stripeEvent.Type}.");
            break;
    }

    // Return a 200 response to acknowledge receipt of the event
    context.Response.StatusCode = 200;
});
    "express": "^4.19.2",
    "dev": "concurrently \"vite --port 3000 --open\" \"node server.js\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"php -S [::1]:4242 server.php\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"python server.py\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"go run server.go\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"mvn compile exec:java -Dexec.mainClass=com.stripe.sample.Server\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"ruby server.rb\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"dotnet run\"",
    const response = await fetch("/api/create-connect-account", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ email }),
    });
    const response = await fetch("/api/create-account-link", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ accountId }),
    });
    const { url } = await response.json();
    window.location.href = url;
    const response = await fetch("/api/create-product", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        productName,
        productDescription,
        productPrice,
        accountId,
      }),
    });
    const response = await fetch(`/api/products/${accountId}`);
    if (!response.ok) {
      throw new Error("Failed to fetch products");
    }
    "express": "^4.19.2",
    "dev": "concurrently \"vite --port 3000 --open\" \"node server.js\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"php -S [::1]:4242 server.php\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"python server.py\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"go run server.go\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"mvn compile exec:java -Dexec.mainClass=com.stripe.sample.Server\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"ruby server.rb\"",
    "dev": "concurrently \"vite --port 3000 --open\" \"dotnet run\"",
const Product = ({ name, price, priceId, period, image }) => {
  const { accountId } = useAccount();

  return (
    <div className="product round-border">
      <div className="product-info">
        <img src={image} alt={name} />
        <div className="description">
          <h3>{name}</h3>
          <h5>{price} {period && `/ ${period}`}</h5>
        </div>
      </div>
      <form action="/api/create-checkout-session" method="POST">
        <input type="hidden" name="priceId" value={priceId} />
        <input type="hidden" name="accountId" value={accountId} />
        <button className="button" type="submit">
          Checkout
        </button>
      </form>
    </div>
  );
};
  const fetchProducts = async () => {
    if (!accountId) return;

    try {
      const response = await fetch(`/api/products/${accountId}`);
      if (!response.ok) throw new Error('Failed to fetch products');

      const products = await response.json();
      setDisplayedProducts(products);
    } catch (error) {
      console.error('Error fetching products:', error);
    }
  };
      const response = await fetch('/api/create-product', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ ...formData, accountId }),
      });
    const fetchProducts = async () => {
      try {
        const response = await fetch(`/api/products/${accountId}`);
        if (!response.ok) throw new Error('Failed to fetch products');

        const data = await response.json();
        setProducts(data);
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    };

    fetchProducts();
"use client";

import Link from "next/link";
import { useSearchParams } from "next/navigation";
import { useAccount } from "../../components/AccountProvider";

export default function Page() {
  const searchParams = useSearchParams();
  const sessionId = searchParams.get("session_id");
  const { accountId } = useAccount();

  return (
    <div className="container">
      <p className="message">Your payment was successful</p>

      <a
        href={`https://dashboard.stripe.com/${accountId}`}
        className="button"
        target="_blank"
        rel="noopener noreferrer"
      >
        Go to Connected Account dashboard
      </a>

      <Link href="/" className="button">
        Back to products
      </Link>
    </div>
  );
}
      const response = await fetch("/api/create-connect-account", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ email }),
      });
      const response = await fetch("/api/create-account-link", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ accountId }),
      });
      const data = await response.json();
      window.location.href = data.url;
export async function POST(request) {
  try {
    const { email } = await request.json();

    // Create a Connect account with the specified controller properties
    const account = await stripe.v2.core.accounts.create({
      display_name: email,
      contact_email: email,
      dashboard: "full",
      defaults: {
        responsibilities: {
          fees_collector: "stripe",
          losses_collector: "stripe",
        },
      },
      identity: {
        country: "US",
        entity_type: "company",
      },
      configuration: {
        merchant: {
          capabilities: {
            card_payments: { requested: true },
          },
        },
      },
    });

    return NextResponse.json({ accountId: account.id });
  } catch (error) {
    return NextResponse.json(
      { error: { message: error.message } },
      { status: 400 }
    );
  }
}
export async function POST(request) {
  try {
    const { accountId } = await request.json();

    const accountLink = await stripe.v2.core.accountLinks.create({
      account: accountId,
      use_case: {
        type: 'account_onboarding',
        account_onboarding: {
          configurations: ['merchant', 'customer'],
          refresh_url: 'https://example.com',
          return_url: 'https://example.com',
        },
      },
    });

    return NextResponse.json({ url: accountLink.url });
  } catch (error) {
    console.error('Error creating account link:', error);
    return NextResponse.json(
      { error: { message: error.message } },
      { status: 400 }
    );
  }
}
export async function GET(request, { params }) {
  try {
    const { accountId } = await params;

    const options = {};
    // If accountId is not 'platform', fetch from connected account
    if (accountId !== "platform") {
      options.stripeAccount = accountId;
    }
    const prices = await stripe.prices.list(
      {
        expand: ["data.product"],
        active: true,
        limit: 100,
      },
      options
    );

    return NextResponse.json(
      prices.data.map((price) => ({
        id: price.product.id,
        name: price.product.name,
        price: price.unit_amount,
        priceId: price.id,
        period: price.recurring ? price.recurring.interval : null,
        image: "https://i.imgur.com/6Mvijcm.png"
      }))
    );
  } catch (error) {
    console.error('Error fetching products:', error);
    return NextResponse.json(
      { error: { message: error.message } },
      { status: 400 }
    );
  }
}
export async function POST(request) {
  try {
    const { productName, productDescription, productPrice, accountId } = await request.json();

    // In direct charges model, create products on the connected account
    const product = await stripe.products.create({
      name: productName,
      description: productDescription,
    }, {
      stripeAccount: accountId,
    });

    const price = await stripe.prices.create({
      product: product.id,
      unit_amount: productPrice,
      currency: 'usd',
    }, {
      stripeAccount: accountId,
    });

    return NextResponse.json({
      productId: product.id,
      priceId: price.id,
    });
  } catch (error) {
    console.error('Error creating product:', error);
    return NextResponse.json(
      { error: { message: error.message } },
      { status: 400 }
    );
  }
}
  const sessionParams = {
    line_items: [
      {
        price: priceId,
        quantity: 1,
      },
    ],
    mode: mode,
    // Defines where Stripe will redirect a customer after successful payment
    success_url: `${process.env.DOMAIN}/done?session_id={CHECKOUT_SESSION_ID}`,
    // Defines where Stripe will redirect if a customer cancels payment
    cancel_url: `${process.env.DOMAIN}`,
  };

  let session;

  if (accountId) {
    session = await stripe.checkout.sessions.create(sessionParams, {
      stripeAccount: accountId,
    });
  } else {
    session = await stripe.checkout.sessions.create(sessionParams);
  }
export const stripe = new Stripe(process.env.STRIPE_SECRET_KEY)
### Install dependencies

Run `npm install` from the root of the project to install the dependencies.

### Run the application

Run `npm run dev` from the root of the project to start the application.

### Navigate to the application

Go to <http://localhost:3000/> in your web browser.

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

### Add an endpoint to create a connected account

Set up an endpoint on your server for your client to call to handle creating a connected account.

### Create a connected account

Call the Stripe API to create a connected account. We’ve configured the attributes used based on your preferences. You can prefill verification information, the business profile of the connected account, and other fields on the account if your platform has already collected it.

Use your Stripe API keys to make API requests on behalf of your connected accounts.

### Call the endpoint to create a connected account

Call the endpoint you added above to create a connected account.

### Add an Account Link endpoint

Set up an endpoint on your server to create an Account Link.

### Provide a return URL

When your connected account completes the onboarding flow, it redirects them to the return URL. That doesn’t mean that all information has been collected or that the connected account has no outstanding requirements. It only means that they entered and exited the flow properly.

### Provide a refresh URL

Stripe redirects your connected account to the refresh URL when the link is expired, the link has already been visited, your platform can’t access the connected account, or the account is rejected. Have the refresh URL create a new onboarding Account Link and redirect your connected accounts to it.

### Call the endpoint to create an Account Link

Provide the connected account ID.

### Redirect the connected account to the URL

Send the connected account to Stripe to complete onboarding using Account Link. They’re redirected back to your app when onboarding is complete.

### Connected account completes onboarding data collection

At this point the connected account will be redirected to a hosted form and will put in the necessary onboarding information for the capabilities requested.

When testing your integration, fill account information using [test data](https://docs.stripe.com/connect/testing.md).

### Handle the connected account returning

Display a useful message on the return URL.

### Handle the Account Link refreshing

Generate a new Account Link on the refresh URL.

### Add an endpoint to create a product for the connected account

Set up an endpoint for creating products and prices on behalf of your connected account. Products define what your connected accounts sell and prices track how much and how often to charge.

### Call the endpoint to create a product

Call the endpoint from the client.

### Add an endpoint to fetch the products

Set up an endpoint for fetching all products for a given connected account.

### Call the endpoint to fetch the products

In your business model, each connected account usually has its own storefront. To fetch all the products for a specific connected account, call the endpoint within your storefront.

### Create an endpoint to create a Checkout Session

The `/create-checkout-session` endpoint creates a Checkout Session and redirects the customer to a Stripe-hosted checkout page.

### Add an application fee amount

The amount that your platform plans to take from the payment.

### Indicate the connected account

The connected account that the payment will be created on.

### Create an endpoint to subscribe a connected account to the platform

Set up an endpoint that creates a subscription for the connected account to pay the platform a SaaS fee.

### Create an endpoint for the billing portal

Set up an endpoint that creates a billing portal session so connected accounts can manage their platform subscription.

### Call the endpoint to redirect the customer to checkout

The `form` makes a request to the `/api/create-checkout-session` endpoint on the server which redirects the customer to a Stripe-hosted checkout page.

### Post-checkout page

After a user completes their payment, Stripe redirects the customer to a payment success page.

### Listen for webhook events

Create a `/webhook` endpoint and obtain your webhook secret key in the [Webhooks](https://dashboard.stripe.com/webhooks) tab in Workbench to listen for events related to your payments. Use a webhook to receive these events and run actions.

### Try it out

You know the integration is working successfully if you complete a payment and see the `/done` page. Once you have the Stripe dashboard setup, the test payment will show there as well. Use any of these test cards to simulate a payment:

| Scenario | Card Number |
| --- | --- |
| Payment succeeds | 4242424242424242 |
| Payment requires 3DS authentication | 4000002500003155 |
| Payment is declined | 4000000000009995 |
