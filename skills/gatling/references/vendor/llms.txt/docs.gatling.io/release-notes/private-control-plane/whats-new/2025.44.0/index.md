
## Support Kubernetes multi-cluster

A `context` field can now be set on Kubernetes locations to target a specific Kubernetes context. This enables a single Control Plane to manage load generators across multiple Kubernetes clusters:

```hocon
control-plane {
  locations = [
    {
      id = "<loc-cluster-a>"
      type = "kubernetes"
      context = "<cluster-a-context>" # Optional
    },
    {
      id = "<loc-cluster-b>"
      type = "kubernetes"
      context = "<cluster-b-context>" # Optional
    }
  ]
}
```

When omitted, the default Kubernetes context is used (unchanged behavior).
