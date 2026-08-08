# wiremock

![wiremock cover](./assets/readme-cover.png)

Reference skill for [WireMock](https://wiremock.org/) — the open-source HTTP API
mocking and stubbing tool for the JVM, with WireMock .NET and WireMock Cloud
variants. It steers an agent through stubbing a request matcher to a response,
recording and replaying real traffic, proxying live services, dynamic Handlebars
response templating, fault and delay injection, and the choice between the
in-process JUnit/Spring rule, the standalone JAR, Docker, and WireMock Cloud —
without relying on stale version recall.

## Install

```bash
npx skills add hardgraph/skills --skill wiremock
```

## Use this skill for

- Stubbing an HTTP dependency so a test runs fast, deterministic, and offline
- Matching incoming requests by URL, method, headers, query, or body pattern
- Returning a dynamic response with Handlebars response templating
- Recording real traffic into a real service and replaying it as stubs
- Proxying live traffic through WireMock while capturing mappings
- Injecting delays, faults, and chunked dribble to test client resilience
- Picking between the JVM core, WireMock .NET, and WireMock Cloud
- Running a shared stub server from the standalone JAR or Docker image

## What is included

- [`SKILL.md`](./SKILL.md) — the WireMock mental model and the variant/running-mode
  decision criteria.
- [`references/vendor/llms.txt/`](./references/vendor/llms.txt/) — a reproducible
  verbatim mirror of the WireMock documentation (the official llms.txt index and
  the complete llms-full.txt documentation it links to).

## Source

Reference material is reproduced from the
[WireMock documentation](https://wiremock.org) via its official
[llms.txt index](https://wiremock.org/llms.txt).
