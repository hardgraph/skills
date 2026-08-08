---
modificationDate: July 29, 2026
title: Using Supabase
description: Add a Postgres database and user authentication to your React Native app with Supabase.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/using-supabase/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/using-supabase/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Integrations > Database and SDKs
Pages in this section:
- [Using Convex](https://docs.expo.dev/guides/using-convex.md)
- [Using Firebase](https://docs.expo.dev/guides/using-firebase.md)
- [Using Supabase](https://docs.expo.dev/guides/using-supabase.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Using Supabase

Add a Postgres database and user authentication to your React Native app with Supabase.

[Supabase](https://supabase.com/?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) is a Backend-as-a-Service (BaaS) app development platform that provides hosted backend services such as a Postgres database, user authentication, file storage, edge functions, realtime syncing, and a vector and AI toolkit. It's an open-source alternative to Google's Firebase.

Supabase automatically [generates a REST API](https://supabase.com/docs/guides/api?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) from your database and employs a concept called [row level security (RLS)](https://supabase.com/docs/guides/auth/row-level-security?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) to secure your data, making it possible to directly interact with your database from your React Native application without needing to go through a server!

Supabase provides a TypeScript client library called [`supabase-js`](https://supabase.com/docs/reference/javascript/introduction?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) to interact with the REST API. Alternatively, Supabase also exposes a [GraphQL API](https://supabase.com/docs/guides/database/extensions/pg_graphql?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) allowing you to use your favorite GraphQL client (for example, [Apollo Client](https://supabase.github.io/pg_graphql/usage_with_apollo/)) should you wish to.

#### Prerequisites

##### A Supabase project

Head over to [database.new](https://database.new?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) to create a new Supabase project.

##### Project URL and Publishable key

Get the **Project URL** from the API settings and **Publishable key** from the API Keys:

1.  Go to the [API Settings](https://supabase.com/dashboard/project/_/settings/api) page in the Dashboard.
2.  Find your Project `URL` and `service_role` keys on this page.
3.  Then go to the [API Keys](https://supabase.com/dashboard/project/_/settings/api-keys).
4.  Find your Project **Publishable key** on this page under the API Keys tab.

## Using the Supabase TypeScript SDK

Using [`supabase-js`](https://supabase.com/docs/reference/javascript/introduction?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) is the most convenient way of leveraging the full power of the Supabase stack as it conveniently combines all the different services (database, auth, realtime, storage, edge functions) together.

### Install and initialize the Supabase TypeScript SDK

After you have created your [Expo project](/get-started/create-a-project.md), install `@supabase/supabase-js` and the required dependencies using the following command:

```sh
# npm
npx expo install @supabase/supabase-js expo-sqlite

# yarn
yarn expo install @supabase/supabase-js expo-sqlite

# pnpm
pnpm expo install @supabase/supabase-js expo-sqlite

# bun
bun expo install @supabase/supabase-js expo-sqlite
```

Create a helper file to initialize the Supabase client (`@supabase/supabase-js`). You need the API URL and the `Publishable` key copied [earlier](/guides/using-supabase.md#prerequisites). These variables are safe to expose in your Expo app since Supabase has [Row Level Security](https://supabase.com/docs/guides/auth/row-level-security?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) enabled in the Database.

```ts
import 'expo-sqlite/localStorage/install';
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = YOUR_REACT_NATIVE_SUPABASE_URL!;
const supabasePublishableKey = YOUR_REACT_NATIVE_SUPABASE_PUBLISHABLE_KEY!;

export const supabase = createClient(supabaseUrl, supabasePublishableKey, {
  auth: {
    storage: localStorage,
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: false,
  },
});
```

Now you can `import { supabase } from '/utils/supabase'` throughout your application to leverage the full power of Supabase!

## Next steps

[Build a User Management App](https://supabase.com/docs/guides/getting-started/tutorials/with-expo-react-native?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) — Learn how to combine Supabase Auth and Database in this quickstart guide.

[Sign in with Apple](https://supabase.com/docs/guides/auth/social-login/auth-apple?platform=react-native&utm_source=expo&utm_medium=referral&utm_term=expo-react-native) — Supabase Auth supports using Sign in with Apple on the web and in native apps for iOS, macOS, watchOS or tvOS.

[Sign in with Google](https://supabase.com/docs/guides/auth/social-login/auth-google?platform=react-native&utm_source=expo&utm_medium=referral&utm_term=expo-react-native) — Supabase Auth supports Sign in with Google on the web, native Android applications, and Chrome extensions.

[Deep Linking for OAuth and Magic Links](https://supabase.com/docs/guides/auth/native-mobile-deep-linking?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) — When performing OAuth or sending magic link emails from native mobile applications, learn how to configure deep linking for Android and iOS applications.

[Offline-first React Native Apps with WatermelonDB](https://supabase.com/blog/react-native-offline-first-watermelon-db?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) — Learn how to store your data locally and sync it with Postgres using WatermelonDB.

[React Native file upload with Supabase Storage](https://supabase.com/blog/react-native-storage?utm_source=expo&utm_medium=referral&utm_term=expo-react-native) — Learn how to implement authentication and file upload in a React Native app.
