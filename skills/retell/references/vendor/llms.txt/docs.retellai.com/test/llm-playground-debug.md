> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Debug your agent response

> Debug a Retell agent in the LLM Playground: open Debug on any response or transition, apply suggested fixes, and regenerate the turn to verify.

When your agent answers wrong or takes the wrong path, Debug lets you fix that exact turn instead of guessing at the whole prompt or flow. Open it on a specific response or transition in the [LLM Playground](/test/llm-playground) to see the fixes that usually help, then regenerate the turn to check your change before it reaches a real call.

<Note>
  Debug is a Playground feature, and what it offers depends on the agent. [Conversation Flow](/build/conversation-flow/overview) agents get the full set, including transition debugging and node-level fixes. Single-prompt and multi-prompt agents get response and tool-call debugging, with temperature as the suggested fix.
</Note>

<Frame caption="Opening the Debug panel on a turn in the LLM Playground.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/IwEyQ7Z17TwMRUlF/images/test-llm/debug-panel-intro.mp4?fit=max&auto=format&n=IwEyQ7Z17TwMRUlF&q=85&s=1bfe4b378e813f9b6bc54c4a63b2dd92" data-path="images/test-llm/debug-panel-intro.mp4" />
</Frame>

## Fix a wrong response

<Steps>
  <Step title="Open Debug on the response">
    Click the <code className="ui-btn">Debug</code> button on the agent's response.
  </Step>

  <Step title="Review the suggested fixes">
    <code className="ui-btn">Debug the AI response</code> lists the fixes that usually help. Each one links to a guide you follow to make the change yourself:

    * [Add Fine-tuning Examples](/build/conversation-flow/debug-guide#add-conversation-finetune-examples) to teach the wording you want.
    * [Split One Node into Two Nodes](/build/conversation-flow/debug-guide#split-the-node-into-multiple-nodes) when one node is doing too much.
    * [Adjust LLM Temperature](/build/conversation-flow/debug-guide#adjust-the-llm-temperature) to trade variety for consistency.

    On a single-prompt or multi-prompt agent, temperature is the only fix offered, since there are no nodes to split.
  </Step>

  <Step title="Regenerate to confirm">
    Click <code className="ui-btn">Regenerate the answer</code> to produce a new response using the agent's current configuration, so once you've applied a fix you can see the new behavior. Use <code className="ui-btn">Regenerate 10 answers</code> to check how consistent the responses are across runs.
  </Step>
</Steps>

<Frame caption="Debugging a wrong response.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/IwEyQ7Z17TwMRUlF/images/test-llm/debug-regenerate-response.mp4?fit=max&auto=format&n=IwEyQ7Z17TwMRUlF&q=85&s=0d3f0e4ac56aa88e10c80551e50fb6b4" data-path="images/test-llm/debug-regenerate-response.mp4" />
</Frame>

## Fix a transition problem

When the agent moves to the wrong node, or doesn't move when it should:

<Steps>
  <Step title="Open Debug on the transition">
    Click <code className="ui-btn">Debug</code> on the agent's response and select <code className="ui-btn">Didn't transition as expected?</code>, or click <code className="ui-btn">Debug</code> directly on the transition dialog.
  </Step>

  <Step title="Review the suggested fixes">
    Each fix links to a guide you follow to make the change yourself:

    * [Add Fine-tuning Transition Examples](/build/conversation-flow/finetune-examples#finetune-examples-for-transition).
    * [Split One Node into Two Nodes](/build/conversation-flow/debug-guide#split-the-node-into-multiple-nodes).
  </Step>

  <Step title="Regenerate the transition">
    Click <code className="ui-btn">Regenerate the transition</code> to try again with the agent's current configuration, or <code className="ui-btn">Regenerate 10 transitions</code> to see which node the agent picks across 10 attempts.
  </Step>
</Steps>

<Frame caption="Debugging a transition.">
  <video muted loop playsInline preload="metadata" className="scroll-play w-full aspect-video rounded-xl" src="https://mintcdn.com/retellai/IwEyQ7Z17TwMRUlF/images/test-llm/debug-regenerate-transition.mp4?fit=max&auto=format&n=IwEyQ7Z17TwMRUlF&q=85&s=45ce74f6456ae992a70a63b38417fa91" data-path="images/test-llm/debug-regenerate-transition.mp4" />
</Frame>

## Fix a wrong tool call

When the agent calls the right function with the wrong arguments, or calls one it shouldn't, hover the <code className="ui-btn">Tool Invocation</code> row in the transcript and click <code className="ui-btn">Debug</code>. <code className="ui-btn">Debug the tool invocation</code> works like the response popup: it links to the fixes that apply, then <code className="ui-btn">Regenerate the tool invocation</code> retries that call with the agent's current configuration and <code className="ui-btn">Regenerate 10 tool invocations</code> shows how often each set of arguments comes up.

## Fix inconsistent responses

When the same prompt gives different answers, click <code className="ui-btn">Debug</code> on the response. The popup links to guides for the fixes that help most: [add fine-tuning examples](/build/conversation-flow/debug-guide#add-conversation-finetune-examples), [split the node](/build/conversation-flow/debug-guide#split-the-node-into-multiple-nodes), or [adjust the temperature](/build/conversation-flow/debug-guide#adjust-the-llm-temperature).

Each debug popup has a 10-run option: <code className="ui-btn">Regenerate 10 answers</code>, <code className="ui-btn">Regenerate 10 transitions</code>, and <code className="ui-btn">Regenerate 10 tool invocations</code>. Each one replays that turn 10 times and lists every distinct result with a count out of 10, so you can see the agent's most common output on a non-deterministic turn and confirm it holds steady after a fix.

## Next steps

* [Manually test your agent](/test/llm-playground) covers the rest of the Playground.
* [Testing overview](/test/test-overview) compares the Playground with simulation and live call testing.
