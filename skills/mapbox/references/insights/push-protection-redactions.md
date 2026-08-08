# Push-protection redactions in vendored docs

- **Observed against**: Mapbox docs mirror, 2026-08-08.
- **What happens**: GitHub Push Protection can block publishing this skill
  because vendored Mapbox API pages embed example access tokens that match the
  Mapbox token format.

## The flagged strings

Mapbox documentation illustrates every API call with an **example public access
token** of the form `pk.eyJ...` (a JWT). Several distinct example tokens appear
across the mirrored pages (notably `api/accounts/tokens.md` and
`api/maps/static-images.md`, where one repeats on nearly every example). These
are Mapbox's own public demo tokens, not live secrets — but they match the
Mapbox token scanner pattern exactly.

## What to do instead

Each example token's value was replaced with a placeholder
(`pk.EXAMPLE_REDACTED` / `pk.EXAMPLE_PUBLIC_TOKEN`) so the public mirror can
publish. The `pk.` prefix is preserved so the examples still read as Mapbox
public tokens; the JWT payload is dropped. The rest of each file is byte-exact
upstream.

- A **re-crawl restores the original example tokens and may re-trip Push
  Protection.** Re-apply the redaction (replace any `(pk|sk|ck)\.eyJ…` JWT run
  with a placeholder) after any re-crawl of this skill.
- Treat any `sk.*` (secret) Mapbox token as a live secret. None appear in this
  mirror; only `pk.*` example public tokens did.

## Why this is not a vendor-integrity violation

`vendor/` is byte-exact upstream by contract, but Push Protection is a hard
publish gate and the flagged strings are example/demo token values, not factual
documentation content. Surgical redaction of only the token payloads is the
smallest change that publishes the corpus without publishing a scanner-flagged
token to a public repository. It is recorded here so the divergence from
upstream is visible and reproducible.
