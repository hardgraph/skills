# Suggestion Adapters
URL: /docs/api-reference/adapters/suggestions

Suggestion adapters for providing starter prompts, contextual actions, and guided composer options to assistant-ui runtimes.

> For AI agents: a documentation index is available at [llms.txt](/llms.txt). Use `.md` for canonical markdown pages; `.mdx` is kept as a backwards-compatible alias on supported URL paths.

## API Reference

### SuggestionAdapter

- `generate`: `( options: SuggestionAdapterGenerateOptions, ) => | Promise<readonly ThreadSuggestion[]> | AsyncGenerator<readonly ThreadSuggestion[], void>`