# convex — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: Python/Pillow programmatic generator (`/tmp/gen_convex_cover.py`),
  the established in-session imagegen fallback used when no ImageGen tool is available.
  Codex CLI was prompted first per the user's image-generation directive (`codex exec`
  with the cover brief), but the run was API-latency-bound and produced no asset within
  the bounded wait, so the Pillow fallback generated the committed cover. No external
  image API or stock asset was used.
- **Tool mode**: programmatic raster generation (Pillow), `SMOOTH_MORE` finish, seeded RNG.

## Composition

A dark slate-blue field with a soft vertical gradient and a faint dotted lattice. A diffuse
indigo radial glow sits behind the focal point. Center-left: a rounded **core block**
representing the reactive database store, with an indigo header bar and three internal
data-row lines. From the right edge of the core, five thin connection lines bow outward to
five smaller rounded **client blocks** stacked vertically on the right; along each line three
**cyan pulses** travel from the store toward the client, expressing realtime PUSH — live
updates flowing from the central store to subscribers. An indigo node sits beneath the core.
No text.

## Palette

- Field: slate-blue `#0e1422` → near-black `#04060c`
- Accent (store / core): indigo `#7c5cff`
- Accent (pulses / client edges): cyan `#38dce6`
- Block fill: dark slate `#161e2e`

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subjects held within the central safe area (responsive
  GitHub rendering crops the edges).
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover: the reactive store-to-client push motif
  (indigo core fanning cyan pulses to client blocks) is unique to `convex`; other covers use
  stacked-server rack (coolify), node/hub/network, mosaic, handshake, and gateway motifs.
