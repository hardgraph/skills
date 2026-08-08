# knip — readme cover generation record

## Selected asset

- `readme-cover.png` — 1536 × 1024 PNG, 3:2 landscape.

## ImageGen use-case taxonomy

`stylized-concept` — a flat geometric illustration of knip cutting unused code:
CLI findings being resolved and a dependency node being cut from a graph.

## Generation method

Per the user directive, image generation is routed through the `codex` CLI. As
re-confirmed for the `agent-browser` cover earlier in this run, the `codex exec`
image-generation path stalls on this machine without writing a file, so the final
asset is produced by the equivalent deterministic path codex was instructed to
use: a hand-authored Pillow script (`/private/tmp/hg-covers/generate_knip_cover.py`)
run directly, writing `/private/tmp/hg-covers/knip-cover.png` and copied to the
canonical asset path. Verified 1536 × 1024, ratio 1.5.

## Final art-direction prompt

Compose a stylized-concept cover for `knip` ("to cut"), on a light
warm-neutral background. Left: a dark rounded terminal window titled `knip`
running `$ npx knip`, listing three findings (unused exports, unused
dependencies, unused files) with red alert markers; one row struck through with a
green check to show it was auto-fixed, and a summary line "3 issues (1
auto-fixed)". Right: a small dependency graph with a filled orange `app` hub node
connected to white `modA`/`modB` nodes and a red-outlined `dep` node, where the
edge to `dep` is a dashed red "cut" line and a pair of orange scissor blades sits
on that edge mid-cut. Brand orange accent `#f56e0f`, red `#e14646` for flagged
unused, green `#28a05a` for fixed. Flat geometric shapes, crisp vector feel,
generous negative space, no photo detail, no logos, no watermarks. Subjects
inside a central safe area for 3:2 responsive cropping.
