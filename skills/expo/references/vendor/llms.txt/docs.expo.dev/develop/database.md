---
modificationDate: February 05, 2026
title: Databases in Expo and React Native apps
description: Learn about adding a database to your Expo project.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/develop/database/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/develop/database/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Develop
Pages in this section:
- [Tools for development](https://docs.expo.dev/develop/tools.md)
- [Navigation](https://docs.expo.dev/develop/app-navigation.md)
- [Database](https://docs.expo.dev/develop/database.md) (this page)
- [Authentication](https://docs.expo.dev/develop/authentication.md)
- [Unit testing](https://docs.expo.dev/develop/unit-testing.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Databases in Expo and React Native apps

Learn about adding a database to your Expo project.

Most apps need to persist data beyond the lifetime of a single session. You can use a cloud-hosted database to store your app's data and sync it across devices and users.

## Convex

[Convex](https://www.convex.dev/) is a TypeScript-based database that requires no cluster management, SQL, or ORMs. Convex provides real-time updates over a WebSocket, making it perfect for reactive apps.

[Using Convex](/guides/using-convex.md) — Add a database to your app with Convex.

## Supabase

[Supabase](https://supabase.com/) is an app development platform that provides hosted backend services such as a Postgres database, user authentication, file storage, edge functions, realtime syncing, and a vector and AI toolkit.

[Using Supabase](/guides/using-supabase.md) — Add a Postgres database and user authentication to your app with Supabase.

## Firebase

[Firebase](https://firebase.google.com/) is an app development platform that provides hosted backend services such as real-time database, cloud storage, authentication, crash reporting, analytics, and more. It is built on Google's infrastructure and scales automatically.

[Using Firebase](/guides/using-firebase.md) — Get started with Firebase JS SDK and React Native Firebase.
