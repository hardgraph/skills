> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Testing with Conductor

> Use Conductor, Retell's AI copilot, to generate simulation test cases from real calls, cold-start suites for new agents, and target tool calls.

[Conductor](/conductor/overview) is Retell's AI copilot in the dashboard. It can write [simulation test cases](/test/llm-simulation-testing) for you, so you build a regression suite from real calls and plain-language prompts instead of authoring every case from a blank slate.

Open the Conductor panel on the agent you want to test, then ask. It drafts the cases into that agent's <code className="ui-btn">Test Cases</code> tab, where you can review, edit, and run them like any other test case.

Conductor can also work the suite, not just write it. Ask it to run a batch and it starts one, waits for the results, and reports the pass rate and each failure back in the panel. It can read a run's full transcript, update a case, and delete cases you no longer want.

<Note>
  Conductor authors test cases for single-prompt, multi-prompt, and Conversation Flow agents.
</Note>

<Frame caption="Generating simulation test cases with Conductor.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/NF2VHII1fTHMM9h-/images/conductor/conductor-simulation-testing.mp4?fit=max&auto=format&n=NF2VHII1fTHMM9h-&q=85&s=339ab666cd8cabf380fcf7667a0ac52d" data-path="images/conductor/conductor-simulation-testing.mp4" />
</Frame>

## Generate test cases from real calls

Point Conductor at calls that already happened and have it turn them into test cases. This is the fastest way to build a regression suite: every real failure becomes a case that guards against the same problem returning.

To point Conductor at a specific call, give it the call's **call ID** from your [Call History](/features/session-history) or [AI QA](/ai-qa/overview), or describe the calls you mean:

```text theme={"dark"}
Turn call_7a3d9f2b1c8e4056a9d1f3b6c2e8074a into a test case.

Make test cases from my last 10 calls where the caller hung up before booking.
```

Conductor reads the transcript and tool history from those calls and drafts matching test cases with a persona and success criteria. It can also set the dynamic variables and function mocks, so you don't have to add them by hand.

## Start a suite from scratch

On a brand-new agent with no call history, ask Conductor for a starter suite so you have coverage before the first real call:

```text theme={"dark"}
Write 5 test cases covering the happy path and common edge cases for booking an appointment.
```

Conductor drafts the cases scoped to the current agent. Review them, then add any [dynamic variables or mocks](/test/llm-simulation-testing#test-variables-and-mocks) the scenarios need.

## Target tool calls and functions

To test how your agent handles a specific tool result, ask Conductor to write a case that mocks the function and checks the agent's response:

```text theme={"dark"}
Add a test case where check_availability returns no open slots, and verify the agent offers a callback instead of ending the call.
```

Conductor writes the case with the [function mock](/test/llm-simulation-testing#custom-function-mocks) and a success criterion, so you can confirm the behavior without calling the real function.

<Warning>
  A tool call that matches no mock falls through to the real tool. Conductor can read your agent's tool definitions, but it won't necessarily mock every one it should, so review each generated case and add a mock for any function that reaches a live endpoint before you run it.
</Warning>

## Next steps

* [Simulation testing](/test/llm-simulation-testing) covers editing and running the cases Conductor drafts.
* [Batch testing](/test/batch-test-simulation) runs the whole suite at once and reports the pass rate.
* [Test and improve with Conductor](/conductor/test-and-improve) covers reviewing real calls and running simulations from the Conductor panel.
