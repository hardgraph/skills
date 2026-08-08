> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Add custom pronunciation

> Control how Retell AI agents pronounce words with IPA, CMU, Mandarin Pinyin, or Cantonese Jyutping based on the selected voice provider and model.

You can control how a Retell agent pronounces specific words or phrases. Choose IPA, CMU, Mandarin Pinyin, or Cantonese Jyutping when the selected voice model supports it.

To use the feature, you set a pronunciation dictionary on the agent. The dictionary is a list of entries, and each entry has:

* **Word or phrase** to be annotated. For example, `actually`.
* **Phonetic alphabet** supported by the selected [voice](#voice-support).
* **Phoneme** with the pronunciation in the selected alphabet.

For example:

| Alphabet           | API value  | Word       | Phoneme              |
| ------------------ | ---------- | ---------- | -------------------- |
| IPA                | `ipa`      | `actually` | `æktʃuəli`           |
| CMU (ARPAbet)      | `cmu`      | `actually` | `AE K CH UW AH L IY` |
| Mandarin Pinyin    | `pinyin`   | `燕少飞`      | `yan4 shao3 fei1`    |
| Cantonese Jyutping | `jyutping` | `你好`       | `nei5 hou2`          |

Use tone numbers `1–5` for Pinyin and `1–6` for Jyutping. Separate syllables with spaces.

## Voice support

Which phonetic alphabets you can use depends on the selected voice provider and its effective voice model. The dashboard shows the supported alphabets under **Pronunciation** in the agent's speech settings. See [supported languages by provider](/build/language-support#model-level-restrictions) for voice-model language restrictions.

| Voice                                                                     | Supported alphabets       |
| ------------------------------------------------------------------------- | ------------------------- |
| Retell Platform voices                                                    | IPA                       |
| Cartesia voices                                                           | IPA                       |
| MiniMax `speech-2.8-turbo`                                                | IPA, Pinyin, and Jyutping |
| MiniMax `speech-02-turbo`                                                 | IPA and Pinyin            |
| ElevenLabs `eleven_flash_v2` (English only)                               | IPA and CMU               |
| ElevenLabs `eleven_flash_v2_5`, `eleven_multilingual_v2`, and `eleven_v3` | Not supported             |
| OpenAI and Fish Audio voices                                              | Not supported             |

If an ElevenLabs voice uses **Auto**, an English-only agent resolves to Flash v2 and supports IPA and CMU. Agents that use other languages resolve to a newer ElevenLabs model that does not support pronunciation dictionaries. The dashboard displays the resulting support.

If the selected voice or model doesn't support an entry's alphabet, Retell preserves the entry but doesn't apply it.

## How many entries can I add?

The pronunciation dictionary is a list on the agent — you can add multiple entries and there is no fixed cap enforced by the API. In practice:

* Add one entry per word or short phrase you want to override. Each entry needs its own `word`, `alphabet`, and `phoneme`.
* Keep the dictionary focused on words the model actually mispronounces (uncommon proper nouns, brand names, technical terms, foreign words). Overriding common words can produce unnatural speech.
* Use a unique `word` value for each entry. The API rejects updates that contain duplicate `word` values.
* Very large dictionaries can push the agent payload close to request size limits when calling `update-agent`. If you hit that, split entries across agents or trim rarely used words.

## Finding the phonetic pronunciation

You can search online to find the phonetic pronunciation of a word, or use tools like:

* [IPA pronouncing dictionary tool](https://tophonetics.com/)
* [CMU pronouncing dictionary tool](http://www.speech.cs.cmu.edu/cgi-bin/cmudict/)
