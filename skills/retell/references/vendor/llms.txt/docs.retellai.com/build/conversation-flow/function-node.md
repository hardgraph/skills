> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Function Node Overview

> Call any prebuilt or custom function from a Retell conversation flow function node — the agent fires the function on entry and transitions on the result.

Function node is used to call a function, whether it's a pre-built function or a custom function. It's not intended for having a conversation with the user, but the agent can still talk while in this node if needed.

The function associated with this node will be called when entering this node.

<img src="https://mintcdn.com/retellai/XoiOOpBkax1duk4y/images/cf/function-node.png?fit=max&auto=format&n=XoiOOpBkax1duk4y&q=85&s=de416b3c88ad9bd960a58d5199848877" width="1540" height="946" data-path="images/cf/function-node.png" />

## Add a Function

Here you need to add the function first, and then select it inside the node. This way if you delete the node, you don't need to re-create the function again.

<img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/add-function.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=df6c4968c14799e166c19b3cb0c679fb" width="587" height="254" data-path="images/cf/add-function.jpeg" />

For specific instructions on different types of functions:

* [Custom Function](/build/conversation-flow/custom-function)
* Pre-built Functions:
  * [Check Calendar Availability](/build/check-availability)
  * [Book Calendar](/build/book-calendar)

## When Can Transition Happen

* if `wait for result` is turned off
  * if `talk while waiting` is turned on, the agent will transition once done talking
  * if `talk while waiting` is turned off, the agent will transition immediately after function gets invoked, which is right upon entering the node
  * if the user interrupts the agent, the transition can also happen once the user is done speaking
* if `wait for result` is turned on
  * if `talk while waiting` is turned on, the agent will transition once function result is ready and agent is done talking
  * if `talk while waiting` is turned off, the agent will transition once function result is ready
  * if the user interrupts the agent, the transition can also happen once function result is ready and the user is done speaking

Given that the function node takes function result into consideration for transition timing, you can write your transition condition to be based on the function result.

## Node Settings

* **Talk While Waiting**: when enabled, a text input box will show up where you can write instructions for the agent to follow to generate an utterance like `Let me check that for you.` to say while the function is being executed. You can choose between `Prompt` and `Static Sentence`.
* **Wait for Result**: when enabled, the agent will wait for the function to finish executing before attempting to transition to any other node. This guarantees that when you reach the next node, the result is already ready to be used.
* **Global Node**: read more at [Global Node](/build/conversation-flow/global-node)
* **LLM**: choose a different model for this particular node. Will be used for function argument generation, and potentially speak during execution message generation.
* **Fine-tuning Examples**: Can finetune transition. Read more at [Finetune Examples](/build/conversation-flow/finetune-examples)

## How to Tell User the Result

Since the function node is not intended for having a conversation with the user, you will need to attach a conversation node to the function node to tell the user the result. You can create different conversation nodes for different function results, so that it can engage the user in different ways when function result varies.
