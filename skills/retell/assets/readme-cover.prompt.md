# retell — readme-cover

- **Asset**: `readme-cover.png` (1536 × 1024, 3:2, PNG, RGB)
- **Selected asset**: `./readme-cover.png` (this directory)
- **Generated with**: `codex exec` CLI (per the user's image-generation instruction for this batch),
  driving a Python/Pillow generator. NOT the built-in `imagegen` skill.
- **Tool mode**: codex non-interactive (`codex exec -s workspace-write --skip-git-repo-check`);
  codex authored `/private/tmp/hg-covers/generate_retell_cover.py`, rendered a validated
  1536×1024 PNG, and asserted size/format/aspect. The cover was placed at the canonical path by
  running the generated script outside the sandbox.
- **Prompt source**: `/tmp/hg-covers/retell.txt` (final selected prompt, verbatim).

## Composition

A realtime conversational voice loop. A dark indigo-to-near-black vertical gradient field. Center:
a horizontal sound waveform of ~28 symmetric vertical rounded bars (taller in the middle, shorter at
the ends) — the listening/speaking voice signal. Left of the waveform: an outlined speech bubble
(the caller's input). Right of the waveform: a smaller speech bubble holding three mint filler bars
(the agent's synthesized reply). Two thin arrows connect left-bubble → waveform → right-bubble,
showing the realtime flow. Behind everything, a faint thin circular ring at low opacity — the
sub-second realtime timing motif.

## Palette

- Field: indigo `#1b1640` → near-black `#08060f`
- Waveform bars: violet `#a78bfa`
- Speech-bubble outlines: soft white `#e8e6ff` (~85% opacity)
- Reply filler bars: mint `#6ee7b7`
- Flow arrows: `#c4b5fd`
- Timing ring: `#8b5cf6` (~25% opacity)

## Acceptance notes

- 3:2 landscape at 1536 × 1024; subjects held within the central ~70% safe area.
- No watermark, no provider marks, no generated text.
- Distinct from every other hardgraph skill cover (caller-bubble → voice-waveform → reply-bubble
  with a timing ring is unique to `retell`).
