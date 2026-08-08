# ollama

![ollama cover](./assets/readme-cover.png)

Reference skill for [Ollama](https://ollama.com/) — run open large language
models locally or through Ollama Cloud, behind one CLI and one OpenAI-compatible
REST API. It steers an agent through the local-vs-cloud routing model, the CLI,
the `/api` surface, the `/v1` OpenAI shim, Modelfiles, and `OLLAMA_HOST`, without
relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill ollama
```

## Use this skill for

- Running models like Gemma, Qwen, GLM, Kimi, or Llama on your own hardware
- Calling the `/api/generate` and `/api/chat` endpoints, or their streaming modes
- Using Ollama as a drop-in OpenAI replacement at `/v1`
- Building a custom model with a Modelfile (`ollama create`)
- Choosing between a local `ollama` server and Ollama Cloud
- Pointing a client or first-party library at a non-default `OLLAMA_HOST`

## What is included

- [`SKILL.md`](./SKILL.md) — the agent procedure and the local-vs-cloud, native-vs-OpenAI decision criteria.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible
  verbatim mirror of the Ollama documentation the seed index links to, used for
  exact endpoint, CLI, and parameter details.

## Source

Reference material is reproduced from the
[Ollama documentation](https://docs.ollama.com) via its official
[llms.txt index](https://ollama.com/llms.txt).
