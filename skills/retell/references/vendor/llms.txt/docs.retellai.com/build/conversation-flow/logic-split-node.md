> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Logic Split Node

> Use a logic split node to branch a Retell conversation flow on rules — the agent evaluates conditions on entry and routes silently to the next node.

Logic split node is used to branch out the conversation flow based on the conditions. When entering this node, the agent will immediately evaluate the conditions and branch out to the corresponding destination nodes. The agent would not speak in this node, and the time spent in this node is minimal.

It can come in handy when you want to further split the conversation flow based on the conditions, and do not want to stack all your conditions in previous nodes. It can also be hard for agent to handle a bunch of conditions all at once, so this node can help break it down. It can also be useful when you want to branch out based on dynamic variables.

<img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/logic-split-node.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=bc5bf2d029c71ffe1c3931d58d694fe9" width="1003" height="444" data-path="images/cf/logic-split-node.jpeg" />

## When Can Transition Happen

Transition happens immediately when agent enters this node.

## Configure branching logic

* add conditions just like you would in other nodes
* set up the else destination: there will always be an else condition, which will be the default destination if none of the conditions are met, because this node is designed to be a split point and you want to make sure the conversation flow is not stuck here.

## Rest of Node Settings

* **Global Node**: read more at [Global Node](/build/conversation-flow/global-node)
* **Fine-tuning Examples**: Can finetune transition. Read more at [Finetune Examples](/build/conversation-flow/finetune-examples)
