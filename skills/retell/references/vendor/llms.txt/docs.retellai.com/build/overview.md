> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Build a Retell agent: configuration overview

> Start building a Retell agent: pick an agent type, then configure the rest, including speech recognition, LLM responses, voice, and telephony behavior.

Building an agent means deciding how it understands callers, decides what to say, sounds when it speaks, and handles the parts of a call beyond talking, like voicemail and transfers. This page orients you across all of it. Start with [choosing an agent type](#choose-an-agent-type), since that decision shapes which other settings apply.

<Frame caption="Start from the Agents page in your dashboard.">
  <img src="https://mintcdn.com/retellai/wM4UcKxv3px1bKim/images/create-agent-dropdown.png?fit=max&auto=format&n=wM4UcKxv3px1bKim&q=85&s=06dddc89a70e150d4771e364d2ad9534" alt="Create an Agent dropdown with Voice Agent and Chat Agent options" width="1303" height="733" data-path="images/create-agent-dropdown.png" />
</Frame>

## Choose an agent type

Every agent is either a **voice agent** (talks on phone calls) or a [**chat agent**](/build/create-chat-agent) (text-only, reachable via API, an embeddable widget, or SMS). When creating a voice agent, choose between:

<CardGroup cols={2}>
  <Card title="Conversation Flow Agent" icon="diagram-project" href="/build/conversation-flow/overview">
    Build a node-by-node call flow with explicit transitions. Best for structured, multi-step, or high-stakes calls where you need predictable behavior.
  </Card>

  <Card title="Single Prompt Agent" icon="file-lines" href="/build/single-multi-prompt/prompt-overview">
    Define the whole agent with one prompt. Fastest to set up, and a good fit for simple, linear conversations with a handful of functions.
  </Card>
</CardGroup>

<Note>
  The dashboard also lists **Multi-Prompt Agent** as a legacy option. It still works for agents already built on it, but build new structured agents on a conversation flow instead. See the [single/multi-prompt overview](/build/single-multi-prompt/prompt-overview) if you're maintaining an existing multi-prompt agent.
</Note>

## Understand what callers say

Retell picks a speech recognition [provider](/build/asr-providers) for your agent's configured [languages](/build/language-support). You can also tune [how transcription behaves](/build/transcription-mode) mid-call and [filter out background noise](/build/handle-background-noise) so it doesn't get transcribed as speech.

## Generate better responses from the LLM

Beyond the prompt itself, you can turn on [Agent Handbook](/build/agent-handbook) presets for personality, accuracy, and safety, enable [Conversational Mode](/build/conversational-mode) to make replies sound more natural, and follow the [prompt engineering guide](/build/prompt-engineering-guide) for structure and edge cases. [LLM options](/build/llm-options) control model-level settings, including temperature, fast tier, and structured output.

## Customize the voice

Pick a [platform](/build/platform-voices) or [custom voice](/build/voice), including cloned voices, and tune how it delivers speech: [expressiveness](/build/expressive-mode), [pauses](/build/add-pause), [holding silently when the caller asks it to wait](/build/no-response), and a [fallback TTS provider](/build/tts-fallback) if the primary one fails.

## Handle telephony scenarios

Configure what the agent does when a call isn't a normal back-and-forth: detect and respond to [voicemail and IVR menus](/build/handle-voicemail), capture [DTMF keypad input](/build/user-dtmf) from the caller, and pass metadata between systems with [custom SIP headers](/build/telephony/sip-headers).

## Give your agent context

Attach a [knowledge base](/build/knowledge-base) of URLs, documents, or text so the agent retrieves accurate answers instead of relying only on the prompt, and inject per-call data like names or account IDs with [dynamic variables](/build/dynamic-variables).

## Add guardrails

Turn on [guardrails](/build/guardrails) to automatically catch prohibited topics in the agent's responses and jailbreak attempts from the caller, replacing them with a safe message instead of ending or transferring the call.

## Test and deploy

Once the agent is configured, validate it in the [LLM Playground or simulation testing](/test/test-overview), then [purchase a number](/deploy/purchase-number) or [connect your own via SIP](/deploy/custom-telephony) to take calls.

If you manage agent configuration in code, follow the [TypeScript SDK agent creation guide](/get-started/create-agent-with-sdk) to create the response engine and agent, test the draft, and publish a version.
