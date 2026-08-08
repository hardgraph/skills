
{{< alert tip >}}
Gatling 3.12 is dropping the Akka library and the Graphite integration.
{{< /alert >}}

## Revamping and adding SDK components to stop an ongoing test

We have changed `Injector` to `LoadGenerator`. Use the following table as a reference for updating your scripts for
3.12 compatibility. 

| Old name         | New name              |
|------------------|-----------------------|
| `stopInjector`   | `stopLoadGenerator`   |
| `stopInjectorIf` | `stopLoadGeneratorIf` |
