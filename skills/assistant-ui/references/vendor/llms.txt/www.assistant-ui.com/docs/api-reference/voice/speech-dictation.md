# Speech and Dictation
URL: /docs/api-reference/voice/speech-dictation

Connect speech synthesis and dictation adapters to assistant-ui voice and composer workflows.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### DictationAdapter

- `listen`: `() => DictationAdapter.Session`
- `disableInputDuringDictation?`: `boolean`

### DictationState

- `status`: `DictationAdapter.Status`
  - `type`: `"starting" | "running"`
- `transcript?`: `string`
- `inputDisabled?`: `boolean`

### SpeechSynthesisAdapter

- `speak`: `(text: string) => SpeechSynthesisAdapter.Utterance`

### WebSpeechDictationAdapter

- `constructor?`: `(options: { language?: string; continuous?: boolean; interimResults?: boolean; } = {}) => WebSpeechDictationAdapter`
- `static isSupported?`: `() => boolean`
- `listen?`: `() => DictationAdapter.Session`

### WebSpeechSynthesisAdapter

- `speak?`: `(text: string) => SpeechSynthesisAdapter.Utterance`