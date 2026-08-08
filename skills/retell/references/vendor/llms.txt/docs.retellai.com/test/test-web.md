> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Web call testing

> Test a Retell voice agent with a browser web call from the dashboard: use the Test Audio panel to hear real audio, latency, and interruptions.

A web call test lets you talk to your agent in the browser, so you hear how it sounds and handles a live voice conversation before you put it on the phone. Where [simulation testing](/test/llm-simulation-testing) runs on text, a web call exercises real audio: latency, turn-taking, and interruptions. It needs no phone number, and you can test a draft agent without publishing it first.

## Start a web call test

<Steps>
  <Step title="Open Test Audio">
    In the agent editor, open the <code className="ui-btn">Test</code> panel on the right and select <code className="ui-btn">Test Audio</code>.
  </Step>

  <Step title="Set test inputs (optional)">
    If your agent uses [dynamic variables](/build/dynamic-variables) or functions you'd rather not fire for real, open the <code className="ui-btn">Test Inputs</code> dropdown next to the tabs, fill in the <code className="ui-btn">Dynamic Variables</code> and <code className="ui-btn">Custom Function Mocks</code> tabs, and click <code className="ui-btn">Save</code>. The same values apply to [Test LLM](/test/llm-playground) runs.
  </Step>

  <Step title="Pick a starting point (optional)">
    For a multi-prompt or Conversation Flow agent, choose a starting state or node to begin partway through the conversation instead of at the beginning. This is the quickest way to hear one branch without talking your way to it.
  </Step>

  <Step title="Run the test">
    Click <code className="ui-btn">Run Test</code> and allow microphone access, then talk to your agent. Click <code className="ui-btn">End the Call</code> to hang up.
  </Step>
</Steps>

<Frame caption="Run a test web call from the agent detail page.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/IwEyQ7Z17TwMRUlF/images/test-llm/web-call-test.mp4?fit=max&auto=format&n=IwEyQ7Z17TwMRUlF&q=85&s=d5e65e3b7aef8ae5ef11cc2c76c5e2c7" data-path="images/test-llm/web-call-test.mp4" />
</Frame>

## What a web call test is good for

Use it for the checks a text simulation can't make:

* **Latency and turn-taking**: how quickly the agent responds and whether it waits for you to finish.
* **Interruptions**: whether the agent stops and listens when you talk over it.
* **How the voice actually sounds**: pacing, pronunciation, and filler.

For example, after reworking your greeting and opening question, run a web call to hear the turn-taking, interrupt the agent mid-sentence, and confirm it yields before you ship the change.

## Limitations

Some telephony behavior can't happen in a browser call:

* **Transfers to a phone number fail**, whether cold, warm, or agentic warm. The dashboard warns you when your agent has a transfer, and the call returns a transfer failure if the agent tries. [Transfer to another agent](/build/single-multi-prompt/transfer-agent) does work, and the test panel follows the swap.
* **Sending an SMS fails**, since there's no phone number on the call.
* **Voicemail and IVR detection don't run**, so you can't check a voicemail drop or a phone-tree branch.

Use [phone call testing](/test/test-phone) for any of those, or for real carrier audio. For automated pass or fail across many scenarios at once, use [simulation testing](/test/llm-simulation-testing) and [batch testing](/test/batch-test-simulation).

A web call test is a real call for billing and limits: it bills per minute, takes one of your [concurrent call](/deploy/concurrency) slots, and won't start if your account has a payment problem.

<Tip>
  After a test call, ask [Conductor](/conductor/test-and-improve) to review it and suggest fixes.
</Tip>

## Next steps

* [Make a web call](/deploy/web-call) adds browser-based calls like this to your own app.
* [Phone call testing](/test/test-phone) validates real telephony, DTMF, and transfers.
* [Testing overview](/test/test-overview) compares web calls with simulation and the LLM Playground.
