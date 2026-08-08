
## Deprecate `working-directory` in favor of `executable-dir`

The `working-directory` config key is deprecated and replaced by `executable-dir`, aligning dedicated machines with all other location types. If `executable-dir` is absent, the Control Plane falls back to `working-directory`.

See the [upgrade guide]({{< ref "/release-notes/private-control-plane/upgrading/to-2026.4.0" >}}).
