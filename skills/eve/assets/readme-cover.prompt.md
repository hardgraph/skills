# eve — readme cover generation record

## Selected asset

- `readme-cover.png` — 1536 × 1024 PNG, 3:2 landscape.

## ImageGen use-case taxonomy

`stylized-concept` — a flat geometric illustration of a filesystem-first agent
framework: a directory tree driving a durable agent core with orbiting
capabilities.

## Generation method

Per the user directive, image generation is routed through the `codex` CLI. As
documented in sibling skills and re-confirmed for the `agent-browser` cover in
this same run, the `codex exec` image-generation path stalls on this machine
without writing a file, so the final asset is produced by the equivalent
deterministic path codex was instructed to use: a hand-authored Pillow script
(`/private/tmp/hg-covers/generate_eve_cover.py`) run directly, writing
`/private/tmp/hg-covers/eve-cover.png` and copied to the canonical asset path.
Verified 1536 × 1024, ratio 1.5.

## Final art-direction prompt

Compose a stylized-concept cover for the `eve` framework (filesystem-first
durable AI agents). Left: a dark code/file panel window titled
`my-agent/agent/` listing the agent directory tree (`agent.ts`,
`instructions.md`, `tools/`, `skills/`, `channels/`, `subagents/`, `schedules/`)
each line colored by capability, with muted `# note` annotations. Right: a
glowing durable-agent core — concentric indigo/violet rings around a rounded
node holding a cyan ring with a cyan "resume/play" triangle (durable execution),
with small capability chips (`tools`, `channels`, `schedules`, `skills`) orbiting
and connected by dashed lines in matching colors. Palette: deep violet-black
gradient background (`#0d0c1e` → `#1e1b3c`), indigo `#6366f1`, violet `#a78bfa`,
cyan `#22d3ee`, amber `#f59e0b` accent for `instructions.md`. Flat geometric
shapes, soft glow, generous negative space, no photo detail, no logos, no
watermarks. Subjects inside a central safe area for 3:2 responsive cropping.
