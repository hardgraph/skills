> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Testing overview

> Compare Retell's agent testing methods — LLM Playground, simulation, batch, web call, and phone call testing — and pick the right one for each stage.

Retell gives you several ways to test an agent before it takes real calls, from a quick text chat to a full phone call. Use the lighter, cheaper methods while you build, and the real-audio methods to validate before you go live.

## Testing methods

### LLM Playground

Chat with your agent in text, by hand or with an AI-simulated user, and inspect tool calls and transitions turn by turn. Best for building and debugging. It's the <code className="ui-btn">Test LLM</code> tab in the agent's test panel. See [Manually test your agent](/test/llm-playground) and [Debug your agent response](/test/llm-playground-debug).

### Simulation testing

Save scenarios as graded test cases and run them, one at a time or as a [batch](/test/batch-test-simulation), to catch regressions automatically. [Conductor](/test/testing-with-conductor) can write the test cases, run the batch, and report the failures. See [Automatically test your agent](/test/llm-simulation-testing).

### Web call testing

Talk to your agent in the browser to hear real audio, latency, and interruptions, without a phone number. See [Web call testing](/test/test-web).

### Phone call testing

Place or receive a real phone call to validate telephony: carrier audio, DTMF, and transfers. See [Phone call testing](/test/test-phone).

## Which method fits each scenario

Match the method to what you're trying to do. Several methods can fit the same scenario, so pick the lightest one that covers it.

| I want to...                                             | LLM Playground | Simulation testing | Web call | Phone call |
| -------------------------------------------------------- | :------------: | :----------------: | :------: | :--------: |
| Iterate on a prompt or flow change quickly               |        ✅       |          ✅         |          |            |
| Inspect tool calls and node transitions turn by turn     |        ✅       |          ✅         |          |            |
| Replay or edit a single turn to reproduce a bug          |        ✅       |                    |          |            |
| Role-play a specific caller or edge case with an AI user |        ✅       |          ✅         |          |            |
| Grade a run pass or fail against success criteria        |                |          ✅         |          |            |
| Catch regressions across a suite, including from CI      |                |          ✅         |          |            |
| Hear real voice, latency, and interruptions              |                |                    |     ✅    |      ✅     |
| Check transfers, DTMF, and carrier audio                 |                |                    |          |      ✅     |
| Test without a phone number                              |        ✅       |          ✅         |     ✅    |            |

Only simulation testing grades a run. The Playground's AI Simulated Chat drives a conversation the same way but doesn't score it, which is why saving a run as a test case is what turns it into a check.

For exact rates, see [testing pricing](/test/testing-pricing).

## What your agent supports

* **Custom LLM agents** can only be tested with a web or phone call. The Test LLM tab is hidden for them, and simulation and batch testing reject them.
* **Speech-to-speech agents** are also audio-only, since there's no text turn to simulate.
* **Chat agents** are the reverse: they have no Test Audio tab, so you test them with the Playground and simulation testing.

## A workflow that works

<Steps>
  <Step title="Build and debug">
    Iterate in the [LLM Playground](/test/llm-playground): chat by hand, inspect tool calls, and [debug](/test/llm-playground-debug) specific turns.
  </Step>

  <Step title="Lock in a regression suite">
    Turn your key scenarios into [simulation test cases](/test/llm-simulation-testing) and run them as a [batch](/test/batch-test-simulation) after every change. Let [Conductor](/test/testing-with-conductor) draft cases from real calls.
  </Step>

  <Step title="Validate the voice">
    Run a [web call](/test/test-web) to hear how the agent sounds and handles interruptions.
  </Step>

  <Step title="Confirm telephony">
    Place a [phone call](/test/test-phone) to check transfers, DTMF, and carrier audio before you go live.
  </Step>
</Steps>

To score real production calls after they happen (for hallucinations, resolution rate, latency, and more), use [AI Quality Assurance](/ai-qa/overview) alongside your test suites.

<Tip>
  Keep a checklist of critical paths, both happy paths and edge cases, and run it before every deployment.
</Tip>
