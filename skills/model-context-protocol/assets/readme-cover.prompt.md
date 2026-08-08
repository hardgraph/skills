# model-context-protocol — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: `codex exec` CLI (per the user's image-generation instruction for this batch), driving a Python/Pillow generator. NOT the built-in `imagegen` skill.
- **Tool mode**: codex non-interactive (`codex exec -s workspace-write --skip-git-repo-check`), model `gpt-5.6-sol`; codex authored and ran a Pillow script that rasterizes the composition below, then verified the output is a 1536×1024 PNG.
- **Prompt source**: `/tmp/hg-covers/mcp.txt` (final selected prompt, verbatim).

## Composition

A literal "host app ↔ server ↔ model" handshake. A dark indigo-to-near-black gradient field. Three glowing nodes left-to-right: a rounded-rectangle (host/client), a larger brighter hexagonal hub (server), and a rounded-rectangle (LLM). Two gentle handshake arcs connect them, each carrying small bright packet-dots traveling along the arc to suggest JSON-RPC messages. Above the central hub, three small distinct glyphs: a wrench (tool), a document/page (resource), and a speech bubble (prompt). A soft radial glow sits behind the hub.

## Palette

- Field: indigo `#0b1026` → near-black `#05060f`
- Nodes/arcs: cyan `#5cc8ff` with indigo `#7b6cff` edges
- Capability-glyph accent: amber `#f4b942`

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subject held within the central ~70% safe area.
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover (host↔hub↔model handshake-with-packet-dots motif is unique to `model-context-protocol`).
