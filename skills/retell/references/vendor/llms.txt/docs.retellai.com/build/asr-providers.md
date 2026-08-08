> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Speech recognition providers

> An overview of Retell's speech recognition providers — Azure, Deepgram, and Soniox — and how Retell auto-routes based on your agent's configured languages.

Retell automatically picks a speech recognition provider based on the languages your agent is configured for. You don't need to choose one manually, but it helps to understand what each provider offers.

<Note>
  These observations are based on our internal testing and routing rules. Results may vary depending on the specific languages, audio conditions, and call patterns.
</Note>

## Provider overview

### Azure

* **Best for:** Broad single-language coverage, including many rare and less common languages.
* **Language support:** Wide range of individual languages; single-language only.
* **Latency:** \~530ms

### Deepgram

* **Best for:** Common languages with low latency.
* **Language support:** Code-switching across 10 languages: English, Spanish, French, German, Hindi, Russian, Portuguese, Japanese, Italian, Dutch.
* **Latency:** \~300ms

### Soniox

* **Best for:** Multilingual agents that need any-to-any code-switching across a wide set of languages.
* **Language support:** 60+ languages with a single model; same coverage in both single- and multi-language modes.
* **Latency:** \~490ms

## How Retell picks a provider

Retell routes to a provider based on what your agent is optimized for:

* **Optimize for latency** → Retell picks the fastest provider that still performs well for your languages.
* **Optimize for accuracy** → Retell picks the best-performing provider for your languages.
* **Multilingual** → the languages you select also influence which providers are available.

If no single provider can cover all of your selected languages together, the dashboard prevents you from selecting that combination. See [Configure a multilingual agent](/agent/multilingual) for details.

For which languages each provider transcribes, see [Language support by provider](/build/language-support).
