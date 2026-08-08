
## Server Configuration Improvements

* Decoupled the server configuration from the repository configuration.
  * The server configuration has been moved from `control-plane.repository.server` to `control-plane.server`
  * The health check endpoint at `/info` is now always available, even if no repository is configured
