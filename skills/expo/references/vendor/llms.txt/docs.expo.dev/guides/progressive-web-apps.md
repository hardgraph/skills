---
modificationDate: June 03, 2026
title: Progressive web apps
description: Learn how to add progressive web app support to Expo websites.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/progressive-web-apps/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/progressive-web-apps/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Web
Pages in this section:
- [Develop websites](https://docs.expo.dev/workflow/web.md)
- [Publish websites](https://docs.expo.dev/guides/publishing-websites.md)
- [DOM components](https://docs.expo.dev/guides/dom-components.md)
- [React Server Components](https://docs.expo.dev/guides/server-components.md)
- [Testing RSC](https://docs.expo.dev/guides/testing-rsc.md)
- [Progressive web apps](https://docs.expo.dev/guides/progressive-web-apps.md) (this page)
- [Tailwind CSS](https://docs.expo.dev/guides/tailwind.md)
- [Local HTTPS development](https://docs.expo.dev/guides/local-https-development.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Progressive web apps

Learn how to add progressive web app support to Expo websites.

A progressive web app (or PWA for short) is a website that can be installed on the user's device and used offline. We recommend building native apps whenever possible as they have the best offline support, but PWAs are a great option for desktop users.

## Favicons

Expo CLI automatically generates the **favicon.ico** file based on the `web.favicon` field in the **app.json**.

```json
{
  "web": {
    "favicon": "./assets/favicon.png"
  }
}
```

Alternatively, you can create a **favicon.ico** file in the **public** directory to manually specify the icon.

## Manifest file

PWAs can be [configured with a manifest file](https://developer.mozilla.org/en-US/docs/Web/Manifest) that describes the app's name, icon, and other metadata.

Create a PWA manifest in **public/manifest.json**:

```json
{
  "short_name": "Expo App",
  "name": "Expo Router Sample",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

The files **logo192.png** and **logo512.png** are the icons that will be used when the app is installed on the user's device. These should be added to the **public** directory too.

`public`

 `manifest.json``PWA Manifest`

 `logo192.png``192x192 icon`

 `logo512.png``512x512 icon`

Now link the manifest in your HTML file. The method here depends on the output mode of your website (indicated in `web.output` in the **app.json**––defaults to `single`).

#### single

If you're using a single-page app, you can link the manifest in your HTML file by first creating a template HTML in **public/index.html**:

```sh
# npm
npx expo customize public/index.html

# yarn
yarn expo customize public/index.html

# pnpm
pnpm expo customize public/index.html

# bun
bun expo customize public/index.html
```

Then add the manifest to the `<head>` tag:

```html
<link rel="manifest" href="/manifest.json" />
```

#### static & server

If you're using static or server rendering, the HTML entry can be dynamically created in **src/app/+html.tsx**. Here we'll link the manifest by adding a `<link>` tag to the `<head>` component:

```tsx
import { ScrollViewStyleReset } from 'expo-router/html';
import type { PropsWithChildren } from 'react';

// This file is web-only and used to configure the root HTML for every
// web page during static rendering.
// The contents of this function only run in Node.js environments and
// do not have access to the DOM or browser APIs.
export default function Root({ children }: PropsWithChildren) {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta httpEquiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        {/* Link the PWA manifest file. */}
        <link rel="manifest" href="/manifest.json" />

        {/*
          Disable body scrolling on web. This makes ScrollView components work closer to how they do on native.
          However, body scrolling is often nice to have for mobile web. If you want to enable it, remove this line.
        */}
        <ScrollViewStyleReset />

        {/* Add any additional <head> elements that you want globally available on web... */}
      </head>
      <body>{children}</body>
    </html>
  );
}
```

## Service workers

Service workers are primarily used to add offline support to websites. Google's Workbox is the best way to add service workers to a website. Follow the guide for [using Workbox CLI](https://developer.chrome.com/docs/workbox/modules/workbox-cli/), and wherever it refers to a "build script" use `npx expo export -p web` instead.

> Be careful adding service workers as they are known to cause unexpected behavior on web. If you accidentally ship a service worker that aggressively caches your website, users cannot request updates easily. For the best offline mobile experience, create a native app with Expo. Unlike websites with service workers, native apps can be updated through the app store to clear the cached experience. This would be similar to resetting the user's native browser (which they may have to do if the service worker is aggressive enough). See [why service workers are suboptimal](https://github.com/facebook/create-react-app/issues/2398) for more information.

For example, here's a possible flow for setting up Workbox:

Create a new project with the following command:

```sh
# npm
npm create expo -t tabs my-app
cd my-app

# yarn
yarn create expo -t tabs my-app
cd my-app

# pnpm
pnpm create expo -t tabs my-app
cd my-app

# bun
bun create expo -t tabs my-app
cd my-app
```

Now register the service worker in the HTML file. The method here depends on the output mode of your website (indicated in `web.output` in the **app.json**––defaults to `single`).

#### single

Next add a service worker registration script to the root **index.html**.

First create a template HTML in **public/index.html** if one does not already exist:

```sh
# npm
npx expo customize public/index.html

# yarn
yarn expo customize public/index.html

# pnpm
pnpm expo customize public/index.html

# bun
bun expo customize public/index.html
```

Then create the service worker registration script in the `<head>` tag:

```html
<script>
  if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
      navigator.serviceWorker
        .register('/sw.js')
        .then(registration => {
          console.log('Service Worker registered with scope:', registration.scope);
        })
        .catch(error => {
          console.error('Service Worker registration failed:', error);
        });
    });
  }
</script>
```

#### static & server

Next, create a root HTML file for the app and add the service worker registration script:

```tsx
import { ScrollViewStyleReset } from 'expo-router/html';
import type { PropsWithChildren } from 'react';

// This file is web-only and used to configure the root HTML for every
// web page during static rendering.
// The contents of this function only run in Node.js environments and
// do not have access to the DOM or browser APIs.
export default function Root({ children }: PropsWithChildren) {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta httpEquiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        {/* Bootstrap the service worker. */}
        <script dangerouslySetInnerHTML={{ __html: sw }} />

        {/*
          Disable body scrolling on web. This makes ScrollView components work closer to how they do on native.
          However, body scrolling is often nice to have for mobile web. If you want to enable it, remove this line.
        */}
        <ScrollViewStyleReset />

        {/* Add any additional <head> elements that you want globally available on web... */}
      </head>
      <body>{children}</body>
    </html>
  );
}

const sw = `
if ('serviceWorker' in navigator) {
    window.addEventListener('load', () => {
        navigator.serviceWorker.register('/sw.js').then(registration => {
            console.log('Service Worker registered with scope:', registration.scope);
        }).catch(error => {
            console.error('Service Worker registration failed:', error);
        });
    });
}
`;
```

Now build the website before running the wizard:

```sh
# npm
npx expo export -p web

# yarn
yarn expo export -p web

# pnpm
pnpm expo export -p web

# bun
bun expo export -p web
```

Run the wizard command, select `dist` as the root of the app, and the defaults for everything else:

```sh
# npm
npx workbox-cli wizard
? What is the root of your web app (that is which directory do you deploy)? dist/
? Which file types would you like to precache? js, html, ttf, ico, json
? Where would you like your service worker file to be saved? dist/sw.js
? Where would you like to save these configuration options? workbox-config.js
? Does your web app manifest include search parameter(s) in the 'start_url', other than 'utm_' or 'fbclid' (like '?source=pwa')? No

# yarn
yarn dlx workbox-cli wizard
? What is the root of your web app (that is which directory do you deploy)? dist/
? Which file types would you like to precache? js, html, ttf, ico, json
? Where would you like your service worker file to be saved? dist/sw.js
? Where would you like to save these configuration options? workbox-config.js
? Does your web app manifest include search parameter(s) in the 'start_url', other than 'utm_' or 'fbclid' (like '?source=pwa')? No

# pnpm
pnpm dlx workbox-cli wizard
? What is the root of your web app (that is which directory do you deploy)? dist/
? Which file types would you like to precache? js, html, ttf, ico, json
? Where would you like your service worker file to be saved? dist/sw.js
? Where would you like to save these configuration options? workbox-config.js
? Does your web app manifest include search parameter(s) in the 'start_url', other than 'utm_' or 'fbclid' (like '?source=pwa')? No

# bun
bunx workbox-cli wizard
? What is the root of your web app (that is which directory do you deploy)? dist/
? Which file types would you like to precache? js, html, ttf, ico, json
? Where would you like your service worker file to be saved? dist/sw.js
? Where would you like to save these configuration options? workbox-config.js
? Does your web app manifest include search parameter(s) in the 'start_url', other than 'utm_' or 'fbclid' (like '?source=pwa')? No
```

Finally, run `npx workbox-cli generateSW workbox-config.js` to generate the service worker config.

Going forward, you can add a build script in **package.json** to run both scripts in the correct order:

```json
{
  "scripts": {
    "build:web": "expo export -p web && npx workbox-cli generateSW workbox-config.js"
  }
}
```

If you host your website and visit with Chrome, you can inspect the service worker by going to **Application > Service Workers** in the Chrome DevTools.
