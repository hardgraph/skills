---
name: gatling
description: Load and performance testing as code. Use when performance-testing an HTTP API or web service — writing a Gatling simulation in Java/Kotlin/Scala/JavaScript, modelling virtual users and ramp-up injection profiles, parametrising with feeders, asserting response criteria, running from Maven/Gradle/sbt or CI, reading HTML reports, or choosing Gatling against k6 or JMeter — across the open-source engine and Gatling Enterprise.
license: Apache-2.0
metadata:
  display-name: Gatling
  category: Testing
  tags: [load-testing, performance-testing, stress-testing, scala, java, simulation]
---

# Gatling

Gatling is a load- and performance-testing framework that treats tests **as code**. A test is a
**simulation**: a program that launches thousands of **virtual users**, each running a **scenario**
of HTTP requests against a target, while the engine records timings, success rates, and response
distributions. Because the test is code, it lives in version control, runs in CI, and reproduces
exactly.

## Mental model

A simulation wires together three things:

1. **The HTTP protocol** — base URL, headers, shared configuration, connection pooling, checks to
   apply to every response.
2. **The scenario** — the ordered steps a single virtual user performs: requests, pauses, loops,
   conditionals, feeders.
3. **The injection profile** — *how many* users run and *how fast* they start: ramp-ups, plateaus,
   spikes. This is the workload model.

```scala
class MySimulation extends Simulation {
  val httpProtocol = http.baseUrl("https://api.example.com")
  val scn = scenario("Buy")
    .exec(http("home").get("/"))
    .exec(http("checkout").post("/checkout"))

  setUp(scn.inject(rampUsers(100).during(60)))
    .protocols(httpProtocol)
}
```

The same simulation can be written in **Java, Kotlin, Scala, or JavaScript** — all compile to the
same engine. Pick the language that matches your team; the concepts are identical.

## Workload models: closed vs open

This distinction is the most consequential decision in a load test, and getting it wrong makes the
result meaningless.

- **Closed model** (`atOnceUsers`, `rampUsers`, `constantConcurrentUsers`) holds a fixed number of
  *concurrent* users. The system's response time throttles the request rate: if it slows down, the
  arrival rate drops. This matches a fixed pool of real users (call-center agents, logged-in
  sessions) but **hides saturation** — the test cannot push harder than the users can cycle.
- **Open model** (`rampUsersPerSec`, `constantUsersPerSec`) injects users at a fixed *arrival rate*
  regardless of response time. If the system slows, requests pile up. This is how the public
  internet behaves and is the only honest way to find a breaking point.

Match the model to the real population. A public API is open; a fixed-seat internal tool is closed.
Mixing them unintentionally produces a number that neither population justifies.

## Scenarios and execs

A scenario is a chain of `exec` blocks. Each `exec` runs an action — usually an `http("name")`
request — and can branch with `doIf`, loop with `repeat`/`forever`/`during`, group with `group`,
and pace with `pace`. State flows between steps through the **session**: a per-virtual-user map you
read and write with EL strings (`#{userId}`) or `saveAs`.

## Feeders

Feeders parametrise scenarios — a different username, product, or token per user — from CSV, JSON,
or in-memory sources. `feed(csv("users.csv").circular)` injects the next record into the session.
Strategies matter: `circular` and `shuffle` recycle records; `queue` exhausts and stops. A test
that reuses one record for every user is testing a cache, not your system — size the feeder to the
population and pick a strategy that does not silently collapse to a single value.

## Checks and assertions

**Checks** run on each individual response (`status.is(200)`, jsonPath, body regex) and mark the
request as failed without aborting the scenario. **Assertions** evaluate the *aggregated* run at
the end (`global.responseTime.mean.lt(200)`, `details("checkout").failedRequests.percent.lt(1)`)
and fail the whole simulation if the SLO is breached. Assertions are how CI gates on performance.

## Running it

| Runner                          | When                                                |
| ------------------------------- | --------------------------------------------------- |
| **Maven / Gradle / sbt plugin** | The default: tests in `src/test/...`, run via the build tool, integrated with CI. |
| **Gatling CLI bundle**          | Standalone runs without a build tool; CI containers. |
| **Gatling Enterprise**          | Distributed runs from multiple load generators, scheduled tests, dashboards, shared reports. Commercial control plane over the same simulations. |

The build plugins (`gatling-maven-plugin`, `gatling-gradle-plugin`) are the durable primary
surface for CI: they compile the simulation, run it, and publish the HTML report as an artifact.
Pin a Gatling version; floating `latest` makes a load-test baseline non-reproducible.

## Reports

After a run Gatling writes a self-contained HTML report: per-request counts, response-time
percentiles (p50/p95/p99), a timeline of active users, and the distribution histogram. The
percentiles are the number that matters — mean response time hides the long tail that real users
experience. Read p95/p99 against your SLO, not the average.

## Gatling vs k6 vs JMeter

| Concern         | Gatling                         | k6                            | JMeter                       |
| --------------- | ------------------------------- | ----------------------------- | ---------------------------- |
| Language        | Java/Kotlin/Scala/JS (as code)  | JavaScript (as code)          | XML GUI / scripted           |
| Engine          | JVM, async, actor-based         | Go, event-loop                | JVM, thread-per-user         |
| Best at         | Protocol load as code, CI       | Developer ergonomics, scripting | GUI authoring, broad protocols |
| Reporting       | Rich static HTML                | CLI + dashboards (Grafana/Cloud) | Live tree + HTML            |

Reach for Gatling when you want protocol-level load defined as compiled, version-controlled code
with first-class CI plugins and detailed reports. Reach for k6 when your team lives in JavaScript
and wants the lowest-friction scripting. Reach for JMeter when you need GUI authoring or an
unusual protocol plugin.

## Current vs mutable

Gatling releases add DSL methods and change defaults frequently. Treat these as mutable and resolve
from the live documentation:

- **Java/Kotlin/JS DSL method names** — the Java-first API is the current direction; older Scala
  method names sometimes differ.
- **Injection and workload methods** (`constantUsersPerSec`, `closedWorkload`, ramping variants)
  — names and availability shift across versions.
- **Build plugin configuration** — `gatling-maven-plugin` and `gatling-gradle-plugin` options and
  the `simulation`/`runDescription` flags change.
- **Enterprise vs open-source feature boundary** — some features move between the OSS engine and
  Gatling Enterprise between releases.

For an exact method name, a plugin option, or whether a feature is OSS-only, read the live Gatling
documentation rather than recalling a version.

## References

- [Gatling documentation home](https://docs.gatling.io/)
- [Getting started](https://docs.gatling.io/tutorials/recording/)
- [Simulation structure](https://docs.gatling.io/reference/script/core/simulation/)
- [Scenario and execs](https://docs.gatling.io/reference/script/core/scenario/)
- [Injection / workload models](https://docs.gatling.io/reference/script/core/injection/)
- [Feeders](https://docs.gatling.io/reference/script/core/session/feeders/)
- [Checks](https://docs.gatling.io/reference/script/http/checks/)
- [Assertions](https://docs.gatling.io/reference/script/core/assertions/)
- [Maven plugin](https://docs.gatling.io/reference/integrations/build-tools/maven-plugin/)
- [Reports](https://docs.gatling.io/reference/result-metrics/general/)
