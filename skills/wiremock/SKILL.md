---
name: wiremock
description: WireMock — open-source HTTP API mocking and stubbing for the JVM, with WireMock .NET for .NET and WireMock Cloud as a managed SaaS. Use when stubbing an HTTP dependency in tests, recording real traffic into reusable stubs, proxying live services, matching requests by URL/method/headers/body, returning dynamic responses with Handlebars response templating, faking delays and faults, running a standalone stub server from the JAR or Docker, or wiring the JUnit 5 / Spring Boot test rule into a Java test suite.
metadata:
  display-name: WireMock
  category: Testing
  tags: [wiremock, api-mocking, mocking, stubbing, testing, java, jvm]
---

# WireMock

WireMock is a tool for simulating HTTP APIs. You describe the responses you want
a dependency to return, point the system under test at WireMock instead of the
real service, and run fast, deterministic, offline tests that do not touch a
network or a database you do not control. The same engine can also stand in front
of a real service, record the traffic it sees, and turn that traffic into stubs.

The model is one sentence: **a stub maps a request matcher to a response.**
Everything else — record/playback, proxying, response templating, fault
injection, stateful behaviour — is a way of generating or shaping those two
halves. Keep that sentence in mind and the feature set becomes legible.

## Mental model

| Concept                 | What it is                                                                                                                   |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Stub mapping**        | One request matcher + one response definition. The atomic unit of WireMock.                                                  |
| **Request matcher**     | URL (path/regex/path template), method, headers, query/body content patterns. The first stub whose matchers all match wins.  |
| **Response definition** | Status, body, headers, fixed or distributed delay, plus optional templating.                                                 |
| **Proxy / recording**   | Forward unmatched traffic to a real target and capture it, then materialise captured mappings.                               |
| **Response templating** | Handlebars expressions in the response body/headers, evaluated against the matched request at serve time for dynamic output. |
| **Scenario / state**    | A state machine per stub group for sequences — return one response the first time, another thereafter.                       |

A request arrives. WireMock walks its mappings in priority order; the first
fully-matched mapping supplies the response. `priority` lets a mapping jump the
queue; otherwise declaration order is the tiebreak. If nothing matches,
WireMock returns a `404` (or, when a proxy is configured, forwards the request
and optionally records it).

## Request matching

Matching is where stubs earn their precision. A loose matcher (path only) is
fine for a smoke stub; a tight one (path + method + a body JSON path + a header)
is what keeps tests from passing against the wrong stub.

| Matcher                             | Typical use                                                       |
| ----------------------------------- | ----------------------------------------------------------------- |
| **URL path**                        | Exact path match.                                                 |
| **URL path pattern (regex)**        | Path templates and dynamic segments.                              |
| **Method**                          | GET/POST/PUT/…                                                    |
| **Query parameters**                | Includes/regex/equals per key.                                    |
| **Headers**                         | Auth tokens, content types; case-insensitive matching.            |
| **Body patterns (regex/XML/XPath)** | Match against the raw body or an XML doc.                         |
| **Path template (`/users/{id}`)**   | Modern path-template matching, capturing segments for templating. |

Body matching is the common footgun: a plain string body match is a substring
test, not equality, which silently passes against a stub you did not intend.
For JSON, prefer a body pattern anchored to the field, or match individual
elements, over a whole-document string compare.

## Responses: static, dynamic, faulty

| Response shape             | How                                                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Static**                 | Literal status, body, headers in the mapping.                                                                             |
| **Templated (Handlebars)** | Enable response templating, then use `{{request.path...}}`, helpers, and custom helpers in the body.                      |
| **Proxy response**         | Forward to a `proxyBaseUrl`, optionally recording the exchange.                                                           |
| **Delay**                  | Fixed `fixedDelayMs`, or `delayDistribution` for a Gaussian spread.                                                       |
| **Faults**                 | `EMPTY_RESPONSE`, `MALFORMED_RESPONSE_CHUNK`, `RANDOM_RESPONSE_THEN_CLOSE`, connection reset — to test client resilience. |
| **Chunked dribble**        | Serve a body in timed chunks to simulate slow streaming endpoints.                                                        |

Response templating is the feature that turns a pile of near-identical stubs
into a handful of flexible ones: read the path segment, echo a request header,
cycle through a list. It must be **explicitly enabled** (a server flag or
extension registration); templating expressions in a response body are left as
literal text until it is on.

## Record and playback

Recording is the fastest way to seed realistic stubs from a real, working
integration:

1. Configure a `proxyBaseUrl` pointing at the live service.
2. Drive the traffic you care about through WireMock (it proxies and records).
3. Snapshot the recorded mappings to disk.

Then replay offline: WireMock serves the recorded responses without the live
service. The trap is to treat a recording as ground truth — recorded stubs
capture exactly the request shape that was sent, so a body pattern recorded
against one payload will not match a slightly different payload next time.
Review and loosen recorded matchers before committing them as a fixture.

## Running it

| Mode                                     | When to reach for it                                                                  |
| ---------------------------------------- | ------------------------------------------------------------------------------------- |
| **JUnit 5 / JUnit 4 rule**               | In-process, per-test, ephemeral port — the default for Java unit tests.               |
| **Spring Boot `@AutoConfigureWireMock`** | Spring integration test wiring, classpath-stubbed `wiremock-server`.                  |
| **Standalone JAR**                       | A shared stub server a whole team/dev environment points at; run anywhere with a JVM. |
| **Docker image**                         | Stub server in CI or docker-compose, no JVM on the host.                              |
| **Programmatic (Java API)**              | Define stubs in code inside a test for full control.                                  |

In-process (JUnit rule) gives the best isolation: each test gets a fresh
WireMock on a random port, and stubs do not leak across tests. Standalone/Docker
suits a long-lived, shared mock that multiple consumers point at — at the cost
of manual reset and shared state between tests.

## Variants: pick the right one

The "WireMock" name covers three related but distinct products. Choosing the
wrong one is the most common mistake.

| Variant                 | Runtime / language | License    | Best for                                                                                                 |
| ----------------------- | ------------------ | ---------- | -------------------------------------------------------------------------------------------------------- |
| **WireMock (Java/JVM)** | JVM (Java 11+)     | Apache 2.0 | The canonical, full-featured core. JUnit rule, standalone JAR, Docker.                                   |
| **WireMock .NET**       | .NET (C#)          | Apache 2.0 | Mocking from .NET test runners; `WireMock.Net` is a separate project, API-similar but not the same code. |
| **WireMock Cloud**      | SaaS               | Commercial | Managed mocking, teams, dashboards, no server to run. Builds on the Java core's concepts.                |

The open-source Java core is the reference implementation and the source of the
documentation on wiremock.org. **WireMock .NET** is a community/affiliated
project (`WireMock.Net` on NuGet) that reimplements the same concepts for the
.NET ecosystem — matchers, response templating, and the record/proxy model map
across, but the API surface, namespaces, and some matcher names differ. **WireMock
Cloud** is the commercial managed product: same mental model, no infrastructure,
plus collaboration and governance features aimed at teams. Pick by your runtime
first (JVM vs .NET), then by whether you want to run a server at all (Cloud).

WireMock also offers a **stub-forge** UI and **WireMock Studio** (historically)
for authoring stubs graphically; treat the CLI, JAR, and in-process rule as the
durable primary surface and check current tooling against the docs.

## In-process vs standalone — decision criteria

- **Per-test, ephemeral, fast feedback** → in-process JUnit rule / Spring Boot
  starter. Random port, no cleanup needed, stubs reset each test.
- **Shared across many clients / a dev environment / CI pipeline** → standalone
  JAR or Docker, with stub mappings loaded from a directory.
- **A team mocking many APIs with history and sharing** → WireMock Cloud, or
  self-host standalone with a mappings store.

## Current vs deprecated

WireMock's extension APIs, response-transformer hooks, and the exact set of
matchers evolve between releases. Treat these as mutable and resolve them from
the live documentation rather than memory:

- **Extension and response-transformer APIs** — the registered extension
  contract and available transformer types; check the current API for your
  version.
- **Standalone JAR and Docker image tags / versions** — pin a version in CI;
  do not float `latest`.
- **Response-templating helper set** — built-in and contributed Handlebars
  helpers change across releases.
- **WireMock .NET API surface** — matcher names and configuration differ from
  the Java core; confirm against the `WireMock.Net` docs, not the Java docs.

For a concrete matcher name, an extension signature, or a Docker tag, read the
live WireMock documentation rather than recalling a version.

## References

- [WireMock documentation home](https://wiremock.org/docs/)
- [Stubbing](https://wiremock.org/docs/stubbing/)
- [Request matching](https://wiremock.org/docs/request-matching/)
- [Response templating](https://wiremock.org/docs/response-templating/)
- [Record and playback](https://wiremock.org/docs/record-playback/)
- [Proxying](https://wiremock.org/docs/proxying/)
- [Simulating faults](https://wiremock.org/docs/simulating-faults/)
- [Running standalone](https://wiremock.org/docs/running-standalone/)
- [JUnit 5 extension](https://wiremock.org/docs/junit-jupiter/)
- [Spring Boot](https://wiremock.org/docs/spring-boot/)
- [WireMock .NET](https://wiremock.org/docs/dotnet/)
- [WireMock Cloud](https://wiremock.org/cloud/)
