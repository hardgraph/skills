> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Best practices

> Keep a Retell custom LLM agent fast and reliable: cut time to first sentence, write prompts for speech, control tool accuracy, and survive dropped connections.

How a custom LLM agent sounds on a real call depends less on the model than on how fast the first sentence arrives, how the prompt is written for speech, and what happens when something fails mid-call.

## Optimize time to first sentence

Retell starts speaking as soon as it has your first complete sentence. So the number that matters is **time to first token plus the time to finish that first sentence**, not total generation time.

* **Stream, don't buffer.** Sending the full response in one event puts the entire generation in the caller's silence.
* **Run close to Retell.** Retell's primary infrastructure is in the US. Host your server and pick your model region accordingly, since a cross-region round trip adds to every turn.
* **Watch provider-side preprocessing.** Anything that runs before generation adds to first-token time. On Azure OpenAI, for example, [content filtering](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/content-filter) runs synchronously by default; switching it to asynchronous mode or turning it off returns responses noticeably faster.
* **Check your quota before you scale.** Provider rate limits produce queuing that looks exactly like a slow model.

## Write prompts for speech

Everything the model writes gets read aloud, which changes what a good prompt looks like.

* **Ban formatting.** No markdown, no bullet lists, no emoji, no headers. Say so explicitly, because models default to writing for a screen.
* **Cap the length.** Long turns feel like being lectured and are painful to interrupt. Ask for one idea per turn.
* **Plan for transcription errors.** The transcript comes from speech recognition and will contain mistakes. Tell the model to infer the caller's meaning rather than asking them to repeat, and when it must ask, to do it conversationally ("sorry, you cut out") instead of mentioning transcription.
* **Allow a little imperfection.** Occasional filler words and contractions make the agent sound human; perfectly polished prose doesn't.
* **Keep prompts short.** Longer prompts cost first-token time and often *reduce* instruction-following. If you have a large knowledge base, retrieve the few relevant chunks per turn instead of pasting everything.

The [prompt engineering guide](/build/prompt-engineering-guide) covers voice prompting in more depth. It's written for Retell agents, but the guidance applies to any model.

## Improve tool accuracy

* **Describe when *not* to call.** "End the call only when the caller has clearly said goodbye" prevents far more misfires than a description of what the function does.
* **Give tools a `message` parameter.** Most providers return either a tool call or text, not both. Without a parameter carrying something to say, the agent goes silent exactly when the caller is waiting.
* **Constrain the tool set.** Ten tools available at once invites wrong choices. Expose only what's reachable from the current point in the conversation.

## Track state on your server

Past a couple of tools, asking the model to infer where it is from the transcript gets unreliable. Keep explicit per-call state on your server, keyed on the call ID from the WebSocket URL, and use it to control what the model can do at each step, like an IVR tree with an LLM at each node.

At each state you decide the prompt, which tools are exposed, and which transitions are legal. That gets you shorter prompts, fewer wrong tool calls, and protection against duplicate side effects.

## Build for failure

* **Turn on `auto_reconnect`.** Retell already rebuilds a connection that closes abnormally. What keepalives add is catching a half-dead socket that never sends a close frame, which otherwise hangs the call until it times out. The cost is echoing `ping_pong`.
* **Never leave a response unclosed.** Send `content_complete: true` in a `finally` block. If your provider errors or times out and you skip it, the turn never finishes: the agent stops speaking, nothing errors, and it only recovers once the caller speaks again.
* **Have a fallback for provider outages.** A secondary model, or at minimum a spoken apology and a transfer, degrades better than silence.
* **Make side effects idempotent.** Retell discards responses when the caller keeps talking and asks again, so a side-effecting tool routinely runs twice for one request. Key the write on the call ID plus the arguments so the repeat is a no-op. Checking whether the `response_id` is still current helps too, but it can't catch a write that committed before the newer request arrived. See [don't double-book](/integrate-llm/integrate-function-calling#going-to-production).
* **Log the raw frames.** Store what you received and sent, keyed on call ID, so you can line your logs up against Retell's [Detail Logs](/features/session-history) when something goes wrong.

## Related

* [Troubleshooting](/integrate-llm/troubleshooting) — disconnection reasons and the silent-failure traps
* [Connect your LLM](/integrate-llm/integrate-llm) — streaming and discarded-response handling
* [LLM WebSocket reference](/api-references/llm-websocket) — every field on every event
