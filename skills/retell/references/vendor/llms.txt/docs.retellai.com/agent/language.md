> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Set language for your agent

> Set a single language for a Retell agent to control speech recognition, voice pronunciation, and the language the agent uses to respond during calls.

You can set a specific language for an agent. The language setting affects three things during a call:

* **Speech recognition** — which language the agent transcribes the caller from.
* **Voice pronunciation** — the language the voice uses to pronounce words and shape its accent.
* **Agent text** — the agent is automatically instructed to respond in the configured language. You do **not** need to add a "respond in X" instruction to your prompt.

The voice you select is still the primary determinant of accent — the language setting refines pronunciation rules and ensures the right speech recognition model is used.

## Pick a single language (recommended)

A single-language agent is the most accurate setup: the agent transcribes in one language only, the voice speaks with that language's pronunciation, and the agent always responds in that language — no language detection involved.

<img src="https://mintcdn.com/retellai/SGc6xrwrS64ZRERp/images/language-single.png?fit=max&auto=format&n=SGc6xrwrS64ZRERp&q=85&s=bc78e8e6ff07f8516726414e9285f303" width="600" height="996" data-path="images/language-single.png" />

If no language is selected, the agent defaults to **English (US)**.

The chosen voice must support the chosen language. The dashboard hides unsupported combinations from the picker, with a tooltip explaining which voice or voice model is blocking it.

## Need more than one language?

<Card title="Configure a multilingual agent" icon="globe" href="/agent/multilingual">
  For agents that serve callers in different languages, see the multilingual guide.
</Card>

If you already know the caller's language at call time (for example, from CRM context or the dialed number), prefer overriding the language per call via the [inbound call webhook](/features/inbound-call-webhook). This gives you single-language accuracy on each call without limiting which customers the agent can serve.

<Note>
  Speech recognition often confuses closely related languages — for example **Cantonese vs. Mandarin Chinese**, or Cantonese mixed with English. If your callers speak these, keep the agent single-language and route each caller to the right agent (or set the language per call) instead of relying on auto-detect. See [Closely related languages are hard to distinguish](/agent/multilingual#closely-related-languages-are-hard-to-distinguish) for details.
</Note>

## Supported languages

The language you pick must be supported by both your chosen **voice provider** (for pronunciation) and at least one **speech recognition provider** (for transcription). The combinations change as providers add coverage, so the dashboard is the source of truth.

On any agent page, open the language picker — the dropdown shows the live set of supported languages and updates as you change the voice provider, voice (and pinned voice model), and ASR provider. The same picker works in both single-language and multiselect modes. Unsupported combinations are greyed out, with a tooltip explaining which selection is blocking each one.

For a full per-language breakdown of which voice and speech recognition providers cover each language, see [Language support by provider](/build/language-support). Still picking your stack? See the [TTS provider comparison](/build/tts-provider-comparison) and [speech recognition providers](/build/asr-providers) for provider-level coverage and trade-offs.

For locale variants (for example, British English vs. US English, European Spanish vs. Latin American Spanish), the dashboard's language picker exposes the specific dialect codes when supported.
