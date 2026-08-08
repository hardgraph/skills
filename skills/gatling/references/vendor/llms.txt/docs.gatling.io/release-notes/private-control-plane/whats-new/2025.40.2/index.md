
## Handle graceful shutdown

The Control Plane now handles SIGTERM gracefully. It stops accepting new tasks, waits for any in-progress build or deployment to complete, then exits cleanly. This enables safe rolling upgrades without interrupting active Build from Sources runs.
