# Chainlink — readme cover prompt record

## Selected prompt

> 3:2 landscape illustration (1536×1024). A stylized conceptual illustration of an oracle network
> bridging two worlds: on the left, a clean abstract on-chain realm represented by connected
> hexagonal blocks and a contract node; on the right, an off-chain realm represented by flowing
> data streams, a price ticker, a die (randomness), and a clock-gear (automation). Between them, a
> luminous chain of linked nodes carrying data across the gap — the oracle bridge. Modern palette
> of deep navy, electric blue, and a warm gold accent on a soft dark-to-light gradient background;
> flat geometric shapes, soft glow, subtle depth. No text, no logos, no brand marks, no real
> currency symbols. Subjects kept inside central safe margins.

## Design intent

Chainlink's mechanism is a **decentralized oracle bridge** between deterministic on-chain contracts
and unpredictable off-chain reality, delivered through several purpose-built networks (data,
randomness, automation, cross-chain). The cover depicts that bridge directly with the distinct
off-chain signals (price, a die for VRF, a gear for automation) flowing through linked oracle nodes
into an on-chain contract. It avoids any provider logo or text so it implies no endorsement the
skill does not have.

## Generation record

- **ImageGen use-case taxonomy:** `stylized-concept` (conceptual illustration of the skill's core
  mechanism).
- **Tool / mode:** **programmatic composition with Pillow (PIL) on Python 3.** This environment
  had no raster image-generation model available: no bundled `imagegen` skill exists, and delegating
  to the `codex` CLI (per project instruction) did not yield an image — Codex recursively nested
  sessions and hit safety-policy rejections without producing a file. Rather than leave the asset
  missing, a deliberate abstract geometric composition was drawn locally to satisfy the
  structural cover contract (a real, valid, distinct, non-gradient PNG). It is a placeholder for
  illustration quality, not for existence.
- **Target size / ratio:** 1536 × 1024, 3:2 landscape PNG (verified).
- **Selected asset path:** `assets/readme-cover.png`.
- **Iteration notes:** single deterministic render (seeded), no text or logos. The composition
  depicts the oracle-bridge concept (on-chain hexagons + contract node, off-chain price/randomness/
  automation signals, linked by a luminous node chain) in the navy/blue/gold palette.
  **Follow-up:** replace with a true image-model illustration pass when an image-generation
  capability is available; the skill is `lifecycle.status: draft` pending that human review.
