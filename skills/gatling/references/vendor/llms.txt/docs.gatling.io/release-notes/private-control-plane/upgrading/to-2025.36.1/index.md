
## Convert JKS keystores to PKCS12

**Who is affected:** users who configured a proxy keystore in JKS format.

### What Changed

The Control Plane now validates keystore and truststore files at startup. Only PKCS12 and PEM formats are accepted. JKS is now explicitly rejected — it was previously accepted but caused load generators to fail silently, since they use curl which does not support JKS.

### Migration Guide

The Control Plane fails to start if a JKS file is detected. If your keystore is in JKS format, convert it to PKCS12 before upgrading.

Then update your configuration to point to the `.p12` file:

```hocon
control-plane {
  enterprise-cloud {
    proxy {
      keystore {
        path = "/path/to/keystore.p12"
        password = "<password>"
      }
    }
  }
}
```

If your keystore is already PKCS12, or you only configured a PEM truststore, no action is required.
