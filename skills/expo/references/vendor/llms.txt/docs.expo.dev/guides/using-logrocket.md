---
modificationDate: June 11, 2026
title: Using LogRocket
description: A guide on installing and configuring LogRocket for session replays and error monitoring.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/using-logrocket/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/using-logrocket/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Analytics and error reports
Pages in this section:
- [Using analytics](https://docs.expo.dev/guides/using-analytics.md)
- [Using Sentry](https://docs.expo.dev/guides/using-sentry.md)
- [Using BugSnag](https://docs.expo.dev/guides/using-bugsnag.md)
- [Using LogRocket](https://docs.expo.dev/guides/using-logrocket.md) (this page)
- [Using Vexo](https://docs.expo.dev/guides/using-vexo.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using LogRocket

A guide on installing and configuring LogRocket for session replays and error monitoring.

[LogRocket](https://logrocket.com) records user sessions and identifies bugs as your users use your app. You can filter sessions by update IDs and also connect to your LogRocket account on the EAS dashboard to get quick access to your app's session data.

## Install and configure LogRocket

You can install the LogRocket SDK with the following command:

```sh
# npm
npx expo install @logrocket/react-native expo-build-properties

# yarn
yarn expo install @logrocket/react-native expo-build-properties

# pnpm
pnpm expo install @logrocket/react-native expo-build-properties

# bun
bun expo install @logrocket/react-native expo-build-properties
```

Then, in your [app config](/workflow/configuration.md), include the LogRocket config plugin:

```json
{
  "plugins": [
    [
      "expo-build-properties",
      {
        "android": {
          "minSdkVersion": 25
        }
      }
    ],
    "@logrocket/react-native"
  ]
}
```

Finally, initialize LogRocket in your app in a top-level file, like **src/app/_layout.tsx**:

```tsx
import { useEffect } from 'react';
import * as Updates from 'expo-updates';
import LogRocket from '@logrocket/react-native';

const App = () => {
  useEffect(() => {
    LogRocket.init('<App ID>', {
      updateId: Updates.isEmbeddedLaunch ? null : Updates.updateId,
      expoChannel: Updates.channel,
    });
  }, []);
};
```

In the code above, replace `<App ID>` with your [LogRocket App ID](https://app.logrocket.com/r/settings/setup).

## Connecting LogRocket on the EAS dashboard

You can link your LogRocket account and project to your Expo account and project on Expo's dashboard, so that you can see the last few sessions from your app in the deployments and updates dashboards.

Go to your **Account settings** > [**Overview**](https://expo.dev/accounts/%5Baccount%5D/settings) > **Connections** and click **Connect** to authenticate with LogRocket:

Then, go to your project, under **Project settings** > [**General**](https://expo.dev/accounts/%5Baccount%5D/projects/%5BprojectName%5D/settings) and click **Connect** to link your LogRocket project with your project on Expo:

Then, you'll start to see **View on LogRocket** buttons in the EAS dashboard in the Native Deployments and Updates dashboards, along with the last few sessions from your app.

## Learn more about LogRocket

To learn more about how to use LogRocket with Expo, check out the [LogRocket documentation](https://docs.logrocket.com/reference/react-native-expo-adding-the-sdk).
