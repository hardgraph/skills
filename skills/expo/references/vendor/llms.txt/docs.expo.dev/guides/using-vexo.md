---
modificationDate: March 05, 2026
title: Using Vexo
description: A guide on installing and configuring Vexo for real-time user analytics.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/using-vexo/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/using-vexo/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Analytics and error reports
Pages in this section:
- [Using analytics](https://docs.expo.dev/guides/using-analytics.md)
- [Using Sentry](https://docs.expo.dev/guides/using-sentry.md)
- [Using BugSnag](https://docs.expo.dev/guides/using-bugsnag.md)
- [Using LogRocket](https://docs.expo.dev/guides/using-logrocket.md)
- [Using Vexo](https://docs.expo.dev/guides/using-vexo.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Vexo

A guide on installing and configuring Vexo for real-time user analytics.

[Vexo](https://www.vexo.co/) provides real-time user analytics for your Expo application, helping you understand how users interact with your app, identify friction points, and improve engagement.

With a two-line integration, Vexo starts collecting data automatically, giving you actionable insights to optimize your app's user experience. If needed, you can also create custom events.

## Features

1.  **Complete Dashboard**
    -   Active Users
    -   Session Time
    -   Downloads
    -   OS Distribution
    -   Version Adoption
    -   Geographic Insights
    -   Popular Screens
2.  **Session Replays**
    -   Watch real user sessions to understand their interactions.
3.  **Heatmaps**
    -   Identify the most engaged areas of your app.
4.  **Funnels**
    -   Analyze user flows and optimize conversion rates.
5.  **Custom Events and Dashboard Personalization**
    -   Track specific user actions by creating custom events.
    -   Customize your dashboard to visualize key metrics.

## Getting started

1.  Create an account: Sign up for a [Vexo account](https://www.vexo.co/).
    
2.  Create a new app: You'll be prompted to create a new app. Give it a name (you can change it later), and once you submit it, you'll receive an API key.
    
3.  Install the Vexo package: Run the following command in your project:
    
    ```sh
    # npm
    npm install vexo-analytics
    
    # yarn
    yarn add vexo-analytics
    
    # pnpm
    pnpm add vexo-analytics
    
    # bun
    bun install vexo-analytics
    ```
    
4.  Initialize Vexo: Add the following code in your app's entry file (for example, **index.js**, **App.js**, or **src/app/_layout.tsx** if using Expo Router):
    
    ```tsx
    import { vexo } from 'vexo-analytics';
    
    // You may want to wrap this with `if (!__DEV__) { ... }` to only run Vexo in production.
    vexo('YOUR_API_KEY');
    ```
    
5.  Rebuild and run your app: Since `vexo-analytics` includes native code, you need to rebuild your application.
    
6.  Verify integration: Go to your app's page on Vexo and you should see your first event!
    

## Compatibility

-   Expo: Vexo is compatible with [Development builds](/develop/development-builds/introduction.md) and does not require additional configuration plugins.
-   Expo Go: Not supported, as Vexo requires custom native code.

## Learn more about Vexo

To learn more about using Vexo with Expo, check out the [Vexo documentation](https://docs.vexo.co/).
