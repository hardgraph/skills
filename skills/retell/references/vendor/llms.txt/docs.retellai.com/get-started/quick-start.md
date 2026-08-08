> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build your first phone agent in 15 minutes

> Build your first Retell AI phone agent in 15 minutes: create an account, pick a template, test in the dashboard, deploy to a phone number, and make a call.

**Retell** is a platform for building, testing, and deploying **AI phone agents** that hold natural conversations over the phone.

<Steps>
  <Step title="Create your account">
    1. Visit the [Retell Dashboard](https://dashboard.retellai.com)
    2. Sign up for a new account

    New accounts start with \$10 in free trial credits, no payment method required. You'll only need to add one if you want to purchase a phone number.
  </Step>

  <Step title="Create a new agent">
    1. Navigate to the "Agents" tab
    2. Click "Create an agent"
    3. Pick the agent type: **Single prompt** (easy to start, simple free-form conversations) or **Conversational flow** (production-ready, deterministic conversations)
    4. Choose how to start: click **Build from scratch** to build everything yourself, pick a ready-made template like Receptionist or Insurance Verification Caller, or click **Generate from prompt** (marked Suggested) to let [Conductor](/conductor/create-agent) draft an agent from a plain-English description of your use case

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/MoqZ2YN1eciIk62m/images/create-agent-templates.png?fit=max&auto=format&n=MoqZ2YN1eciIk62m&q=85&s=2502d0beee48e4743dbfb0477e4ccb2d" alt="Create agent dialog: pick a Single prompt or Conversational flow type, then choose Build from scratch, the suggested Generate from prompt option, or a ready-made template like Insurance Verification Caller." data-path="images/create-agent-templates.png" />
    </Frame>
  </Step>

  <Step title="Test your agent">
    1. Click the "Test" button to start a web call with your agent
    2. Allow microphone access when your browser prompts for it, then talk to your agent like a real caller

    This step is free and doesn't need a phone number or payment method. You can test a draft agent this way as many times as you want before publishing it.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/2bc_YV-DZysjbtgO/images/test-agent-playground.png?fit=max&auto=format&n=2bc_YV-DZysjbtgO&q=85&s=e6727898af839baafefadcb0775b2108" alt="Single Prompt Agent editor with the prompt on the left and Functions in the middle; an arrow points to the Test Audio panel on the right with its Run Test button." data-path="images/test-agent-playground.png" />
    </Frame>
  </Step>

  <Step title="Deploy to a phone number">
    1. Go to the "Phone Numbers" tab
    2. Click "Buy New Number"
    3. If you don't have a payment method on file yet, add one: click "Add Payment Method" right on the "New Number" page, or go to the "Billing" tab and click "Manage Payment Methods" to add one through Stripe
    4. (Optional) Enter the area code you want to buy the number for
    5. Purchase your number
    6. Assign your agent to the number in the configuration settings

    Retell-managed numbers are US and Canada only: \$2 per month for normal phone numbers, \$5 per month for toll-free phone numbers. If you need a number from another country or want to bring your own telephony provider, see [custom telephony](/deploy/custom-telephony) instead.

    <Frame>
      <img height="200" src="https://mintcdn.com/retellai/eDm7SYLrvELb5Shw/images/phone-number-config.png?fit=max&auto=format&n=eDm7SYLrvELb5Shw&q=85&s=0e73ad5463dea8e2c1d0ecab973f9d75" alt="Phone number configuration page with arrows pointing to the Inbound Call Agent and Outbound Call Agent selectors, where you assign agents to handle calls." data-path="images/phone-number-config.png" />
    </Frame>
  </Step>

  <Step title="Test your phone agent">
    1. Incoming Calls:
       * Dial your purchased number

    2. Outbound Calls:
       * Click "Make an outbound call"
       * Enter the phone number including the country code (e.g., `+12137774445`)
  </Step>
</Steps>

Congratulations! Your agent is now live. It can:

* Receive incoming calls
* Make outbound calls
* Handle natural conversations 24/7

## FAQ

<AccordionGroup>
  <Accordion title="My browser won't let the test call access the microphone. What do I do?">
    Check your browser's site permissions and allow microphone access for the dashboard, then click "Test" again. If you dismissed the permission prompt, most browsers require you to re-enable it from the address bar's site settings rather than retriggering the prompt automatically.
  </Accordion>

  <Accordion title="Do I need a phone number to try Retell?">
    No. Web call testing (step 3) works on a draft, unpublished agent with no payment method or phone number required. You only need a number once you want to make or receive real phone calls.
  </Accordion>

  <Accordion title="Can I get a number outside the US or Canada?">
    Not as a Retell-managed number. Connect your own provider instead. See [custom telephony](/deploy/custom-telephony).
  </Accordion>

  <Accordion title="My payment method was declined. What now?">
    Double-check the card details and try again, or add a different payment method from the "Billing" tab. See [add payment methods](/accounts/add-payment) for more detail.
  </Accordion>
</AccordionGroup>

## Next steps

Now that your agent works, keep going:

<CardGroup cols={2}>
  <Card title="Customize Your Agent" icon="sparkles" href="/build/overview">
    See everything you can configure, prompts, voice, knowledge, and more
  </Card>

  <Card title="Add Functions" icon="code" href="/build/single-multi-prompt/function-calling">
    Integrate APIs and external services into your agent
  </Card>

  <Card title="Monitor Performance" icon="chart-line" href="/features/analytics-dashboard">
    Track call metrics and analyze agent performance
  </Card>

  <Card title="Set Up Post-Call Analysis" icon="clipboard-check" href="/features/post-call-analysis-create">
    Automatically extract structured data like call outcome and sentiment from every call
  </Card>
</CardGroup>

### Follow us on YouTube

<Card title="Retell YouTube" icon="monitor-play" href="https://www.youtube.com/watch?v=w8W4AuheIms&list=PLGrX1_bbFSHrtX8PLjGFkijEIkcNrsTz8">
  Subscribe for tutorials, feature walkthroughs, and other educational content. See more in the [video hub](/videos/introduction).
</Card>
