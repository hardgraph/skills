
## Customize AWS resource name tag prefix

AWS locations now support a `name-tag-prefix` to customize the Name tag applied to EC2 instances and related resources:

```hocon
control-plane {
  locations = [
    {
      id = "<location-id>"
      type = "aws"
      name-tag-prefix = "<EXAMPLE_PREFIX_>"
    }
  ]
}
```
