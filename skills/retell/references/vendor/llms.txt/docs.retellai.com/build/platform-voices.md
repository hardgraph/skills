> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Platform voices

> Retell platform voices are a curated library tuned for phone conversations — clear at telecom bitrates, with automatic TTS fallback and no extra setup.

Platform voices are Retell's curated voice library, fine-tuned specifically for conversational AI over the phone. They handle fillers, pacing, and conversational rhythm well, and are calibrated for clarity at telecom bitrates.

When using a platform voice, fallback is handled automatically to maintain the same voice experience — you do not need to configure a TTS fallback plan in Security & Fallback Settings.

<Note>
  If you are using a non-platform voice, you will need to [configure TTS fallback](/build/tts-fallback) manually.
</Note>

## Select a platform voice

Platform voices are available in the voice selector in your agent settings. You can preview each voice directly in the dashboard before selecting.

## Custom voice cloning

You can clone your own voice as a platform voice by setting `voice_provider` to `platform` when calling the [Clone Voice](/api-references/clone-voice) API. The cloned voice will behave the same as a built-in platform voice — fallback is handled automatically and you do not need to configure TTS fallback separately.
