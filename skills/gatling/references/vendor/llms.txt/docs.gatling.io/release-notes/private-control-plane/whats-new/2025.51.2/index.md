
## Set default system properties for all locations

A new `default` block allows setting system properties inherited by all locations, eliminating the need to repeat them per location. This is useful for configuring monitoring agents (Datadog, etc.) across all private locations:

```hocon
control-plane {
  default {
    locations {
      system-properties = {
        sysprop-example = "<default-property-value>"
      }
    }
  }
}
```

Per-location `system-properties` merge with and override defaults:

* **Merge rule:** defaults are merged into each location's configuration.
* **Conflict rule:** location-specific keys take precedence in case of duplicate entries (no override from defaults).

## Fix builder packager temporary files

Fixed packager temporary files not being created inside the builder's working directory, which caused permission errors in certain configurations.
