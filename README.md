# Hardgraph Skills

[![skills.sh](https://skills.sh/b/hardgraph/skills)](https://skills.sh/hardgraph/skills)

Public, installable agent skills curated by Hardgraph.

## Repository status

The repository foundation is ready for skills.sh. The first public skill is intentionally not
invented by this scaffold: publication becomes active after an approved skill is added at
`skills/<slug>/SKILL.md` and `pnpm check:publishable` passes.

## Install

List the skills published by this repository:

```bash
npx skills add hardgraph/skills --list
```

Install one skill globally after selecting its slug:

```bash
npx skills add hardgraph/skills --skill <slug> --global --yes
```

## Author a skill

Create `skills/<slug>/SKILL.md` with the required frontmatter:

```markdown
---
name: example-skill
description: Describe what the skill does and the situations in which an agent should use it.
---

# Example skill

Give the agent direct, procedural instructions.
```

The directory and frontmatter name must be the same lowercase kebab-case slug. Supporting material
belongs beside the skill in `references/`, `scripts/`, or `assets/`.

## Verify

```bash
pnpm install --frozen-lockfile
pnpm check
pnpm check:publishable
```

See [the specification index](specs/README.mdx) and [contribution guide](CONTRIBUTING.md) for the
full contract.

## License

MIT
