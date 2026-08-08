> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Prompt guide & examples for specific situations

> Prompt examples for common voice agent situations — phone numbers, emails, dates, spellings, confirmations — many also available as Agent Handbook presets.

Here are some specific prompt guides and examples for common situations you might encounter in building a voice agent.

<Tip>Many of these patterns — including phone number pronunciation, email spelling, and speech normalization — are now available as one-click presets in the [Agent Handbook](/build/agent-handbook).</Tip>

## Pronounce the phone numbers

We'll utilize the Read Slowly feature to add pauses between words. For more information, see the [Speech Controllability documentation](https://docs.retellai.com/agent/speech-controllability#add-pauses-how-to-read-slowly).

It's recommended to include a prompt as a guideline for the LLM to follow. This ensures that the agent can consistently reply with the correct format, even if customers double-check the phone number.

```
When people ask about your phone number, your phone number is 4158923245

## Guideline
When speaking the phone number, transform the format as follows:
- Input formats like 4158923245, (415) 892-3245, or 415-892-3245
- Should be pronounced as: "four one five - eight nine two - three two four five"
- Important: Don't omit the space around the dash when speaking
```

Listen to this audio clip for a demonstration of proper phone number pronunciation:

[Speak Phone Number Example](https://retell-utils-public.s3.us-west-2.amazonaws.com/speak_phone_number.wav)

## Pronounce the email

```
## How to spell out
The possible email format is name@company.com 
to spell out an email address is n-a-m-e-@-c-o-m-p-a-n-y-dot-com,
@ is pronounced by "at". 
```

## Pronounce the website

```
Whenever you encounter a website URL, please:
Identify each segment of the domain name.
If a segment consists of individual letters (e.g., "NK"), pronounce each letter using its spoken form in English (e.g., "N" → "en," "K" → "kay").
If a segment is a recognizable word (e.g., "laundry"), pronounce it normally as that word.
Pronounce "dot" before stating the top-level domain (e.g., "dot com," "dot net," "dot org," etc.).
Example:
"nklaundry.com" → "en-kay-laundry dot com"
"abctest.net" → "A B C test dot net"
"xyzco.org" → "ex-why-zee-co dot org"
Adhere to this phonetic breakdown carefully to ensure clarity and proper pronunciation for customers.
```

## Pronounce the time

```
For State Numbers, Times & Dates
For 1:00 PM, say "One PM."
For 3:30 PM, say "Three thirty PM."
For 8:45 AM, say "Eight forty-five AM."
Never say O'clock. Instead, just say O-Clock.
Always say "AM" or "PM".
```

## Handle being put on hold / no response needed

### For non-reasoning models

We hard-coded a stop sequence in the LLM: `NO_RESPONSE_NEEDED`. Whenever this sequence is met, the response generation stops.
You can then prompt the LLM to output nothing by writing something like:

```
- when user says hold on, reply exactly the following: "NO_RESPONSE_NEEDED".
```

### For reasoning models

<Warning>
  Reasoning models like `gpt 5`, `gpt-5.1` do not support this feature. You need to prompt engineer it differently to achieve this.
</Warning>

You can try the following prompt: `When user says hold on, simply do not respond`.
