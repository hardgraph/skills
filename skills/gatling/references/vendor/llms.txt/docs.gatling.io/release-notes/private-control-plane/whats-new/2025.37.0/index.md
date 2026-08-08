
## Standardize the authorization header

The Control Plane now uses the standard `Authorization: Bearer <token>` header when communicating with Gatling Cloud. The proprietary `Control-Plane-Bearer` header is no longer sent.

See the [upgrade guide]({{< ref "/release-notes/private-control-plane/upgrading/to-2025.37.0" >}}) if you have custom network filtering on this header.
