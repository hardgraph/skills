
## Configure proxy credentials

Username/password authentication can now be configured on the HTTP proxy used for Control Plane to Gatling Cloud communication:

```hocon
control-plane {
  enterprise-cloud {
    proxy {
      http {
        url = "http://proxy.tld:1234"
        credentials {
          username = "<proxyUser>"
          password = "<proxyPassword>"
        }
      }
    }
  }
}
```
