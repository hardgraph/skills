> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Supported languages by provider

> Which Retell text-to-speech and speech recognition providers support each language, with the codes accepted by the API and model-level restrictions.

Every language your agent uses must be supported by **both** a voice (text-to-speech) provider and a speech recognition (ASR) provider. This page lists, for each language, which providers cover it.

<Note>
  The dashboard's language picker is the live source of truth — it greys out combinations that aren't supported and updates as providers add coverage. Use these tables for planning; confirm the exact combination in the dashboard. See [Set language for your agent](/agent/language) and [Configure a multilingual agent](/agent/multilingual) for how selection works.
</Note>

Codes are the values accepted by the API `language` field. Where a language has multiple locale variants (for example, US vs. British English), they share the same provider support.

## Text-to-speech (voice) support

A ✓ means the language is supported by **at least one** of that provider's voice models. Some providers gate certain languages to specific models — see [Model-level restrictions](#model-level-restrictions) below.

| Language    | Code(s)                                     | Platform | MiniMax | Fish Audio | ElevenLabs | Cartesia | OpenAI |
| ----------- | ------------------------------------------- | :------: | :-----: | :--------: | :--------: | :------: | :----: |
| Afrikaans   | `af-ZA`                                     |     ✓    |    ✓    |      ✓     |      ✓     |          |    ✓   |
| Arabic      | `ar-SA`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Armenian    | `hy-AM`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Azerbaijani | `az-AZ`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Bosnian     | `bs-BA`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Bulgarian   | `bg-BG`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Cantonese   | `yue-CN`                                    |     ✓    |    ✓    |            |            |          |        |
| Catalan     | `ca-ES`                                     |     ✓    |    ✓    |      ✓     |      ✓     |          |    ✓   |
| Chinese     | `zh-CN`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Croatian    | `hr-HR`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Czech       | `cs-CZ`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Danish      | `da-DK`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Dutch       | `nl-NL`, `nl-BE`                            |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| English     | `en-US`, `en-IN`, `en-GB`, `en-AU`, `en-NZ` |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Filipino    | `fil-PH`                                    |     ✓    |    ✓    |            |      ✓     |     ✓    |    ✓   |
| Finnish     | `fi-FI`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| French      | `fr-FR`, `fr-CA`                            |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Galician    | `gl-ES`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| German      | `de-DE`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Greek       | `el-GR`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Hebrew      | `he-IL`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Hindi       | `hi-IN`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Hungarian   | `hu-HU`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Icelandic   | `is-IS`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Indonesian  | `id-ID`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Italian     | `it-IT`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Japanese    | `ja-JP`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Kannada     | `kn-IN`                                     |          |         |      ✓     |      ✓     |     ✓    |    ✓   |
| Kazakh      | `kk-KZ`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Korean      | `ko-KR`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Latvian     | `lv-LV`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Lithuanian  | `lt-LT`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Macedonian  | `mk-MK`                                     |          |         |            |      ✓     |          |    ✓   |
| Malay       | `ms-MY`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Marathi     | `mr-IN`                                     |          |         |      ✓     |      ✓     |     ✓    |    ✓   |
| Nepali      | `ne-NP`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Norwegian   | `no-NO`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Persian     | `fa-IR`                                     |     ✓    |    ✓    |      ✓     |      ✓     |          |    ✓   |
| Polish      | `pl-PL`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Portuguese  | `pt-PT`, `pt-BR`                            |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Romanian    | `ro-RO`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Russian     | `ru-RU`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Serbian     | `sr-RS`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Slovak      | `sk-SK`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Slovenian   | `sl-SI`                                     |     ✓    |    ✓    |            |      ✓     |          |    ✓   |
| Spanish     | `es-ES`, `es-419`                           |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Swahili     | `sw-KE`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Swedish     | `sv-SE`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Tamil       | `ta-IN`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Thai        | `th-TH`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Turkish     | `tr-TR`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Ukrainian   | `uk-UA`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Urdu        | `ur-IN`                                     |          |         |      ✓     |      ✓     |          |    ✓   |
| Vietnamese  | `vi-VN`                                     |     ✓    |    ✓    |      ✓     |      ✓     |     ✓    |    ✓   |
| Welsh       | `cy-GB`                                     |          |         |      ✓     |      ✓     |          |    ✓   |

### Model-level restrictions

Most providers support the same languages across all of their models. The exceptions:

**ElevenLabs**

| Model                    | Languages                                                                                                                                                                                                                                                                                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `eleven_v3`              | All languages in the ElevenLabs column above                                                                                                                                                                                                                                                     |
| `eleven_flash_v2_5`      | English, Japanese, Chinese, German, Hindi, French, Korean, Portuguese, Italian, Spanish, Indonesian, Dutch, Turkish, Filipino, Polish, Swedish, Bulgarian, Romanian, Arabic, Czech, Greek, Finnish, Croatian, Malay, Slovak, Danish, Tamil, Ukrainian, Russian, Hungarian, Norwegian, Vietnamese |
| `eleven_multilingual_v2` | Same as `eleven_flash_v2_5` above, **minus** Hungarian, Norwegian, and Vietnamese                                                                                                                                                                                                                |
| `eleven_flash_v2`        | English only                                                                                                                                                                                                                                                                                     |

**Fish Audio**

| Model                | Languages                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `s2-pro`, `s2.1-pro` | All languages in the Fish Audio column above                                                                     |
| `s1`                 | English, Chinese, Japanese, German, French, Spanish, Korean, Arabic, Russian, Dutch, Italian, Polish, Portuguese |

**OpenAI** (`tts-1`, `gpt-4o-mini-tts`), **Cartesia** (`sonic-3`, `sonic-3-latest`, `sonic-3.5`), and **MiniMax** (`speech-02-turbo`, `speech-2.8-turbo`) support their full language set on every model.

## Speech recognition (ASR) support

A ✓ means the provider can transcribe that language. By default Retell auto-routes across providers based on your agent's languages and whether it's optimized for latency or accuracy. You can also pin a specific provider manually with a custom speech-to-text config, as long as it supports every language your agent uses. See [Speech recognition providers](/build/asr-providers).

| Language    | Code(s)                                     | Deepgram | Azure | Soniox | AssemblyAI |
| ----------- | ------------------------------------------- | :------: | :---: | :----: | :--------: |
| Afrikaans   | `af-ZA`                                     |          |   ✓   |    ✓   |            |
| Arabic      | `ar-SA`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Armenian    | `hy-AM`                                     |          |   ✓   |        |            |
| Azerbaijani | `az-AZ`                                     |          |   ✓   |    ✓   |            |
| Bosnian     | `bs-BA`                                     |          |   ✓   |    ✓   |            |
| Bulgarian   | `bg-BG`                                     |     ✓    |   ✓   |    ✓   |            |
| Cantonese   | `yue-CN`                                    |     ✓    |   ✓   |    ✓   |            |
| Catalan     | `ca-ES`                                     |     ✓    |   ✓   |    ✓   |            |
| Chinese     | `zh-CN`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Croatian    | `hr-HR`                                     |          |   ✓   |    ✓   |            |
| Czech       | `cs-CZ`                                     |     ✓    |   ✓   |    ✓   |            |
| Danish      | `da-DK`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Dutch       | `nl-NL`, `nl-BE`                            |     ✓    |   ✓   |    ✓   |      ✓     |
| English     | `en-US`, `en-IN`, `en-GB`, `en-AU`, `en-NZ` |     ✓    |   ✓   |    ✓   |      ✓     |
| Filipino    | `fil-PH`                                    |          |   ✓   |        |            |
| Finnish     | `fi-FI`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| French      | `fr-FR`, `fr-CA`                            |     ✓    |   ✓   |    ✓   |      ✓     |
| Galician    | `gl-ES`                                     |          |   ✓   |    ✓   |            |
| German      | `de-DE`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Greek       | `el-GR`                                     |     ✓    |   ✓   |    ✓   |            |
| Hebrew      | `he-IL`                                     |          |   ✓   |    ✓   |      ✓     |
| Hindi       | `hi-IN`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Hungarian   | `hu-HU`                                     |     ✓    |   ✓   |    ✓   |            |
| Icelandic   | `is-IS`                                     |          |   ✓   |        |            |
| Indonesian  | `id-ID`                                     |     ✓    |   ✓   |    ✓   |            |
| Italian     | `it-IT`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Japanese    | `ja-JP`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Kannada     | `kn-IN`                                     |          |   ✓   |    ✓   |            |
| Kazakh      | `kk-KZ`                                     |          |   ✓   |    ✓   |            |
| Korean      | `ko-KR`                                     |     ✓    |   ✓   |    ✓   |            |
| Latvian     | `lv-LV`                                     |     ✓    |   ✓   |    ✓   |            |
| Lithuanian  | `lt-LT`                                     |     ✓    |   ✓   |    ✓   |            |
| Macedonian  | `mk-MK`                                     |          |   ✓   |    ✓   |            |
| Malay       | `ms-MY`                                     |     ✓    |   ✓   |    ✓   |            |
| Marathi     | `mr-IN`                                     |          |   ✓   |    ✓   |            |
| Nepali      | `ne-NP`                                     |          |   ✓   |        |            |
| Norwegian   | `no-NO`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Persian     | `fa-IR`                                     |          |   ✓   |    ✓   |            |
| Polish      | `pl-PL`                                     |     ✓    |   ✓   |    ✓   |            |
| Portuguese  | `pt-PT`, `pt-BR`                            |     ✓    |   ✓   |    ✓   |      ✓     |
| Romanian    | `ro-RO`                                     |     ✓    |   ✓   |    ✓   |            |
| Russian     | `ru-RU`                                     |     ✓    |   ✓   |    ✓   |            |
| Serbian     | `sr-RS`                                     |          |   ✓   |    ✓   |            |
| Slovak      | `sk-SK`                                     |     ✓    |   ✓   |    ✓   |            |
| Slovenian   | `sl-SI`                                     |          |   ✓   |    ✓   |            |
| Spanish     | `es-ES`, `es-419`                           |     ✓    |   ✓   |    ✓   |      ✓     |
| Swahili     | `sw-KE`                                     |          |   ✓   |    ✓   |            |
| Swedish     | `sv-SE`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Tamil       | `ta-IN`                                     |          |   ✓   |    ✓   |            |
| Thai        | `th-TH`                                     |     ✓    |   ✓   |    ✓   |            |
| Turkish     | `tr-TR`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Ukrainian   | `uk-UA`                                     |     ✓    |   ✓   |    ✓   |            |
| Urdu        | `ur-IN`                                     |          |   ✓   |    ✓   |            |
| Vietnamese  | `vi-VN`                                     |     ✓    |   ✓   |    ✓   |      ✓     |
| Welsh       | `cy-GB`                                     |          |   ✓   |    ✓   |            |

<Note>
  * **Azure** covers every supported language and acts as the broad-coverage fallback.
  * **AssemblyAI** supports code-switching across its listed languages.
  * **Deepgram** and **Soniox** support any-to-any code-switching for multilingual agents across their supported languages; Azure is single-language only.
</Note>

## Multilingual agents

For a multilingual agent, every selected language must be covered by a single voice and a single ASR provider that can handle the whole set together. If no single provider covers the combination, the dashboard blocks it. See [Configure a multilingual agent](/agent/multilingual) for accuracy trade-offs.
