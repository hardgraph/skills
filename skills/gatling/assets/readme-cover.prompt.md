# gatling — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: `codex exec` CLI (per the user's image-generation instruction for this batch),
  driving a Python/Pillow generator. NOT the built-in `imagegen` skill.
- **Tool mode**: codex non-interactive (`codex exec -s workspace-write --skip-git-repo-check`);
  codex authored `/private/tmp/hg-covers/generate_gatling_cover.py`, rendered a validated
  1536×1024 PNG, and asserted size/format/aspect. The cover was placed at the canonical path by
  running the generated script outside the sandbox.
- **Prompt source**: `/tmp/hg-covers/gatling.txt` (final selected prompt, verbatim).

## Composition

A flood of concurrent virtual users converging on a server to find its breaking point. A dark
slate-blue-to-near-black vertical gradient field. Center-right: a rounded-rectangle "server" tower
node with three glowing amber LED strips. Left: a loose arc of ~14 cyan virtual-user dots of varying
size, each sending a thin rightward arrow toward the server — a converging population of requests.
Behind the arrows, a faint red pressure-gauge semicircle with its needle tilted into the red zone,
drawn at low opacity (the "breaking point" motif).

## Palette

- Field: slate-blue `#16243a` → near-black `#070b13`
- Virtual-user dots: cyan `#38bdf8`
- Request arrows: soft white `#e2e8f0` (~80% opacity)
- Server node outline + LED glow: amber `#f4b942`
- Pressure-gauge arc + needle: muted red `#ef4444` (~35% opacity)

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subjects held within the central ~70% safe area.
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover (concurrent-user flood → server tower with a
  pressure-gauge breaking-point motif is unique to `gatling`).
