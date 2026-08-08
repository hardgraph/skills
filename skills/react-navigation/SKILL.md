---
name: react-navigation
description: Use when adding navigation to a React Native app with React Navigation — choosing between Stack, Tabs, and Drawer navigators, wiring deep linking and web URLs, typing routes, migrating from Expo Router, or diagnosing screen-mounting and header-customisation problems. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: React Navigation
  category: Mobile and frontend
  tags: [react-native, navigation, routing, expo, mobile, deep-linking]
---

# React Navigation

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

React Navigation is the imperative navigation library for React Native. You build a tree of
navigators (Stack, Tabs, Drawer, Material Top Tabs) and navigate between screens by calling
imperative methods. It is the lower-level alternative to Expo Router's file-based routing, and the
two share internals but expose a different mental model.

## The decision that shapes everything else

React Navigation is **v7-current**, and a version boundary is the single most consequential fact
about a project using it. v6 and v7 differ in how navigation state is structured, how the type
system is declared, and several option names — code that "worked on the last project" often targets
the wrong major version. Confirm the installed major version before writing or modifying any
navigator config; behaviour described from memory is a moving target.

The second early decision is **native vs JS navigation**. With `react-native-screens` enabled
(the default and recommended path), screens use real native containers and unmount when off-screen.
With it disabled, every screen in a stack stays mounted and rendered — fine for a prototype, a
performance and state-leak problem at scale. Do not treat "screens stay mounted" as a bug when it
is the documented consequence of disabling native screens.

## Things that get confused

- **React Navigation vs Expo Router.** Expo Router is built *on top of* React Navigation but is
  file-based and URL-first. Picking React Navigation directly is reasonable when you want explicit,
  imperative control or are not in an Expo project; do not assume file-based routing exists because
  "it's all the same library."
- **Navigators are composable, not alternatives.** A Tab navigator can live inside a Stack screen;
  nesting is the normal way to build real apps. The mistake is reaching for one giant Stack.
- **`navigate` vs `reset` vs `replace`.** `navigate` to a screen already in the stack does not
  remount it — it goes back to the existing instance. Use `reset` to wipe history (a logout flow)
  and `replace` to swap the top screen without adding to history. Confusing these is the most common
  "my component didn't re-render" report.
- **Params are for identification, not application state.** Passing large or frequently-changing
  objects through params causes re-renders and history bloat. Keep params small; hold real state
  elsewhere.

## What surprises people

Navigation state lives in React context, not in the URL. A web-style back button or shareable link
only works once you configure **linking** — without it there is no deep linking, no web URL, and no
state restoration across reloads. Linking in v7 uses a path/parse config object whose shape changed
from v6; copying a v6 `linking` block into a v7 app is a common silent break.

## What to verify rather than recall

Option names, the type-declaration API, supported navigator packages, and the linking config shape
all change between major versions. Confirm them against the mirrored corpus under
`references/vendor/` or the live docs rather than asserting a remembered value — a stale option name
fails silently (it is ignored) rather than raising an error.

## References

- [React Navigation documentation](https://reactnavigation.org/docs/getting-started)
- [Navigating between screens](https://reactnavigation.org/docs/navigating)
- [Configuring links (deep linking + web)](https://reactnavigation.org/docs/configuring-links)
- [Upgrading from 6.x](https://reactnavigation.org/docs/upgrading-from-6.x)
