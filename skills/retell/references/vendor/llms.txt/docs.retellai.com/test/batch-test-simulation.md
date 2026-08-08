> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Batch test your agent

> Run Retell batch tests on many simulation cases at once, then review pass rate and per-case results in Batch Testing History to catch regressions.

A batch test run is a set of [test cases](/test/llm-simulation-testing) run together in a simulation, so you can check many scenarios in one pass instead of one at a time. Batches are how you use your saved test cases as a regression suite: run them after every prompt or flow change, and confirm the pass rate holds before you deploy.

For example, a support team keeps two dozen test cases covering refunds, escalations, and identity checks. Before each release they run the whole set as a batch, then compare the pass rate to the previous run to catch anything a prompt change broke.

## Run a batch

In the <code className="ui-btn">Test Cases</code> tab, select the cases you want to run (one, several, or all) and click <code className="ui-btn">Run Test</code>. Retell launches a batch, even for a single case, and sends the results to <code className="ui-btn">Batch Testing History</code>.

<Frame caption="Selecting test cases and running them as a batch.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/IwEyQ7Z17TwMRUlF/images/test-llm/simulation-tab-batch-run.mp4?fit=max&auto=format&n=IwEyQ7Z17TwMRUlF&q=85&s=1c51cd01a196b835abf8f3e5dbe88626" data-path="images/test-llm/simulation-tab-batch-run.mp4" />
</Frame>

## Track a running batch

You track and review batches under the <code className="ui-btn">Batch Testing History</code> tab. A batch that's still running is marked **Ongoing** in the list on the left, and a **Running tests** indicator sits below its results table until the last case finishes.

Results stream in on a 10-second refresh rather than all at once: each case joins the results table as it completes, and the counts above the table go up with it. Cases that haven't finished yet stay hidden, so the table only ever shows completed runs.

## Review results

Runs land in <code className="ui-btn">Batch Testing History</code>. The ledger on the left lists every run, each showing:

* the number of test cases in the run
* the date and time it finished
* the pass rate (the share of cases that met all their success criteria)

The list only ever shows batches run against one [agent version](/agent/version) at a time, and the version chip above it tells you which. It defaults to your latest version, so older batches won't be listed until you switch. To see another version's history, select that version in the agent's version panel; to go back to the latest, clear the chip with its <code className="ui-btn">X</code>.

Select a run to open its results table, and filter it by <code className="ui-btn">All</code>, <code className="ui-btn">Passed</code>, or <code className="ui-btn">Failed</code>. An **Error** count appears alongside them when a run failed to complete instead of being graded, such as a simulation that timed out. Its columns are:

| Column       | What it shows                                                                            |
| ------------ | ---------------------------------------------------------------------------------------- |
| Test Case    | The name of the case that ran.                                                           |
| Test Call ID | The ID of the simulated run, for reference or API lookup.                                |
| Time         | The time the simulation started.                                                         |
| Test Result  | The grader's written explanation of the result, or the error message if the run errored. |
| Test Success | Whether the run met its success criteria.                                                |

Select a test case row to open its details panel on the right, which shows:

* the test case ID and test result
* one explanation of why the run passed or failed across all of its success criteria
* the full run transcript: node transitions, tool calls and their responses, and the agent and user turns

A <code className="ui-btn">View In Test Playground</code> button opens the agent in a new tab with that run's transcript loaded into the [Playground](/test/llm-playground#test-playground), where you can edit any turn and continue the conversation by hand.

## Manage batches with the API

Run and read batches programmatically to fold them into CI before you deploy:

* [Run a batch test](/api-references/create-batch-test) starts the batch and returns its ID.
* [Get batch test](/api-references/get-batch-test) returns the batch's status and its pass, fail, and error counts. Poll it until the status is `complete`.
* [List test runs](/api-references/list-test-runs) lists each run in the batch, and [Get a test run](/api-references/get-test-run) returns one run's transcript and explanation.
* [List batch tests](/api-references/list-batch-tests) lists past batches for an agent version.

The [simulation testing](/test/llm-simulation-testing#manage-test-cases-with-the-api) page walks through this as a full CI sequence.

## Next steps

* [Simulation testing](/test/llm-simulation-testing) covers building the test cases a batch runs.
* [Testing with Conductor](/test/testing-with-conductor) can generate those test cases for you.
* [Testing overview](/test/test-overview) compares simulation and batch testing with live web and phone call testing.
