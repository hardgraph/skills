---
modificationDate: January 06, 2026
title: Using authentication SDKs and libraries
description: An overview of authentication integrations available in the Expo and React Native ecosystem.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/using-authentication/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/using-authentication/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Authentication
Pages in this section:
- [Overview](https://docs.expo.dev/guides/using-authentication.md) (this page)
- [Using Clerk](https://docs.expo.dev/guides/using-clerk.md)
- [Using Facebook authentication](https://docs.expo.dev/guides/facebook-authentication.md)
- [Using Google authentication](https://docs.expo.dev/guides/google-authentication.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using authentication SDKs and libraries

An overview of authentication integrations available in the Expo and React Native ecosystem.

Authentication in mobile apps refers to how you identify who a user is, manage sign-up or sign-in flows, and maintain their authenticated session across app launches and across multiple devices. Authentication SDKs and libraries help you add these flows, so you do not need to build your own custom auth backend. The guides below highlight popular SDKs and providers for your Expo and React Native projects.

> Some providers require custom native code and aren't supported in Expo Go. Use a [development build](/develop/development-builds/introduction.md) when needed.

[Using Clerk](/guides/using-clerk.md) — Add Clerk authentication and user management to your Expo and React Native projects.

[Using Facebook authentication](/guides/facebook-authentication.md) — Configure react-native-fbsdk-next to add Facebook authentication in your Expo project. — react-native-fbsdk-next

[Using Google authentication](/guides/google-authentication.md) — Configure @react-native-google-signin/google-signin to add Google authentication in your Expo project. — @react-native-google-signin/google-signin
