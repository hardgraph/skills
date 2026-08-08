> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Phone call testing

> Test a Retell voice agent on a real phone call: attach a number, place an outbound test call or dial in, and validate audio, latency, DTMF, and transfers.

A phone call test runs your agent over real telephony, so you can validate what a [web call](/test/test-web) can't: carrier audio quality, network latency, DTMF menu navigation, and live call transfers. Run one as a final check before you take the agent live.

<Note>
  Phone testing needs a [phone number](/deploy/purchase-number) attached to your agent. The test call runs whichever [agent version](/agent/version) the number is bound to: a specific version number, a tag, <code className="ui-btn">Latest Published</code>, or <code className="ui-btn">Latest Created</code>. Because <code className="ui-btn">Latest Created</code> resolves to your newest version, published or not, you can dial a draft agent without publishing it first.
</Note>

## Place an outbound test call

Have Retell call your phone so you can talk to the agent.

<Note>
  Newer workspaces have to pass identity verification before Retell will place an outbound call, including a test call. If yours hasn't, the number's page shows a verification prompt in place of the call button. See [identity verification](/accounts/kyc).
</Note>

<Steps>
  <Step title="Attach an outbound agent to a number">
    On the <code className="ui-btn">Phone Numbers</code> page, open a number you own and set your agent as its <code className="ui-btn">Outbound agent</code>. Pick the version you want to test here: a numeric version, a tag, <code className="ui-btn">Latest Published</code>, or <code className="ui-btn">Latest Created</code> for your current draft.
  </Step>

  <Step title="Make the call">
    On the number's detail page, click <code className="ui-btn">Make an outbound call</code>, enter your phone number, and place the call. Set any [dynamic variables](/build/dynamic-variables) in the same dialog so they render on the test call.
  </Step>
</Steps>

## Test with an inbound call

<Steps>
  <Step title="Attach an inbound agent to a number">
    On the <code className="ui-btn">Phone Numbers</code> page, open a number you own and set your agent as its <code className="ui-btn">Inbound agent</code>.
  </Step>

  <Step title="Dial the number">
    Call that number from your own phone. The call connects to the attached agent.
  </Step>
</Steps>

## What to check on a phone call

A phone call is where telephony-specific behavior shows up:

* **DTMF and press-digit**: whether the agent navigates or responds to keypad input.
* **Call transfers**: whether a warm or cold transfer connects. Transfers to a phone number can't be tested over a web call.
* **[Voicemail](/build/handle-voicemail) and IVR detection**: whether the agent recognizes a machine and takes the action you configured. Detection only runs on phone calls, so this is the one behavior a web call can't approximate at all.
* **SMS**: whether a send-SMS step delivers, which also needs a real number.
* **Carrier audio and latency**: how the agent sounds and how it handles delay over the network.

For example, if your agent transfers billing questions to a human and reads out a reference number for the caller to enter, place a phone test call, trigger the transfer, and confirm the handoff connects and the digits register.

<Tip>
  After a test call, ask [Conductor](/conductor/test-and-improve) to review it and suggest fixes.
</Tip>

## Next steps

* [Web call testing](/test/test-web) for a quick browser check without a phone number.
* [Simulation testing](/test/llm-simulation-testing) for automated pass or fail across many scenarios.
* [Testing overview](/test/test-overview) compares all the testing methods.
