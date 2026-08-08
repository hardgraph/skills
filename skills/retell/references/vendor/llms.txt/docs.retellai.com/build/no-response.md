> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Make agent hold and respond nothing

> Make a Retell agent stay silent on demand using the `NO_RESPONSE_NEEDED` stop sequence so it can hold for the caller without speaking or interrupting.

Sometimes you might want the agent to hold and not respond to the user, like when user says "hold on" or "give me a minute".

We hard-coded a stop sequence in the LLM: `NO_RESPONSE_NEEDED`. Whenever this sequence is met, the response generation stops.
You can then prompt the LLM to output nothing by writing something like:

```
- if user says "hold on", reply exactly the following: "NO_RESPONSE_NEEDED".
```
