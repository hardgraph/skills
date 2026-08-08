# Hardgraph Skills Agent Contract

## Purpose

This public repository is the canonical source for installable Hardgraph agent skills. The private
`hardgraph/core` repository may consume this repository, but it must not privately author a
competing copy of a public skill.

## Authority

- `specs/README.mdx` is the specification index and source-of-truth map.
- `specs/repository.mdx` owns the repository layout and skill-authoring contract.
- `specs/publication.mdx` owns validation, skills.sh discovery, and publication evidence.
- Each `skills/<slug>/SKILL.md` owns that skill's public instructions.

## Skill boundaries

- Put every public skill at `skills/<slug>/SKILL.md`.
- Use a lowercase kebab-case slug and make the frontmatter `name` exactly match the directory.
- Give every skill a specific `description` that states both what it does and when to use it.
- Keep the skill body procedural. Put detailed or optional material in that skill's `references/`,
  `scripts/`, or `assets/` directory.
- Do not add placeholder, compatibility, forwarding, or duplicate skills.
- Do not copy generated or crawler-owned material from `hardgraph/core` into this repository.

## Commands

```bash
pnpm install --frozen-lockfile
pnpm check
pnpm check:publishable
```

`pnpm check` validates the repository foundation. `pnpm check:publishable` additionally requires
at least one public skill and verifies that the official `skills` CLI discovers it.

## Safety

- Never commit secrets, credentials, private source material, or local environment files.
- Review scripts and linked resources as public executable or instructional supply-chain content.
- Preserve unrelated and concurrent changes. Stage only files owned by the current task.
