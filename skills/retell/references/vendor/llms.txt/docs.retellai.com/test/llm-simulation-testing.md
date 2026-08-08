> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Automatically test your agent

> Test a Retell agent with LLM simulation: an AI-simulated user runs your scenario in a text conversation, and success criteria grade each run.

Simulation testing lives in the <code className="ui-btn">Simulation</code> tab, where you build test cases and run them against your agent without placing a call. An AI plays the caller, and each run is graded pass or fail with a written explanation, so your saved test cases become a regression suite you rerun after every prompt or flow change, one at a time or as a [batch](/test/batch-test-simulation).

A test case is made up of:

* a **user prompt** that describes the simulated user (who they are, what they want, how they behave)
* **success criteria** that grade the run
* **dynamic variables** to preset for the run
* **custom function mocks**
* the **LLM** the simulated user runs on

This is different from the [LLM Playground](/test/llm-playground) on the side panel, where you chat with the agent by hand. Simulation testing is the repeatable, graded version.

For example, an e-commerce support team keeps a test case where the simulated caller wants to return a package and grows impatient if the conversation drags. Its criteria check that the agent processes the refund, ends the call with the `end_call` function, and keeps responses short. Any prompt change that breaks one of these behaviors fails the test before it reaches production.

<Note>
  Simulation testing works with single-prompt, multi-prompt, and Conversation Flow agents, and it runs as a text conversation. Agents using a custom LLM are not supported.
</Note>

## The Simulation tab

Open your agent and select the <code className="ui-btn">Simulation</code> tab in the top menu bar. It has two tabs:

* <code className="ui-btn">Test Cases</code> holds every test case you've built for the agent, and is where you create or import them.
* <code className="ui-btn">Batch Testing History</code> is the ledger of past runs, where you review results. See [Batch testing](/test/batch-test-simulation).

<Frame caption="The Simulation tab, with the Test Cases and Batch Testing History tabs.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/IwEyQ7Z17TwMRUlF/images/test-llm/simulation-tab-overview.mp4?fit=max&auto=format&n=IwEyQ7Z17TwMRUlF&q=85&s=56f0dee71a2adad1d53d279a270e560b" data-path="images/test-llm/simulation-tab-overview.mp4" />
</Frame>

<Tip>
  Short on time? Conductor can generate test cases for you from real calls or from scratch. See [Testing with Conductor](/test/testing-with-conductor).
</Tip>

## Create a test case

In the <code className="ui-btn">Test Cases</code> tab, click <code className="ui-btn">Test Case</code> to open the <code className="ui-btn">Add a Test Case</code> dialog, or <code className="ui-btn">Import</code> to load cases from a JSON file. Each test case captures the scenario and how to grade it.

<Steps>
  <Step title="Name the test case">
    Give it a name you'll recognize in a batch, like `return-refund-impatient-caller`.
  </Step>

  <Step title="Write the user prompt">
    Describe the person the AI should play. Include the identity details your agent asks for (name, date of birth, order number), the caller's goal, and a personality that shapes how they respond:

    ```markdown theme={"dark"}
    ## Identity

    Your name is Mike.
    Your date of birth is June 10, 1999.
    Your order number is 7891273.

    ## Goal

    Your primary objective is to return the package you received and get a refund.

    ## Personality

    You are a patient customer. However, if the conversation becomes too long or complicated, you will show signs of impatience. If the issue remains unresolved, you may become frustrated and angry.
    ```
  </Step>

  <Step title="Set success criteria, variables, and mocks">
    Fill in the [success criteria](#define-success-criteria) that grade the run, plus any [test variables and mocks](#test-variables-and-mocks) the scenario needs.
  </Step>

  <Step title="Choose the simulated user's model">
    At the bottom of the dialog, pick which LLM generates the simulated user's replies, then save the test case. This only affects the simulated user. Your agent keeps its own [model configuration](/build/llm-options), and both bill per message.
  </Step>
</Steps>

Once you have cases, select them with the checkboxes to act on several at once: <code className="ui-btn">Run Test</code> starts a batch, <code className="ui-btn">Duplicate</code> copies them, <code className="ui-btn">Export</code> downloads them as JSON you can re-import to another agent, and <code className="ui-btn">Delete</code> removes them for good.

<Frame caption="The Add a Test Case dialog.">
  <img src="https://mintcdn.com/retellai/pRGcctz_zOqy0mSt/images/sim_create.png?fit=max&auto=format&n=pRGcctz_zOqy0mSt&q=85&s=dabaebf61a1200159a96b00c93fbea05" alt="Add a test case dialog with fields for Name, User Prompt, and Success Criteria, plus a Test Variables & Mocks section with Dynamic Variables and Custom Function Mocks tabs." width="679" height="811" data-path="images/sim_create.png" />
</Frame>

## Define success criteria

Success criteria are the checks that grade a test run. When a test case finishes, all of its criteria are judged together in a single pass against the transcript: the run passes only if every criterion is met, and you get one explanation covering the whole run rather than a verdict per criterion. Because it's one combined judgment, a single unmet criterion fails the run.

A run can also end in **Error** instead of being graded, which means the simulation never got as far as a verdict. A test times out after 10 minutes, and a conversation is cut off once it passes 400 utterances, the simulated user starts repeating itself, or the agent goes silent and stops responding to the caller. In that case the explanation is the error, not a grade.

<Note>
  This is not how [AI QA](/ai-qa/overview) scores real calls. AI QA evaluates each metric separately and reports which ones passed and which failed, with a reason for each. Simulation testing gives one verdict for the run. If you need a per-criterion breakdown, phrase each check as its own test case, or use AI QA on real calls.
</Note>

Write one behavior per criterion, and be specific about what counts as success:

```markdown theme={"dark"}
1. Verify that the customer successfully returned the package and received a refund.
2. Confirm that the end_call function was called at the end of the conversation.
3. Ensure the agent's responses are conversational and contain 5 sentences or fewer.
```

Criteria can check outcomes (the refund was processed), function behavior (`end_call` was invoked), and conversation quality (response length, tone).

## Test variables and mocks

The <code className="ui-btn">Test Variables & Mocks</code> section of a test case keeps runs realistic and repeatable. It applies to both simulation (LLM) tests and [web call tests](/test/test-web).

### Dynamic variables

If your agent uses [dynamic variables](/build/dynamic-variables), set a test value for each one so placeholders like `{{customer_name}}` resolve during the simulation, the same way they would on a real call.

Use them to play a specific caller or to exercise a specific branch. An agent that opens with "Hi `{{customer_name}}`, calling about your `{{appointment_date}}` appointment" behaves differently for a returning customer than for someone with no record. Give both variables real values to test the returning-customer path, or leave `appointment_date` empty to test the "no appointment on file" branch.

Instead of retyping the same values for every test case, load them from an [environment tag](/agent/version). Every agent ships with a `prod` and a `staging` tag, you can add your own, and each tag carries its own set of dynamic variable values. Pick one from <code className="ui-btn">Load Saved Values</code> to fill the form with that tag's values, then adjust them for this test. Your edits stay on the test case and don't change the tag.

<Warning>
  A tag's dynamic variables are not test-only. They're injected into every live call and chat running on that tag, so editing `prod` from the test panel changes production behavior. Load a tag's values freely; be deliberate about editing them.
</Warning>

### Custom function mocks

A mock intercepts a [function call](/build/add-function-calling) during the test and returns a set result instead of calling the real function, so the function doesn't reach live systems and returns the same result every run.

Mocks are honored for the functions that reach outside Retell: custom functions, [code tools](/build/single-multi-prompt/code-tool), the Cal.com availability and booking tools, integration tools, call transfers, and SMS. Built-in conversation actions are always simulated and ignore any mock you set for them, including end call, press digit, extract dynamic variables, and agent transfer. **MCP tools also ignore mocks**: the dropdown lists them, but a test calls your MCP server for real.

On the <code className="ui-btn">Custom Function Mocks</code> tab, click <code className="ui-btn">+ Add</code> and pick the function from the <code className="ui-btn">Function</code> dropdown:

* For a built-in action like a transfer, choose <code className="ui-btn">Successfully</code> or <code className="ui-btn">Failed</code>.
* For a custom function, enter the mock result it should return.

For example, a `check_availability_cal` function might return:

```json theme={"dark"}
{
  "availability_for_selected_time": [
    {
      "date": "Tuesday, February 25, 2025 (Pacific Standard Time)",
      "availability_range": [
        "From 1:00 PM to 3:00 PM",
        "From 3:30 PM to 4:00 PM",
        "From 7:00 PM to 7:30 PM"
      ]
    }
  ]
}
```

Add a mock for each function you want to control. Any function you leave unmocked calls its real endpoint during the test.

<Warning>
  An unmocked custom function, code tool, calendar tool, or MCP tool calls its real production endpoint during a test, so a test can create a real booking, charge, or record. Mock every function that reaches production before you run a test, and give each mock a non-empty result: a mock left blank is skipped and the real function runs.

  Transfers and SMS are the exception. In a simulation they're always faked, whether you mock them or not, so an unmocked transfer or text can't reach a real phone.
</Warning>

<Frame caption="Mocking the Transfer to Human function to return a canned success.">
  <img src="https://mintcdn.com/retellai/pRGcctz_zOqy0mSt/images/test-llm/custom-function-mock.png?fit=max&auto=format&n=pRGcctz_zOqy0mSt&q=85&s=159972b5bc8e2cb24b7a035ba84d4dc5" alt="Custom Function Mocks tab with Transfer to Human selected in the Function dropdown, the Successfully radio chosen, a mocked response reading 'Successfully transferred the call', and a + Add button below to mock more functions." width="1078" height="990" data-path="images/test-llm/custom-function-mock.png" />
</Frame>

## Run and review test cases

To run test cases, select them in the <code className="ui-btn">Test Cases</code> tab and click <code className="ui-btn">Run Test</code>, or run a single case with the <code className="ui-btn">Test</code> button on its row. Retell launches a batch either way, even for one case, and grades each run in <code className="ui-btn">Batch Testing History</code>.

A batch runs each selected case exactly once. To sample the same scenario several times, duplicate the case or run it again. [Batch testing](/test/batch-test-simulation) covers reading the results.

## Manage test cases with the API

Test cases and batch runs are both available over the API, so you can gate a deploy on your suite from CI:

<Steps>
  <Step title="Create or update your cases">
    [Create a test case definition](/api-references/create-test-case-definition) for each scenario, or [list](/api-references/list-test-case-definitions) and [update](/api-references/update-test-case-definition) the ones you already have.
  </Step>

  <Step title="Start the batch">
    [Run a batch test](/api-references/create-batch-test) with the case IDs you want. The response is the batch ID and a `status` of `in_progress`; the runs are queued and graded asynchronously, so nothing is finished yet.
  </Step>

  <Step title="Poll until it's done">
    Call [Get batch test](/api-references/get-batch-test) until `status` is `complete`, then read `pass_count`, `fail_count`, `error_count`, and `total_count` to decide whether to ship.
  </Step>

  <Step title="Read the individual runs">
    [List test runs](/api-references/list-test-runs) for the batch to get each run's ID and result, and [Get a test run](/api-references/get-test-run) for one run's transcript and explanation.
  </Step>
</Steps>

Running a single case still means creating a batch of one; there's no separate run-one-case endpoint.

## Best practices

* **Mock functions that reach production before you run.** An unmocked custom function, code tool, calendar tool, or MCP tool calls its real endpoint, so a test can create a real booking or record.
* **Write one behavior per success criterion**, phrased as a checkable outcome.
* **Mirror real call context with dynamic variables**, and keep a separate case per branch (returning customer versus no record on file).
* **Rerun a batch before you trust a single failure**, since the simulated user and the grader are both LLMs. Judge a scenario on its pass rate across runs, not one run.
* **Grow the suite from real calls.** Turn a failed production call into a regression case with [Testing with Conductor](/test/testing-with-conductor) or [AI QA](/ai-qa/overview).
* **Cover edge cases, not just happy paths**: an angry caller, a refusal to verify identity, a mid-call change of mind.
* **Rerun the suite after every prompt or flow change** before you deploy.

## Glossary

| Term                      | What it means                                                                                            |
| ------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Test case**             | A saved scenario (user prompt, success criteria, variables, and mocks) you can rerun against your agent. |
| **Simulated user**        | The LLM that plays the caller, following your user prompt.                                               |
| **User prompt (persona)** | The description of who the simulated user is, their goal, and how they behave.                           |
| **Success criteria**      | The checks that grade a run. They're judged together for one pass or fail verdict and one explanation.   |
| **Function mock**         | A canned response that intercepts a function call so the real function doesn't run.                      |
| **Dynamic variables**     | Placeholders like `{{customer_name}}` you set to test values so they resolve during a run.               |
| **Batch test**            | Running many test cases together; results land in Batch Testing History.                                 |
| **Pass rate**             | The share of runs that met all success criteria across a batch.                                          |

## Next steps

* [Batch test your agent](/test/batch-test-simulation) to run many test cases at once and track the pass rate across releases.
* [Testing with Conductor](/test/testing-with-conductor) to generate and run test cases automatically.
* [Testing overview](/test/test-overview) compares simulation testing with the LLM Playground and live web or phone call testing.
