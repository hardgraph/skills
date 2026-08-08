---
modificationDate: July 29, 2026
title: React Native analytics SDKs and libraries
description: An overview of analytics services available in the Expo and React Native ecosystem.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/using-analytics/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/using-analytics/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Analytics and error reports
Pages in this section:
- [Using analytics](https://docs.expo.dev/guides/using-analytics.md) (this page)
- [Using Sentry](https://docs.expo.dev/guides/using-sentry.md)
- [Using BugSnag](https://docs.expo.dev/guides/using-bugsnag.md)
- [Using LogRocket](https://docs.expo.dev/guides/using-logrocket.md)
- [Using Vexo](https://docs.expo.dev/guides/using-vexo.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# React Native analytics SDKs and libraries

An overview of analytics services available in the Expo and React Native ecosystem.

An analytics service allows you to track how users interact with your app. With this data, you can take a measured approach when improving your app.

The following list provides common analytics providers that are available in the Expo and React Native ecosystem.

> Most analytics SDK requires configuring custom native code. Native code is not configurable when using Expo Go. However, you can create a [development build](/develop/development-builds/introduction.md), which will allow you to use any of the services below.

[Google Firebase Analytics](https://rnfirebase.io/analytics/usage) — Learn how to integrate React Native Firebase Analytics in your project.

[Segment](https://segment.com/docs/connections/sources/catalog/libraries/mobile/react-native/) — Learn how to integrate Segment Analytics SDK in your project.

[Amplitude](https://www.docs.developers.amplitude.com/data/sdks/typescript-react-native/) — Learn how to integrate Amplitude Analytics SDK in your project.

[AWS Amplify](https://docs.amplify.aws/lib/analytics/getting-started/q/platform/react-native/) — Learn how to integrate AWS Amplify Analytics in your project.

[Vexo](https://docs.vexo.co/) — Learn how to integrate Vexo Analytics in your project.

[Aptabase](https://aptabase.com/for-react-native) — Learn how to integrate Aptabase Analytics in your project. Works with Expo Go.

[Astrolytics](https://www.astrolytics.io/react-native) — Learn how to integrate Astrolytics in your project. Works with Expo Go.

[PostHog](https://posthog.com/docs/libraries/react-native) — Learn how to integrate PostHog in your project. Works with Expo Go.

[Dreambase](https://dreambase.ai/docs) — Learn how to integrate Dreambase Analytics to track user behavior and app performance in your Expo and Supabase project.
