> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# End node

> Use an end node in a Retell conversation flow to terminate the call cleanly, optionally with a closing message generated from a prompt or static text.

The end node ends the call. It's a terminal node — it has no outgoing edges, and the call ends the moment the agent enters it. You can add as many end nodes as you need, one for each way a call can finish.

By default the agent hangs up immediately, which callers experience as abrupt. Enable **Speak During Execution** so the agent says a closing line first.

<Frame caption="An end node with Speak During Execution enabled for a closing message.">
  <img src="https://mintcdn.com/retellai/LPjiIxDz6s4F2qHE/images/cf/end-node.png?fit=max&auto=format&n=LPjiIxDz6s4F2qHE&q=85&s=05ab60919489306c63cc0fff022544f5" alt="End node on the flow canvas with the Speak During Execution setting enabled and a closing message instruction" width="1386" height="434" data-path="images/cf/end-node.png" />
</Frame>

For example, an appointment-booking flow can end with a static closing line — "You're all set for Tuesday at 2 PM. Goodbye!" — after the booking succeeds, and use a second end node with a different message on the cancellation path.

## Node settings

* **Speak During Execution**: When enabled, a text box appears where you define the closing message the agent speaks before hanging up. Choose **Prompt** to let the LLM generate a farewell from your instruction, or **Static Sentence** for an exact line like `Goodbye, have a nice day`.
* **Global Node**: Make the end node reachable from anywhere in the flow when its condition is met — for example `User wants to end the call or says goodbye` — so you don't have to wire an edge to it from every node. Read more at [global node](/build/conversation-flow/global-node).

## End node vs End Call tool

Use an end node when the call should end at a fixed point in the flow. If instead the agent should be able to end the call mid-dialogue whenever it judges the conversation complete, attach an [End Call tool](/build/single-multi-prompt/end-call) to a [subagent node](/build/conversation-flow/subagent-node).
