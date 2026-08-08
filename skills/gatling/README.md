# gatling

![gatling cover](./assets/readme-cover.png)

Reference skill for [Gatling](https://gatling.io/) — the load- and performance-testing engine that
runs protocol-level tests as code. It steers an agent through writing a simulation in
Java/Kotlin/Scala/JavaScript, modelling closed vs open workloads, parametrising with feeders,
checks and assertions, the Maven/Gradle/sbt CI plugins, HTML report reading, and the choice
between Gatling, k6, and JMeter — without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill gatling
```

## Use this skill for

- Load testing an HTTP API with thousands of virtual users
- Writing a Gatling simulation scenario with execs, loops, and conditionals
- Choosing a closed (concurrent users) vs open (requests/sec) workload model
- Parametrising a test with CSV/JSON feeders
- Asserting p95/p99 latency and error-rate SLOs in CI
- Running Gatling from the Maven or Gradle plugin in a pipeline
- Deciding between Gatling, k6, and JMeter

## What is included

- [`SKILL.md`](./SKILL.md) — the Gatling mental model, the workload-model decision, and the CI
  running modes.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible verbatim mirror
  of the Gatling documentation via its official
  [llms.txt index](https://docs.gatling.io/llms.txt).

## Source

Reference material is reproduced from the
[Gatling documentation](https://docs.gatling.io) via its official
[llms.txt index](https://docs.gatling.io/llms.txt).
