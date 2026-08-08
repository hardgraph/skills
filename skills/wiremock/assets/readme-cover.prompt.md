# wiremock — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: `codex exec` CLI (per the user's image-generation instruction for this batch), driving a Python/Pillow generator. NOT the built-in `imagegen` skill.
- **Tool mode**: codex non-interactive (`codex exec -s workspace-write --skip-git-repo-check`), model `gpt-5.6-sol`; codex authored and ran a Pillow script that rasterizes the composition below, then verified the output is a 1536×1024 PNG.
- **Prompt source**: `/tmp/hg-covers/wiremock.txt` (final selected prompt, verbatim).

## Composition

A request hitting a stub that returns a canned response. A dark teal-green-to-near-black gradient field. Left: a rounded-rectangle node (client) with a bold rightward arrow. Center: a larger hexagonal hub (the stub/mock server). Right: a response-card rectangle with three horizontal filler bars, returned via a leftward arrow from the hub. Behind everything, a faint tape-reel motif: two circles with thin radial spokes and small sprocket dots around the rim (record/playback).

## Palette

- Field: teal-green `#0c1a14` → near-black `#050a07`
- Hub and arrows: amber `#f4b942`
- Response card and connectors: green `#34d399`
- Left node: soft `#a7f3d0`

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subject held within the central ~70% safe area.
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover (client→stub-hub→response-card with tape-reel motif is unique to `wiremock`).
