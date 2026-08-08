# Convex Agent Skills

> For AI agents: see [llms.txt](/llms.txt) for the complete documentation index. Markdown versions are available by adding .md to a page URL or requesting Accept: text/markdown.

[Agent Skills](https://agentskills.io) are portable packages of instructions and workflows that teach AI coding agents how to perform specialized tasks. Convex provides a set of ready-made skills for common workflows like setting up auth, designing a schema, and running migrations.

## Install[​](#install "Direct link to Install")

Use the `npx skills` CLI to add the Convex skills to your project:

```
# Choose which skills to install

npx skills add get-convex/agent-skills



# Or install all of them at once

npx skills add get-convex/agent-skills --all
```

Skills are installed into `.agents/skills/` in your project and are automatically picked up by compatible agents including Cursor, Claude Code, and GitHub Copilot.

## Available Skills[​](#available-skills "Direct link to Available Skills")

| Skill                       | Description                                                            |
| --------------------------- | ---------------------------------------------------------------------- |
| `/convex`                   | Top-level entry point — routes to the right Convex skill for your task |
| `/convex-quickstart`        | Set up a new Convex project from scratch                               |
| `/convex-setup-auth`        | Configure authentication for your Convex app                           |
| `/convex-migration-helper`  | Plan and run data migrations                                           |
| `/convex-create-component`  | Create a new Convex component                                          |
| `/convex-performance-audit` | Audit and optimize Convex queries and mutations                        |

Skills are added and updated regularly, so this list may not be exhaustive. See the [get-convex/agent-skills](https://github.com/get-convex/agent-skills) repo for the latest.

## Using Skills[​](#using-skills "Direct link to Using Skills")

Skills are applied automatically when the agent determines they're relevant. How you manually invoke them depends on your tool:

| Tool                     | Manual invocation |
| ------------------------ | ----------------- |
| Cursor                   | `/skill-name`     |
| VS Code (GitHub Copilot) | `/skill-name`     |
| Claude Code              | `/skill-name`     |
| Codex (OpenAI)           | `$skill-name`     |

For example, to kick off auth setup in Cursor or Claude Code:

```
/convex-setup-auth
```

## Learn More[​](#learn-more "Direct link to Learn More")

* [get-convex/agent-skills](https://github.com/get-convex/agent-skills) - full list of skills, source code, and contributing guide
* [Agent Skills standard](https://agentskills.io) - the open standard these skills are built on
