
## Configure global HTTPS credentials for Git authentication

HTTPS credentials can now be configured globally for Git authentication in the builder image:

```hocon
control-plane {
  builder {
    git.global.credentials.https {
      username = "<username>" # (optional)
      password = "<token>"
    }
  }
}
```
