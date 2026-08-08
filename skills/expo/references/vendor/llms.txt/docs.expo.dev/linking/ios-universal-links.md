---
modificationDate: June 03, 2026
title: iOS Universal Links
description: Learn how to configure iOS Universal Links to open your Expo app from a standard web URL.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/linking/ios-universal-links/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/linking/ios-universal-links/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Linking
Pages in this section:
- [Overview](https://docs.expo.dev/linking/overview.md)
- [Into other apps](https://docs.expo.dev/linking/into-other-apps.md)
- [Into your app](https://docs.expo.dev/linking/into-your-app.md)
- [Android App Links](https://docs.expo.dev/linking/android-app-links.md)
- [iOS Universal Links](https://docs.expo.dev/linking/ios-universal-links.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# iOS Universal Links

Learn how to configure iOS Universal Links to open your Expo app from a standard web URL.

To configure iOS Universal Links for your app, you need to set up the **two-way association** to verify your website and native app.

[Watch: Set up iOS Universal Links with Expo Router](https://www.youtube.com/watch?v=kNbEEYlFIPs&t=68) — Learn how to configure iOS Universal Links so your Expo app opens from standard web URLs.

## Set up two-way association

To setup **two-way association** between the website and app for iOS, you need to perform the following steps:

-   **Website verification:** This requires creating a **apple-app-site-association (AASA)** file inside the **/.well-known** directory and hosting it on the target website. This file is used to verify that the app opened from a given link is the correct app.
-   **Native app verification:** This requires some form of code signing that references the target website domain (URL).

### Create AASA file

Create an **apple-app-site-association** file for the website verification inside the **/.well-known** directory. This file specifies your Apple Developer Team ID, bundle identifier, and a list of supported paths to redirect to the native app.

> You can run the [experimental](/more/release-statuses.md#experimental) CLI command `npx setup-safari` inside your project to automatically register a bundle identifier to your Apple account, assign entitlements to the ID, and create an iTunes app entry in the store. The local setup will be printed and you can skip most the following. This is the easiest way to get started with universal links on iOS.

If you're using Expo Router to build your website (or any other modern React framework such as Remix, Next.js, and so on), create the AASA file at **public/.well-known/apple-app-site-association**. For legacy Expo webpack projects, create the file at **web/.well-known/apple-app-site-association**.

```json
{
  // This section enables Universal Links
  "applinks": {
    "apps": [],
    "details": [
      {
        // Syntax: "<APPLE_TEAM_ID>.<BUNDLE_ID>"
        "appID": "QQ57RJ5UTD.com.example.myapp",
        // All paths that should support redirecting.
        "paths": ["/records/*"]
      }
    ]
  },
  // This section enables Apple Handoff
  "activitycontinuation": {
    "apps": ["<APPLE_TEAM_ID>.<BUNDLE_ID>"]
  },
  // This section enable Shared Web Credentials
  "webcredentials": {
    "apps": ["<APPLE_TEAM_ID>.<BUNDLE_ID>"]
  }
}
```

In the above example:

-   Any links to `https://www.myapp.io/records/*` (with wildcard matching for the record ID) should be opened directly by the app with a matching bundle identifier on an iOS device. It is a combination of the [Apple Team ID](https://expo.fyi/apple-team) and the bundle identifier.
-   The `*` wildcard does **not** match domain or path separators (periods and slashes).
-   The `activitycontinuation` and `webcredentials` objects are optional, but recommended.

> See [Apple's documentation](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html) for further details on the format of the AASA. Branch provides an [AASA validator](https://branch.io/resources/aasa-validator/) which can help you confirm that your AASA is correctly deployed and has a valid format.

### Supporting `details` format

The [`details` format is supported](https://developer.apple.com/documentation/xcode/supporting-associated-domains) as of iOS 13. It allows you to specify:

-   `appIDs` instead of `appID`: Makes it easier to associate multiple apps with the same configuration
-   An array of `components`: Allows you to specify fragments, exclude specific paths, and add comments

#### An example AASA JSON from Apple's documentation

```json
{
  "applinks": {
    "details": [
      {
        "appIDs": ["ABCDE12345.com.example.app", "ABCDE12345.com.example.app2"],
        "components": [
          {
            "#": "no_universal_links",
            "exclude": true,
            "comment": "Matches any URL whose fragment equals no_universal_links and instructs the system not to open it as a universal link"
          },
          {
            "/": "/buy/*",
            "comment": "Matches any URL whose path starts with /buy/"
          },
          {
            "/": "/help/website/*",
            "exclude": true,
            "comment": "Matches any URL whose path starts with /help/website/ and instructs the system not to open it as a universal link"
          },
          {
            "/": "/help/*",
            "?": {
              "articleNumber": "????"
            },
            "comment": "Matches any URL whose path starts with /help/ and which has a query item with name 'articleNumber' and a value of exactly 4 characters"
          }
        ]
      }
    ]
  }
}
```

To support all iOS versions, you can provide both the above formats in your `details` key, but we recommend placing the configuration for more recent iOS versions first.

### Host AASA file

Host the **apple-app-site-association** file using a web server with your domain. This file must be served over an HTTPS connection. Verify that your browser can access this file.

After you have setup the AASA file, deploy your website to a server that supports HTTPS (most modern web hosts).

### Native app configuration

After deploying your **apple-app-site-association** (AASA) file, configure your app to use your associated domain by adding [`ios.associatedDomains`](/versions/latest/config/app.md#associateddomains) to your [app config](/workflow/configuration.md). Make sure to follow [Apple's specified format](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_associated-domains) and **not** include the protocol (`https`) in your URL. This is a common mistake that will result in the universal links not working.

For example, if an associated website is `https://expo.dev/`, the `applinks` is:

```json
{
  "expo": {
    "ios": {
      "associatedDomains": ["applinks:expo.dev"]
    }
  }
}
```

Build your iOS app with [EAS Build](/build/setup.md) which ensures that the entitlement is registered with Apple automatically.

#### Manual native configuration

If you're not using EAS or [Continuous Native Generation](/workflow/continuous-native-generation.md) (`npx expo prebuild`), you have to [manually configure](/build-reference/ios-capabilities.md#manual-setup) the **Associated Domains** capability for your bundle identifier.

If you enable through the [Apple Developer Console](/build-reference/ios-capabilities.md#apple-developer-console), then make sure to add the following entitlements in your **ios/[app]/[app].entitlements** file:

```xml
<key>com.apple.developer.associated-domains</key>
<array>
  <string>applinks:expo.dev</string>
</array>
```

### Native app verification

Install the app on your iOS device to trigger the verification process. A link to your website on your mobile device should open your app. If it doesn't, re-check the previous steps to ensure that your AASA is valid, the path specified in the AASA, and you have correctly configured your App ID in the [Apple Developer Console](https://developer.apple.com/account/resources/identifiers/list).

Once you have your app opened, see [Handle links into your app](/linking/into-your-app.md#handle-urls) for more information on how to handle inbound links and show the user the content they requested.

> iOS downloads your AASA when your app is first installed or when updates are installed from the App Store. The operating system does not refresh frequently after that. If you want to change the paths in your AASA for a production app, you will need to issue a full update via the App Store so that all of your users' apps re-fetch your AASA and recognize the new paths.

## Apple Smart Banner

If a user doesn't have your app installed, they'll be directed to the website. You can use the [Apple Smart Banner](https://developer.apple.com/documentation/webkit/promoting_apps_with_smart_app_banners) to show a banner at the top of the page that prompts the user to install the app. The banner will only show up if the user is on a mobile device and doesn't have the app installed.

To enable the banner, add the following meta tag to the `<head>` of your website, replacing `<ITUNES_ID>` with your app's iTunes ID:

```html
<meta name="apple-itunes-app" content="app-id=<ITUNES_ID>" />
```

If you're having trouble setting up the banner, run the following command to automatically generate the meta tag for your project:

```sh
# npm
npx setup-safari

# yarn
yarn dlx setup-safari

# pnpm
pnpm dlx setup-safari

# bun
bunx setup-safari
```

### Add the meta tag to your statically rendered website

If you're building a [statically rendered website with Expo Router](/router/web/static-rendering.md), then add the HTML tag to the `<head>` component in your [**src/app/+html.js** file](/router/web/static-rendering.md#root-html).

```tsx
import { type PropsWithChildren } from 'react';

export default function Root({ children }: PropsWithChildren) {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta httpEquiv="X-UA-Compatible" content="IE=edge" />
        <meta name="apple-itunes-app" content="app-id=<ITUNES_ID>" />
        {/* Other head elements... */}
      </head>
      <body>{children}</body>
    </html>
  );
}
```

## Debugging

Expo CLI enables you to test iOS Universal Links without deploying a website. Utilizing the [`--tunnel`](/more/expo-cli.md#tunneling) functionality, you can forward your dev server to a publicly available HTTPS URL.

Set the environment variable `EXPO_TUNNEL_SUBDOMAIN=my-custom-domain` where `my-custom-domain` is a unique string that you use during development. This ensures that your tunnel URL is consistent across dev server restarts.

Add `associatedDomains` to your app config as [described above](/linking/ios-universal-links.md#native-app-configuration). Replace the domain value with a Ngrok URL: `my-custom-domain.ngrok.io`.

Start your dev server with the `--tunnel` flag:

```sh
# npm
npx expo start --tunnel

# yarn
yarn expo start --tunnel

# pnpm
pnpm expo start --tunnel

# bun
bun expo start --tunnel
```

Compile the development build on your device:

```sh
# npm
npx expo run:ios

# yarn
yarn expo run:ios

# pnpm
pnpm expo run:ios

# bun
bun expo run:ios
```

You can now type your custom domain link in your device's web browser to open your app.

## Troubleshooting

Here are some common tips to help you troubleshoot when implementing iOS Universal Links:

-   Read Apple's official documentation on [debugging universal links](https://developer.apple.com/documentation/technotes/tn3155-debugging-universal-links)
-   Ensure your apple app site association file is valid by using a [validator tool](https://branch.io/resources/aasa-validator/).
-   The uncompressed `apple-app-site-association` file cannot be [larger than 128kb](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html).
-   Ensure your website is served over HTTPS.
-   If you update your web files, rebuild the native app to trigger a server update on the vendor side (Apple).
