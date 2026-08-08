# Push-protection redactions in vendored docs

- **Observed against**: Apify docs mirror, 2026-08-08.
- **What happens**: GitHub Push Protection blocks publishing this skill because
  three vendored pages contain byte sequences its secret scanner flags.

## The flagged strings

Two distinct kinds, both inside `references/vendor/llms.txt/docs.apify.com/`:

1. **Webpack asset-hash fragments misread as Mailgun API keys.** Apify's docs
   embed screenshots with content-hashed filenames of the form
   `*_api_key-<32-hex>.png` and `*-api-key-<32-hex>.png`. The `key-<32-hex>`
   substring matches the Mailgun key pattern. These are image-file hashes, not
   credentials (the hex values are deliberately not reproduced here). Found in
   `integrations/bubble.md` and `integrations/workato.md`.
2. **An example proxy password.** `proxy.md` shows a sample external connection
   string of the form `http://auto:apify_proxy_<example-token>@proxy.apify.com:8000`.
   The token is a documentation example, but it matches the Apify Proxy Password
   format exactly (the literal value is deliberately not reproduced here).

## What to do instead

The four flagged substrings were replaced with placeholders
(`redactedassethash0N` for the asset hashes, `apify_proxy_EXAMPLE_REDACTED` for
the proxy password) so the public mirror can publish. The rest of each file is
byte-exact upstream.

- A **re-crawl restores the originals and will re-trip Push Protection.** Re-apply
  these four substitutions after any re-crawl of this skill, or configure a
  redaction layer in the crawler.
- Treat any real Apify proxy password or API token as a live secret even if it
  appears in the docs; the example here is not one, but the format is the format.

## Why this is not a vendor-integrity violation

`vendor/` is byte-exact upstream by contract, but Push Protection is a hard
publish gate and the flagged strings are example/false-positive values, not
factual documentation content. Surgical redaction of only those substrings is the
smallest change that publishes the corpus without publishing a scanner-flagged
secret to a public repository. It is recorded here so the divergence from
upstream is visible and reproducible.
