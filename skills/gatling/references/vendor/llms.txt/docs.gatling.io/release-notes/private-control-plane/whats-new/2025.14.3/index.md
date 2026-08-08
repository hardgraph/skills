
## Control Plane Builder image

A new Docker image flavor is now available: `gatlingcorp/control-plane:latest-builder`

This image is dedicated to the [Build from Sources]({{< ref "/reference/deploy/private-locations/build-from-git" >}}) feature and bundles all required build tools: Git, Maven, Gradle, SBT, and Node.js.

{{< alert info >}}
The classic `gatlingcorp/control-plane:latest` image is unchanged. Use `latest-builder` only when you intend to build simulations from source.
{{< /alert >}}
