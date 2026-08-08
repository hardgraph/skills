
## Cache build artifacts

Build artifacts can now be cached and reused between runs in the builder image. Caching is based on the build command and git commit:

```hocon
control-plane {
  repository {
    cache {
      enabled = true
      ttl = 7 days # (optional, default 7 days)
      cleanup-interval = 1 day # (optional, default: 1 day)
    }
  }
}
```
