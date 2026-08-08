
## Limit concurrent builds

A new `max-concurrency` option controls how many builds from sources the Control Plane runs in parallel. Defaults to 1:

```hocon
control-plane {
  builder {
    max-concurrency = 2
  }
}
```
