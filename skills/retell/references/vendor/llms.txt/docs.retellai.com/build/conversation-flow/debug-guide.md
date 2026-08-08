> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Debug guide

> Diagnose and fix Retell conversation flow agent issues — wrong responses, missed transitions, and prompt problems — with a step-by-step guide.

Conversation flow is a powerful and flexible tool, which means that there's a lot of action items one can take when the agent's performance is not meeting your expectation. This guide is designed to help you identify the root cause of the issue, and provide actionable steps to improve the agent's responses and transitions.

<Note>This guide only covers the response part of the agent, if you have issues with agent audio, like pronunciation, please refer to other guides.</Note>

## Step 1: Identify the issue

When the agent is not responding as expected, there can be several reasons:

* The agent is not following instructions within a node
* Node transitions are not working as expected
* The actual conversation does not match the flow graph (e.g., users deviate from expected steps)

## Step 2: Fix the issue

Note that these issues are not mutually exclusive - you may need to implement multiple solutions to fully resolve the problem.

### Issue: Agent is not following instructions within a node

#### Split the node into multiple nodes

For example, if a node contains instructions to collect customer name, phone number, and address, the agent might inconsistently ask for only some of this information:

<Frame>
  <img src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/one_node.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=0084cbb508cb520b66d6290738a9fb95" alt="One node" width="874" height="826" data-path="images/one_node.png" />
</Frame>

You can improve consistency by splitting this into three separate nodes:

<Frame>
  <img src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/multiple_node.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=bd19b8e627feaf7bb2eb8763256406e2" alt="Three nodes" width="2086" height="1010" data-path="images/multiple_node.png" />
</Frame>

#### Change the node model

If the instructions are concise but the agent struggles to follow them, try using a more capable LLM model for this node.

#### Add conversation finetune examples

To achieve a specific response style, add conversation finetune examples. Learn more in our [Finetune Examples](/build/conversation-flow/finetune-examples) guide.

#### Adjust the LLM temperature

If the agent's responses are inconsistent, try adjusting the LLM temperature:

<img src="https://mintcdn.com/retellai/AJT6JQMM1II9WOl-/images/cf/model-selection.png?fit=max&auto=format&n=AJT6JQMM1II9WOl-&q=85&s=b4efb92e9fb30d002fbb6ecdf764d0f4" width="826" height="1000" data-path="images/cf/model-selection.png" />

### Issue: Node transitions are not working as expected

If the agent isn't transitioning to the expected node, try these solutions:

* Review your transition conditions: Ensure they precisely match your intended triggers. Consider prompt engineering or breaking down complex conditions into multiple simpler ones.
* Add transition finetune examples: Provide examples to help the model understand your expectations. See our [Finetune Examples](/build/conversation-flow/finetune-examples) guide.
* Remove numbered labels from prompts: Items like "PRIORITY 1" or "Step 2" in your global prompt or node instructions can be confused with transition options and trigger a transition whose condition was never met. Use letters ("PRIORITY A") or descriptive names instead — see the [transition conditions FAQ](/build/conversation-flow/transition-condition#faq).

To handle missing transition scenarios:

* Add more nodes to cover edge cases, particularly global nodes for handling unexpected situations. Learn more about [Global Nodes](/build/conversation-flow/global-node).
* Make transition conditions more flexible and general.

### Issue: Actual conversation does not match the flow graph

When users deviate from the defined flow:

* Add key steps as global nodes to allow users to skip or jump between nodes. This is particularly useful for inbound support cases without a rigid call structure. See our [Global Node](/build/conversation-flow/global-node) guide.
* Make node instructions more flexible and let the model handle the details naturally.
