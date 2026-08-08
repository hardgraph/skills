> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Finetune Examples

> Add finetune examples to Retell conversation, subagent, and function nodes to correct unexpected responses or transitions with concrete transcript snippets.

When agent response or transition is not meeting your expectation, you might want to supply some examples to finetune the behavior. You can do so by adding `finetune examples`.

Here are the nodes that support finetune examples:

* Conversation Node: support finetune examples for response and transition
* Subagent Node: support finetune examples for response and transition
* Function Node: support finetune examples for transition

When configuring the finetune example, you will provide a transcript as the context. You can select `user`, `agent`, `function` as the role of the transcript. When selecting `function` as the role, you can fill out both the invocation and result of the function. Refer to the History tab of the dashboard to see examples of transcripts.

## Finetune Examples for Conversation

<img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/finetune-conversation.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=bd018081e36b4f1535b7cb9d06eb3596" width="790" height="895" data-path="images/cf/finetune-conversation.jpeg" />

Supplying a transcript as the context is everything you need to do. It's not necessary to provide the entire call transcript, you can simply provide the relevant part. Please note that at least one `agent` response is required, as this is finetuning the agent's response.

## Finetune Examples for Transition

<img src="https://mintcdn.com/retellai/zL2HeUqUnagEN9eK/images/cf/finetune-transition.jpeg?fit=max&auto=format&n=zL2HeUqUnagEN9eK&q=85&s=bb20a29af46e1714bb7dabbc6650da8e" width="797" height="626" data-path="images/cf/finetune-transition.jpeg" />

Here you need to provide both a transcript as context, and the transition result. If you cannot distinguish between the different nodes available as transition target, you can try to rename your nodes to make it easier to distinguish.

<Note>
  [Global nodes](/build/conversation-flow/global-node#global-node-examples) also take example conversations, but they follow a different rule: because a global node is only evaluated after a user turn, each example should end with a user utterance.
</Note>
