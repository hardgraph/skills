---
name: chrome-devtools
description: Chrome DevTools — the inspector, debugger, profiler, and automation toolset built into Chrome and Edge. Use when debugging web pages (DOM, styles, network, console), profiling runtime or load performance, finding memory leaks, inspecting storage and service workers, debugging JavaScript with breakpoints and source maps, recording and replaying user flows with the Recorder, emulating devices and network conditions, running remote debugging over CDP, or driving DevTools headless for CI. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Chrome DevTools
  category: Developer tools
  tags: [browser, debugging, performance, network, automation]
---

# Chrome DevTools

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Chrome DevTools is the in-browser toolkit for inspecting, debugging, and
profiling web applications. Open it with `Cmd+Opt+I` (macOS) or `Ctrl+Shift+I`.
Every panel answers a different question; knowing which panel owns a problem is
most of using DevTools well.

## Mental model

| Panel           | Answers                                                |
| --------------- | ------------------------------------------------------ |
| **Elements**    | What is in the DOM and the computed styles right now?  |
| **Console**     | What did the page log, and what can I evaluate here?   |
| **Sources**     | Where is my code, and what is it doing as it runs?     |
| **Network**     | What requests did the page make, and how did they go?  |
| **Performance** | Where is time spent during an interaction or a load?   |
| **Memory**      | What is retaining objects so they cannot be GC'd?      |
| **Application** | What is in storage, and who registered the service worker? |
| **Recorder**    | Can I record this flow and replay it (and measure it)? |

The panels share state: select an element in Elements and `$0` refers to it in
the Console; a network request can be replayed or have its headers copied;
a performance trace links a flame-chart task back to its source function.

## Resolving the surface

**Resolve the current DevTools feature set from the live docs, not memory.**
DevTools ships with Chrome and changes with every release; panel names, command
menus, and settings drift.

```
Cmd+Shift+P          open the Command Menu (all DevTools actions)
Cmd+P                open a file in Sources
Cmd+F (in a panel)   search within the current panel
```

The Command Menu is the single entry point — nearly every capability, including
experimental ones and screenshots, is reachable by typing its name.

## Debugging JavaScript

Set breakpoints in **Sources** rather than scattering `console.log`. A **line
breakpoint** pauses execution; a **conditional breakpoint** (right-click the
gutter) pauses only when an expression is true — invaluable for catching a bad
value in a hot loop. **Logpoints** let you log without editing source.

When source maps are present, DevTools shows authored (TypeScript/Sass) source
and maps breakpoints onto it. If breakpoints "don't hit", the usual cause is a
mismatched or missing source map, or that production builds strip function names
— toggle "Enable JavaScript source maps" and rebuild with source maps on.

## Performance profiling

Two distinct tools, often confused:

- **Performance** panel (formerly *Record*): records a timeline of the page's
  work — main thread, compositor, network, GPU. Use it for jank, slow
  interactions, and long tasks. Read the **flame chart**: a wide block on the
  main thread is time your user is waiting.
- **Inspect animations / Layers**: for visual jank tied to compositing and paint,
  not script.

Always profile a **clean profile** (no DevTools open during capture where
possible, or use the puppeteer/`--headless` path), record the smallest
reproduction, and look at the longest task first. "Performance" measurements are
colored by whether you captured a page load or an interaction — record the right
one.

## Memory leaks

The **Memory** panel has three heap-snapshot modes. The workflow that actually
finds leaks:

1. Take snapshot A.
2. Perform the action you suspect leaks (open then close a view, navigate away
   and back).
3. Take snapshot B.
4. Use the **Comparison** view filtered to "Objects allocated between Snapshot 1
   and 2" and look for retained objects whose retainer chain leads back to an
   unexpected holder (a detached DOM node, a listener, a cache).

Allocation instrumentation over time is the better choice when the leak is a slow
drip rather than a single event.

## Network and storage

- **Network**: preserve log across navigations ("Preserve log"), disable cache
  to defeat 304s during testing, and right-click a request to copy as `fetch` or
  `cURL`. The **Waterfall** shows timing phases; the **Timing** tab breaks them
  down.
- **Application**: inspect cookies, Local/Session Storage, IndexedDB, and the
  registered service worker. "Unregister" a service worker here when a stale one
  traps an old build; "Update on reload" forces it to re-fetch on every
  navigation while debugging.

## Automation and remote debugging

DevTools speaks the **Chrome DevTools Protocol (CDP)**. That is what powers
Puppeteer, Playwright, the Recorder's export-to-Puppeteer, and remote debugging
of a Chrome on another device or a Cloudflare Browser Rendering instance. The
wire format is JSON over WebSocket at the debugging endpoint printed by
`chrome --remote-debugging-port=9222`.

## Current vs deprecated

- Prefer the **Recorder** panel for recording and replaying flows over manual
  scripting; it exports to Puppeteer/Playwright JSON and measures performance
  per step.
- "Performance Monitor" (the live-meters overlay) is for spotting, not
  diagnosing — record a trace to diagnose.
- Device emulation emulates viewport and some APIs; it is not a substitute for
  testing on a real device, and it does not reproduce device GPU or scheduling.

## References

- [Chrome DevTools documentation](https://developer.chrome.com/docs/devtools)
- [Open Chrome DevTools](https://developer.chrome.com/docs/devtools/open)
- [Console reference](https://developer.chrome.com/docs/devtools/console)
- [Performance analysis reference](https://developer.chrome.com/docs/devtools/performance)
- [Memory reference](https://developer.chrome.com/docs/devtools/memory-problems)
- [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/)
