# composio — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: `codex exec` CLI (per the user's image-generation instruction for this batch), driving a Python/Pillow generator. NOT the built-in `imagegen` skill.
- **Tool mode**: codex non-interactive (`codex exec -s workspace-write --skip-git-repo-check`), model `gpt-5.6-sol`; codex authored and ran a Pillow script that rasterizes the composition below, then verified the output is a 1536×1024 PNG.
- **Prompt source**: `/tmp/hg-covers/composio.txt` (final selected prompt, verbatim).

## Composition

An AI agent pulling tools through a managed-auth gateway. A dark violet-to-near-black gradient field. Center: a bright rounded-square node (the agent). A horizontal translucent band passes in front of it (the managed-auth layer). To the right of the band, a fan of five small distinct tool glyphs fanning outward and upward: an envelope, a calendar grid, code brackets, a gear, and a cloud. Thin connector lines run from the central node, through the band, to each tool glyph, with small key/lock dots on the connectors.

## Palette

- Field: violet `#140a24` → near-black `#07030f`
- Primary: violet `#8b5cf6` and magenta `#c026d3`
- Connector/key accent: warm amber `#f4b942`
- Tool glyphs: light `#e9d5ff`

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subject held within the central ~70% safe area.
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover (agent→auth-band→tool-fan motif is unique to `composio`).
