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

## Structure

`specs/skills.mdx` in `hardgraph/core` is the structural specification for a skill directory — every
allowed path, its owner, and how it is checked. Read it before creating a skill or reviewing a
change under `skills/`.

In short: `SKILL.md` and `metadata.yaml` are required; `references/insights/`,
`references/examples/`, and `assets/` are authored; `references/vendor/` is crawler-owned and must
never be hand-edited. Crawl sources are declared as data in `metadata.yaml`, so a skill carries no
`package.json`, no dependency, and no build step.

## Everything here is public

There is no private tier. `SKILL.md`, `metadata.yaml`, references, vendored material, and assets all
publish. Consequences that follow, and that are easy to forget:

- Vendored material is **redistribution**. Vendor only what the source's licence permits, and record
  those terms in `provenance.sources`. A trademark or a doc corpus under restrictive terms is not
  made publishable by having crawled it.
- Retrieval tuning, provenance, and crawl configuration are visible to anyone. Nothing in
  `metadata.yaml` may contain secrets, internal URLs, customer names, personas, private repository
  paths, or unpublished business data.
- Write guidance for another agent facing the same class of problem, not as a diary of the project
  that prompted it.

## Editorial authority

- AI and contributors may research, propose, generalize, and validate skill changes.
- Human approval of the change is the publication authority. Schema validity and a successful crawl
  are not approval.
- Feedback is evidence for a future edit, not a direct mutation of published content.
- Nothing autonomously promotes research or generated output into accepted guidance.
- Never invent dates, authors, licences, URLs, or evidence. An unknown field is omitted, not guessed.

## Change discipline

Authoring is deliberate and manual: one skill at a time. Do not use codemods, glob rewrites, batch
formatters, mass deletion, or any command that rewrites multiple skills. A tool that rewrites many
skills at once cannot be reviewed, and unreviewed guidance is the one thing this repository must not
ship.

- Preserve unrelated skill work. Do not refresh upstream content or rewrite generated inventory while
  editing authored guidance unless the task requires it.
- When authored content changes materially, update the matching provenance and lifecycle date in the
  same change.
- When a crawl changes upstream bytes, review both the corpus diff and the generated failure
  inventory before accepting it.

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
