> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Extract Dynamic Variables

> Add the Extract Dynamic Variable tool to a Retell single- or multi-prompt agent to capture values from a call as typed dynamic variables.

The Extract Dynamic Variable tool captures values from the conversation while a
call is in progress and stores them as [dynamic variables](/build/dynamic-variables)
your agent can reuse in later turns. The agent decides when to call it from the
tool's name, the variables you define, and your agent prompt.

For example, a booking agent can extract the caller's name and email as soon as
they give them, then reference `{{user_name}}` in its confirmation and pass
`{{user_email}}` to a later custom function that creates the appointment.

If you use a conversation flow agent, see [Extract Dynamic Variable node](/build/conversation-flow/extract-dv-node).

## When to use it

Use this tool when the agent needs a value again *during the same call* — to
personalize a later response, branch on it, or send it to a function or transfer.

| Need                                                                 | Use                                                                                           |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Reuse a captured value later in the same call                        | **Extract Dynamic Variable tool**                                                             |
| Collect structured data only for after the call (analytics, webhook) | [Post-call analysis](/features/post-call-analysis-overview)                                   |
| Run the extraction on a fixed step in a set sequence                 | [Extract Dynamic Variable node](/build/conversation-flow/extract-dv-node) (conversation flow) |
| Compute or transform values in code                                  | [Code Tool](/build/single-multi-prompt/code-tool)                                             |

Because the agent chooses when to call the tool, extraction isn't guaranteed to
run at an exact point. When a value must be captured in a strict order, use a
[conversation flow](/build/conversation-flow/extract-dv-node) instead.

## Create the tool

<Steps>
  <Step title="Add an Extract Dynamic Variable tool">
    In the agent's **Functions** section, select **+ Add**, then select
    **Extract Dynamic Variable**.

    <Frame>
      <img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/extract-dv/dropdown.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=49f0432ef7c2341ae1d8f3c6db45cbaa" alt="Function-type menu with Extract Dynamic Variable listed above Custom Function." width="333" height="326" data-path="images/extract-dv/dropdown.png" />
    </Frame>
  </Step>

  <Step title="Name the tool">
    Give the tool a **Function Name** with 1–64 letters, numbers, underscores,
    or dashes. Spaces aren't allowed, and the name must be unique among the
    agent's tools.

    Add a short **Description** summarizing what the tool captures. What actually
    steers extraction is the per-variable descriptions (next step) and your agent
    prompt, so keep this field brief.

    * **Function Name:** `extract_user_details`
    * **Description:** `Captures the caller's name and email.`

    <Frame>
      <img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/extract-dv/modal.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=302f541fdcbf9953dec485f91b0a6424" alt="Extract Dynamic Variable dialog with Function Name, Description, and a Variables list." width="687" height="453" data-path="images/extract-dv/modal.png" />
    </Frame>
  </Step>

  <Step title="Add variables">
    Select **+ Add** under **Variables** and fill in the fields for each value
    you want to capture:

    * **Variable Name** – the name you'll reference as `{{variable_name}}`. Keep
      it short and unique within the tool.
    * **Variable Description** – what the value is. The agent uses this to decide
      what to pull from the conversation, so a clear description is the main
      driver of extraction accuracy.
    * **Variable Type** – `Text`, `Number`, `Boolean`, or `Enum`. Optional;
      defaults to `Text`.
    * **Enum Options** – the allowed values, added one per row. Shown only when
      the type is `Enum`. At least one option is required.

    <Frame>
      <img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/extract-dv/add-variable.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=1186a4aafcbfadd4702dd8579dcddb5b" alt="Variable editor with Variable Name, Variable Description, and a Variable Type dropdown set to Text." width="779" height="498" data-path="images/extract-dv/add-variable.png" />
    </Frame>

    Select **Save**, then add more variables as needed.
  </Step>

  <Step title="Save the tool">
    Select **Save** to add the tool to the agent. Each variable appears in the
    tool's **Variables** list.

    <Frame>
      <img src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/extract-dv/variable-added.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=bb02abc71dcc49c80ecde11c93dc42df" alt="Extract Dynamic Variable dialog showing the saved user_name variable in the Variables list." width="720" height="521" data-path="images/extract-dv/variable-added.png" />
    </Frame>
  </Step>

  <Step title="Tell the agent when to extract">
    Add an instruction to the agent prompt so the agent calls the tool at the
    right moment.

    ```text theme={"dark"}
    When the caller states their name and email address, call
    extract_user_details to save the information before continuing.
    ```
  </Step>
</Steps>

## Variable types

Pick the type that matches the value. The type guides how the agent formats what
it extracts.

| Type        | Stores                          | Examples                     |
| ----------- | ------------------------------- | ---------------------------- |
| **Text**    | Any word or phrase              | `"headache"`, `"John Smith"` |
| **Number**  | A numeric value                 | `42`, `98.6`                 |
| **Boolean** | True or false                   | `true`, `false`              |
| **Enum**    | One value from **Enum Options** | `"yes"`, `"no"`, `"maybe"`   |

All extracted values are stored as strings, matching how dynamic variables work
everywhere else in Retell. A `Number` is stored as `"42"` and a `Boolean` as
`"true"`. In a [Code Tool](/build/single-multi-prompt/code-tool), convert them
before use, for example `Number(dv.order_count)`.

## Use the extracted values

Once extracted, a value behaves like any other [dynamic variable](/build/dynamic-variables):

* Reference it in prompts and tool fields with `{{variable_name}}` on later
  turns.
* Read it in a [Code Tool](/build/single-multi-prompt/code-tool) as `dv.variable_name`.
* Pass it to a [custom function](/build/single-multi-prompt/custom-function) or
  an [agent transfer](/build/single-multi-prompt/transfer-agent).

If the caller never provides a value, the tool doesn't set that variable, and
`{{variable_name}}` stays in its raw form. See
[handling missing variables](/build/dynamic-variables#handling-missing-variables)
for how to write prompts that tolerate an unset value.

## Play typing sound

Turn on **Play typing sound** in the tool to play a subtle typing sound on the
agent's audio track while it extracts. Extraction is usually fast, so most
agents leave this off.

## FAQ

<AccordionGroup>
  <Accordion title="Can I extract several values with one tool?">
    Yes. Add as many variables as you need under one tool, and the agent fills
    the ones the conversation supports in a single call. Group related values (name
    and email, for example) so they're captured together.
  </Accordion>

  <Accordion title="What happens if the caller doesn't give a value?">
    The tool only sets variables it can extract. A value the caller never
    provides isn't stored, so `{{variable_name}}` stays literal until it's set.
    Design prompts to handle an unset variable, or set a default at the agent
    level.
  </Accordion>

  <Accordion title="Can I overwrite a variable later in the call?">
    Yes. If the tool extracts the same variable again, the new value replaces the
    old one for the rest of the call.
  </Accordion>

  <Accordion title="Is this the same as post-call analysis variables?">
    No. This tool captures values *during* the call so the agent can use them in
    later turns. [Post-call analysis](/features/post-call-analysis-overview) extracts data
    after the call ends for reporting and webhooks, and those values aren't
    available mid-call.
  </Accordion>

  <Accordion title="The agent keeps re-calling the extract tool. How do I stop it?">
    Spell out in your agent prompt when to call the tool (for example, "call
    extract\_user\_details once, right after the caller gives both their name and
    email"). Since the agent prompt and variable descriptions drive the timing,
    vague instructions lead the agent to call the tool repeatedly.
  </Accordion>
</AccordionGroup>
