# agent-browser — readme cover generation record

## Selected asset

- `readme-cover.png` — 1536 × 1024 PNG, 3:2 landscape.

## ImageGen use-case taxonomy

`stylized-concept` — a flat geometric illustration of an AI agent driving a real
browser via the snapshot-and-ref loop.

## Generation method

Per the user directive, image generation was first attempted through the `codex`
CLI directly:

```bash
cat agent-browser-cover.prompt.txt | codex exec -s workspace-write --skip-git-repo-check -C <repo> -
```

Codex was prompted to author and run a Pillow script. As observed in earlier
runs on this machine, the `codex` image-generation path stalled without writing
any file (interrupted after ~3.5 minutes of no output). The final asset was
therefore produced by the equivalent deterministic path that codex itself was
instructed to use: a hand-authored Pillow script
(`/private/tmp/hg-covers/generate_agent_browser_cover.py`) run directly, writing
the PNG to `/private/tmp/hg-covers/agent-browser-cover.png` and copied to the
canonical asset path. Verified 1536 × 1024.

## Final art-direction prompt

Compose a stylized-concept cover for the `agent-browser` developer tool. Theme:
an AI agent driving a real web browser through a snapshot-and-ref model. Left: an
abstract browser window (rounded frame, traffic-light dots, address bar) holding
a simplified page of stacked content bars and two buttons, with small labeled
"ref" chips (`@e1`, `@e2`) tagging specific elements (the active ref in amber).
Right: an abstract agent node (rounded robot icon with glow rings) sending a
dashed cyan arrow toward the browser to convey the automation loop
(snapshot → ref → act). Palette: indigo/navy gradient background (`#0f172a` →
`#1e293b`), electric cyan/teal accents (`#22d3ee`, `#2dd4bf`) for refs and the
connection, soft white UI bars, single amber accent (`#f59e0b`) for the active
ref chip. Flat geometric shapes, subtle shadows, generous negative space, crisp
vector feel, no photo detail, no logos, no watermarks. Subjects kept inside a
central safe area to survive responsive 3:2 cropping. No generated explanatory
text inside the image beyond the tiny ref labels.
