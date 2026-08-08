> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# TTS provider comparison

> Compare Retell TTS providers — ElevenLabs, Cartesia, and MiniMax — on spelling accuracy, voice naturalness, pacing, and accent support.

When selecting a TTS provider, consider the trade-offs between spelling accuracy (pronouncing spelled-out words like "W - O - R - D"), voice naturalness, pacing/tone consistency, and accent support.

<Note>
  These observations are based on our internal testing. Results may vary depending on the specific voice, model, or language used.
</Note>

## Provider Overview

### ElevenLabs

* **Best for:** Most natural sounding; best support for niche accent needs (e.g., Australian English)
* **Consideration:** You may occasionally notice small pacing/tone quirks; less reliable for exact spelling

### Cartesia

* **Best for:** Natural sounding with stronger spelling than ElevenLabs
* **Consideration:** Pacing/tone can sometimes be less consistent than ElevenLabs; localization may be weaker for certain accents

### MiniMax

* **Best for:** Strongest spelling + most consistent tone (rarely has pacing/tone quirks); great for Asian languages
* **Consideration:** Voice sound can sometimes feel more robotic compared to other providers

## Rules of Thumb

* Need most natural sound → ElevenLabs (or Cartesia)
* Need spelling accuracy → MiniMax (or Cartesia)
* Need most consistent tone → MiniMax
* Need specific accents → any provider can work, but ElevenLabs tends to perform best for niche accents
* Using an Asian language → MiniMax

For which languages each voice provider and model supports, see [Language support by provider](/build/language-support).
