---
name: eve
description: Vercel eve — a filesystem-first framework for durable backend AI agents. Use when creating, editing, debugging, or operating an eve agent project: authoring instructions, defining typed tools, on-demand skills, message channels (Slack/Discord/HTTP), scheduled cron jobs, subagents, sandboxes, human-in-the-loop prompts, model/runtime config, and spend guards. Covers project layout, agent.ts, defineAgent/defineTool, extensions, and the bundled docs.
---

# eve

`eve` is a framework for **durable backend AI agents** whose authoring interface
is the filesystem. An agent is a directory on disk; instructions, tools, skills,
channels, subagents, and schedules are all ordinary files in conventional
locations. `eve` compiles and runs that directory, so a project is easy to
inspect, extend, and operate.

## Project layout — the filesystem is the interface

```text
my-agent/
└── agent/
    ├── agent.ts            # optional: model + runtime config (defineAgent)
    ├── instructions.md     # required: the always-on system prompt
    ├── tools/              # optional: typed functions the model can call
    │   └── get_weather.ts
    ├── skills/             # optional: procedures loaded on demand
    │   └── plan_a_trip.md
    ├── channels/           # optional: message channels (HTTP, Slack, Discord)
    │   └── slack.ts
    ├── subagents/          # optional: delegated sub-agents
    └── schedules/          # optional: recurring cron jobs
        └── weekly_recap.ts
```

Every capability lives in a predictable place; you rarely need to learn an
imperative API — you author files and `eve` discovers them.

## Quick start

```bash
npx eve@latest init my-agent      # scaffold a new agent
cd my-agent && npm run dev         # start the interactive terminal UI
```

Add eve to an existing project with `npx eve@latest init .`.

### A minimal agent

`agent/instructions.md`:

```md
You are a concise weather demo assistant.
```

`agent/tools/get_weather.ts`:

```ts
import { defineTool } from "eve/tools";
import { z } from "zod";

export default defineTool({
  description: "Return mock weather data for a city.",
  inputSchema: z.object({ city: z.string().min(1) }),
  async execute({ city }) {
    return { city, condition: "Sunny", temperatureF: 72 };
  },
});
```

`agent/agent.ts`:

```ts
import { defineAgent } from "eve";

export default defineAgent({
  model: "anthropic/claude-sonnet-5",
});
```

## Core primitives

| Primitive | Where | What it does |
| --- | --- | --- |
| **Agent** | `agent.ts` (`defineAgent`) | Model, runtime, and connection config. |
| **Instructions** | `instructions.md` | The always-on system prompt; required. |
| **Tools** | `tools/*.ts` (`defineTool`) | Typed functions with a Zod `inputSchema`. |
| **Skills** | `skills/*.md` | On-demand procedures the model loads when relevant. |
| **Channels** | `channels/*.ts` | Inbound/outbound messaging — HTTP, Slack, Discord. |
| **Subagents** | `subagents/*` | Delegated agents with scoped instructions/tools. |
| **Schedules** | `schedules/*.ts` | Recurring cron jobs the agent runs itself. |
| **Sandbox** | config | Code/tool execution isolation. |
| **Extensions** | project | Distributable capability packages layered on an agent. |

## Durable execution and human-in-the-loop

Agents are **durable**: long-running, resumable, and resilient to restarts. State
is checkpointed so a turn can pause for a human decision (an approval, a
clarification) and resume without losing context. Human-in-the-loop prompts are a
first-class tool shape, not an afterthought — see `docs/tools/human-in-the-loop`.

Spend is guarded: set budgets and limits so a runaway agent stops before it
overspends (`docs/tutorial/guard-the-spend`).

## What surprises people

- **The filesystem is the API.** Adding a capability usually means creating a file
  in the right directory, not calling a registration function. A tool placed in
  `tools/` is discovered automatically.
- **`instructions.md` is required and always-on.** It is the system prompt for
  every turn; skills, by contrast, are loaded on demand, so keep the always-on
  instructions lean and push conditional detail into skills.
- **The bundled docs are the version-accurate source of truth.** `eve` ships its
  full documentation inside the installed package at `node_modules/eve/docs/`.
  When working in a real project, read those — they match the installed version
  exactly — rather than relying on memory.
- **Durable state persists across restarts.** A channel turn or schedule that was
  paused mid-flight resumes from its checkpoint; do not assume a fresh process
  starts a fresh conversation.

## What to verify rather than recall

API signatures (`defineAgent`, `defineTool`), the supported model identifiers,
channel/schedule options, and the exact project-layout conventions change between
releases. Confirm them against the mirrored corpus under `references/vendor/` or,
in a real install, `node_modules/eve/docs/README.md` — which is the index and
recommended reading order — rather than asserting a remembered shape.

## References

- [eve documentation](https://eve.dev/docs)
- [eve repository](https://github.com/vercel/eve)
- [First-agent tutorial](https://eve.dev/docs/tutorial/first-agent)
- [Project structure](https://github.com/vercel/eve/blob/main/docs/project-structure.mdx)
