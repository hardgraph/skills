---
name: ollama
description: Ollama — run open large language models locally or through Ollama Cloud, via a CLI and an OpenAI-compatible REST API. Use when running models like Gemma, Qwen, GLM, Kimi, or Llama on your own hardware, building a Modelfile, calling the /api/generate and /api/chat endpoints, using Ollama as a drop-in OpenAI replacement at /v1, pointing a library at OLLAMA_HOST, or choosing between a local server and Ollama Cloud. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Ollama
  category: AI and agents
  tags: [ollama, local-models, llm, openai-compatible, inference]
---

# Ollama

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Ollama runs open-weight large language models — Gemma, Qwen, GLM, Kimi, Llama,
and thousands more — behind one CLI and one REST API. The same model can run on
your own hardware (a local `ollama` server) or on **Ollama Cloud**; the only
thing that changes between the two is the base URL.

The surface is intentionally small: a CLI for people, a JSON REST API for
programs, and an OpenAI-compatible shim so anything that speaks OpenAI can speak
Ollama. There is no cluster to operate and no separate model server to wire up.

## Mental model

| Concept          | What it is                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------- |
| **Model**        | A name plus optional `:tag`. `gemma3` is a tag shorthand for the default tag; `gemma4:27b` pins a size. |
| **Local server** | A background process (`ollama serve`, auto-started by the CLI) serving the API at `http://localhost:11434`. |
| **Cloud**        | The same API hosted at `https://ollama.com`. A model pulled with the `:cloud` tag runs there instead of locally. |
| **Modelfile**    | A recipe for a custom model: a `FROM` base, a system prompt, parameters, and chat templates. Built with `ollama create`. |

`ollama run` and `ollama pull` both **pull on demand**: the first use of a model
downloads its weights into the local library. Subsequent runs are local and
offline-capable.

## Local vs Ollama Cloud

This is the first decision, and the docs treat the two as the same API with a
different host.

- **Local** — models run on your machine. Base URL `http://localhost:11434/api`.
  No account, no network after the pull. Bounded by your GPU/CPU and RAM.
- **Cloud** — a model pulled or run with a `:cloud` tag runs on Ollama's
  infrastructure. Base URL `https://ollama.com/api`. Requires an account; billed
  per Free / Pro / Max plan.

The `:cloud` tag is how Ollama routes a request to the cloud instead of the
local server. `ollama run gemma4` runs locally; `ollama run gemma4:cloud` runs
on Ollama Cloud.

## The CLI

```bash
ollama                  # interactive menu: run a model, launch an integration
ollama run gemma4       # download (if needed) then chat interactively
ollama run gemma4:cloud # same model, served by Ollama Cloud
ollama pull llama3.1    # download without running
ollama list             # show installed models
ollama rm llama3.1      # delete a model
ollama show gemma4      # inspect a model's modelfile, parameters, template
```

Leave an interactive chat with `/bye`. The CLI auto-starts the local server if
it is not already running, so a fresh install needs no `serve` step.

## The REST API

The API is JSON over HTTP, streams responses by default, and is not strictly
versioned — the docs commit to backwards compatibility and announce rare
deprecations in release notes.

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "gemma4",
  "prompt": "Why is the sky blue?"
}'
```

The core endpoints, all under `/api`:

| Endpoint            | Purpose                                                        |
| ------------------- | ------------------------------------------------------------- |
| `/api/generate`     | One-shot text generation from a prompt.                       |
| `/api/chat`         | Multi-turn chat over a `messages` array.                      |
| `/api/embeddings`   | Generate vector embeddings for input text.                    |
| `/api/tags`         | List locally installed models.                                |
| `/api/show`         | Return a model's modelfile, parameters, and template.         |
| `/api/pull` / `/api/delete` | Manage the local model library over the API.          |

Both `/api/generate` and `/api/chat` stream a JSON object per token unless
`"stream": false` is set, in which case they return a single object once the
response is complete.

## OpenAI compatibility

Ollama exposes an OpenAI-compatible API at `/v1`, so a client built for OpenAI
works by changing only the base URL.

```bash
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemma4",
    "messages": [{"role": "user", "content": "Why is the sky blue?"}]
  }'
```

Use this when integrating with a tool, library, or framework that already speaks
OpenAI and you do not want to rewrite it for `/api`. The `/v1` shim and the
native `/api` surface serve the same models; pick one per integration and stay
consistent.

## Building a custom model (Modelfile)

A Modelfile assembles a custom model from a base model, a system prompt,
inference parameters, and a chat template.

```text
FROM gemma4

PARAMETER temperature 0.3
PARAMETER num_ctx 8192

SYSTEM """You are a precise senior engineer. Answer in the shortest correct form."""
```

```bash
ollama create my-engineer -f Modelfile
ollama run my-engineer
```

`num_ctx` sets the context window the model will reserve; raising it raises peak
memory. `temperature` and the other parameters mirror the values Ollama accepts
in the API `options` object.

## Pointing clients at a non-default host

`OLLAMA_HOST` selects where the local server listens and where the CLI and
libraries connect. Set it when the server runs on another machine or port.

```bash
OLLAMA_HOST=0.0.0.0:11434 ollama serve   # listen on all interfaces
OLLAMA_HOST=192.168.1.10:11434 ollama run gemma4
```

Binding to `0.0.0.0` exposes the API on your network; do it deliberately and
behind a firewall — the local server has no built-in authentication.

## Official libraries

Ollama ships first-party [Python](https://github.com/ollama/ollama-python) and
[JavaScript](https://github.com/ollama/ollama-js) clients that wrap `/api`. They
take a `host` option that accepts the local URL or the cloud URL, so the same
code targets either.

## Current vs deprecated

- Prefer the **REST API** and **CLI** over scraping the model library pages. The
  API is the stable, documented surface; the website changes for marketing.
- The API is not versioned but is treated as stable and backwards-compatible;
  deprecations are announced in the
  [release notes](https://github.com/ollama/ollama/releases), so resolve the
  current behaviour from there rather than from memory.
- `ollama` (no argument) opens the interactive menu on current installs; older
  versions printed help. Use `ollama run` for scripted use, never the menu.

## References

- [Ollama quickstart](https://docs.ollama.com/quickstart)
- [API reference](https://docs.ollama.com/api/introduction)
- [OpenAI compatibility](https://docs.ollama.com/openai)
- [Model library](https://ollama.com/library)
- [Ollama on GitHub](https://github.com/ollama/ollama)
