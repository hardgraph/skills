> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Flex Mode: dynamic conversation flow execution

> Flex Mode compiles a Retell conversation flow into a single structured prompt at runtime so the agent can handle varied user behavior while keeping flow logic.

Flex Mode combines the best of both worlds:

* Conversation Flow: clear, visual business logic that’s easy to manage.
* Single Prompt Agent: flexible, natural handling of varied user behavior.

You design your conversation flow as usual (nodes, edges, tools). At
runtime, Flex Mode compiles that flow into one structured prompt made of Tasks
and available Tools. The agent then navigates Tasks dynamically while still
following your global prompt.

## Cost Impact

<Warning>
  Flex mode can significantly increase your LLM costs. Because all node instructions,
  transitions, and tool descriptions are compiled into a single prompt, the total token
  count is much higher than in rigid mode (where only the active node's prompt is sent
  to the LLM). When the combined prompt exceeds 4,000 tokens, the
  [token scaling billing rule](/accounts/billing-exceptions#rule-2-llm-price-scaling-for--4000-token-prompt-length)
  applies, which can multiply your costs several times over.
</Warning>

To control costs, consider using **rigid mode** or **breaking your flow into smaller
subflows** so that fewer nodes are compiled into a single prompt. If you do use flex
mode, keep node instructions concise to minimize token usage.

## When To Use

* You want the clarity of a flowchart (business steps) but need the freedom of a
  single prompt:
  * You can easily switch context from different tasks e.g. every node would
    become a global node
  * The agent could move on to the proper task if the user completed multiple tasks at the
    same time.
  * After switching the context to another flow, the agent could resume the previous
    task without repeating the already completed steps.

## How It Works

You can enable the 'Flex Mode' either at the subflow level or the agent level.

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/I1roxGSvtapAS92i/images/cf/flex-mode.png?fit=max&auto=format&n=I1roxGSvtapAS92i&q=85&s=4531e34b2189e6ef7c36465795c78136" data-path="images/cf/flex-mode.png" />
</Frame>

When enabled at agent level, all the nodes get converted to a single flex
node. It will stay on the flex node and behave like a single prompt agent until
it reaches the 'End Call'.

When enabled on a subflow, only that subflow’s nodes are converted into a
single prompt; the rest stays as standard conversation flow.

## Tool Call / Function

There are some differences in how flex mode (single prompt) and traditional
conversation flow handle the tool call/function.

* **Speak During Execution** The execution message part will still work the same.
* **Speak After Execution** There is no 'Speak After Execution' setting in Flex
  Mode. The agent will always speak after function execution.
* **Wait For Result** There is no 'waitForResult' setting in Flex Mode. Agent
  will always wait for the function to complete (similar to Single Prompt
  agent).

## Knowledge Base

Node-level knowledge base will be ignored in Flex Mode. You will need to configure the knowledge base at agent level.

## Best Practices & Known Issues

* Write the node instruction in a concise manner so that LLM could better focus
  on the task.
* Only use Prompt edge, avoid using Equation edge as LLM is really bad at
  interpreting equation conditions. You might see very weird behaviors.
* Be explicit on transitions: write crisp, observable conditions.
* If you use flex mode for more than 20 nodes, performance might degrade and
  agent might have higher hallucination risk. We recommend splitting into
  smaller subflows.
* LLM might not always follow the static text instruction.
