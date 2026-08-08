# revenuecat — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: `codex exec` CLI (per the user's image-generation instruction for this batch),
  driving a Python/Pillow generator. NOT the built-in `imagegen` skill.
- **Tool mode**: codex non-interactive (`codex exec -s workspace-write --skip-git-repo-check`);
  codex authored `/private/tmp/hg-covers/generate_revenuecat_cover.py`, rendered a validated
  1536×1024 PNG, and asserted size/format/aspect. The cover was placed at the canonical path by
  running the generated script outside the sandbox.
- **Prompt source**: `/tmp/hg-covers/revenuecat.txt` (final selected prompt, verbatim).

## Composition

A purchase unlocking an entitlement. A dark emerald-green-to-near-black vertical gradient field.
Center: a rounded-rectangle "paywall" card containing three stacked plan-tier rows; the top tier is
filled bright (the selected plan) while the lower two are dim outlines. To the right of the card, an
unlocked padlock (rectangular body with an open shackle arc above) drawn from primitives — the
granted entitlement. A single thin arrow runs from the highlighted tier to the open padlock. Behind
the card, a faint circular-arrow ring with a small gap and arrowhead at low opacity — the recurring
subscription renewal cycle.

## Palette

- Field: emerald `#0c241a` → near-black `#050a07`
- Paywall card outline: soft green `#a7f3d0` (~75% opacity)
- Highlighted tier (filled): emerald `#34d399`; dim tiers: `#1f6f54`
- Open padlock + unlock arrow: gold `#f4b942`
- Renewal ring: `#34d399` (~25% opacity)

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subjects held within the central ~70% safe area.
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover (paywall-card with a highlighted tier unlocking an
  open padlock beside a renewal ring is unique to `revenuecat`).
