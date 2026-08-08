> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add pause or read slowly

> Control speech pacing in Retell agents by adding spaced dashes for short pauses and longer Read Slowly markup for phone numbers, addresses, and confirmations.

Although you can adjust general speed of the audio by changing the voice speed, you might want to
slow down the agent's speech only at certain points (like reading phone numbers).
You can do this by prompting the LLM and generating text with `-` in between (note, the space around `-` is important):

```json theme={"dark"}
The number is 2 - 1 - 3 - 4
```

<Warning>
  Note: The spaces around the dash (`-`) are important for proper pausing behavior.
</Warning>

### How to add long pauses

Sometimes you might want to add longer pauses to the conversation. You can do this by adding multiple `-` in between the words.

Important: The spaces around the dash (`-`) are important for proper pausing behavior:

```json theme={"dark"}
The number is 2 -  -  -  - 1 -  -  -  - 3
// Notice the double spaces between the dashes
```
