
## Configure global SSH key credentials for Git authentication

SSH key credentials can now be configured globally for Git authentication in the builder image:

```hocon
control-plane {
  builder {
    git.global.credentials.ssh {
      key-file = "<keyFile>"
      user-known-hosts-file = "<userKnownHostsFile>" # (optional – omit this line to disable strict host checking)
    }
  }
}
```
