---
modificationDate: July 27, 2026
title: 'Tutorial: Create a native view'
description: A tutorial on creating a native view that renders a WebView with Expo Modules API.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/native-view-tutorial/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/native-view-tutorial/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Modules API > Tutorials
Pages in this section:
- [Create a native module](https://docs.expo.dev/modules/native-module-tutorial.md)
- [Create a native view](https://docs.expo.dev/modules/native-view-tutorial.md) (this page)
- [Create an inline module](https://docs.expo.dev/modules/inline-modules-tutorial.md)
- [Generate module TS interface](https://docs.expo.dev/modules/type-generation-tutorial.md)
- [Create a module with a config plugin](https://docs.expo.dev/modules/config-plugin-and-native-module-tutorial.md)
- [How to use a standalone Expo module](https://docs.expo.dev/modules/use-standalone-expo-module-in-your-project.md)
- [Wrap third-party native libraries](https://docs.expo.dev/modules/third-party-library.md)
- [Integrate in an existing library](https://docs.expo.dev/modules/existing-library.md)
- [Additional platform support](https://docs.expo.dev/modules/additional-platform-support.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Tutorial: Create a native view

A tutorial on creating a native view that renders a WebView with Expo Modules API.

In this tutorial, you'll build an example module with a native view that renders a WebView. For Android, you'll use the [`WebView`](https://developer.android.com/reference/android/webkit/WebView) component, and for iOS, [`WKWebView`](https://developer.apple.com/documentation/webkit/wkwebview). Web support can be implemented using an [`iframe`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) and is left as an exercise for you.

## Initialize a new module

Create a new module by running the following command and name the example module `expo-web-view`:

```sh
# npm
npx create-expo-module expo-web-view

# yarn
yarn create expo-module expo-web-view

# pnpm
pnpm create expo-module expo-web-view

# bun
bun create expo-module expo-web-view
```

> Since this is an example library and won't be published, press Return for all prompts to accept the default values.

## Set up workspace

Clean up the default module to start with a clean slate by deleting the following files:

```sh
cd expo-web-view
rm src/ExpoWebView.types.ts src/ExpoWebViewModule.ts
rm src/ExpoWebView.web.tsx src/ExpoWebViewModule.web.ts
```

Locate the following files and replace them with the provided minimal boilerplate:

```kotlin
package expo.modules.webview

import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition

class ExpoWebViewModule : Module() {
  override fun definition() = ModuleDefinition {
    Name("ExpoWebView")

    View(ExpoWebView::class) {}
  }
}
```

```swift
import ExpoModulesCore

public class ExpoWebViewModule: Module {
  public func definition() -> ModuleDefinition {
    Name("ExpoWebView")

    View(ExpoWebView.self) {}
  }
}
```

```tsx
import { ViewProps } from 'react-native';
import { requireNativeViewManager } from 'expo-modules-core';
import * as React from 'react';

export type Props = ViewProps;

const NativeView: React.ComponentType<Props> = requireNativeViewManager('ExpoWebView');

export default function ExpoWebView(props: Props) {
  return <NativeView {...props} />;
}
```

```tsx
export { default as WebView, Props as WebViewProps } from './ExpoWebView';
```

```tsx
import { WebView } from 'expo-web-view';

export default function App() {
  return <WebView style={{ flex: 1, backgroundColor: 'purple' }} />;
}
```

## Run the example project

To ensure everything is working, start the TypeScript compiler to watch for changes and rebuild the module's JavaScript:

```sh
# npm
npm run build

# yarn
yarn run build

# pnpm
pnpm run build

# bun
bun run build
```

```sh
# npm
cd example
npx expo run:android
npx expo run:ios

# yarn
cd example
yarn expo run:android
yarn expo run:ios

# pnpm
cd example
pnpm expo run:android
pnpm expo run:ios

# bun
cd example
bun expo run:android
bun expo run:ios
```

You should now see a blank purple screen. While it's not very exciting, it's a good start. Next, turn it into a WebView.

## Add the system WebView as a subview

Add the system `WebView` with a hardcoded URL as a subview of `ExpoWebView`. The `ExpoWebView` class extends `ExpoView`, which extends `RCTView` from React Native, and eventually extends `View` on Android and `UIView` on iOS.

Ensure that the `WebView` subview uses the same layout as `ExpoWebView`, whose layout is calculated by React Native's layout engine.

### Android view

On Android, use `LayoutParams` to set the WebView's layout to match the `ExpoWebView` layout. You can do this when you instantiate the WebView.

```kotlin
package expo.modules.webview

import android.content.Context
import android.webkit.WebView
import android.webkit.WebViewClient
import expo.modules.kotlin.AppContext
import expo.modules.kotlin.views.ExpoView

class ExpoWebView(context: Context, appContext: AppContext) : ExpoView(context, appContext) {
  internal val webView = WebView(context).also {
    it.layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT)
    it.webViewClient = object : WebViewClient() {}
    addView(it)

    it.loadUrl("https://docs.expo.dev/modules/")
  }
}
```

### iOS view

On iOS, set `clipsToBounds` to `true` and ensure the WebView's `frame` matches the bounds of `ExpoWebView` in `layoutSubviews`. The `init` method is called when the view is created, and `layoutSubviews` is called when the layout changes.

```swift
import ExpoModulesCore
import WebKit

class ExpoWebView: ExpoView {
  let webView = WKWebView()

  required init(appContext: AppContext? = nil) {
    super.init(appContext: appContext)
    clipsToBounds = true
    addSubview(webView)

    let url =  URL(string:"https://docs.expo.dev/modules/")!
    let urlRequest = URLRequest(url:url)
    webView.load(urlRequest)
  }

  override func layoutSubviews() {
    webView.frame = bounds
  }
}
```

### Example app

No changes are required. Rebuild and run the app using the following commands:

```sh
# npm
npx expo prebuild --clean
npx expo run:android
npx expo run:ios

# yarn
yarn expo prebuild --clean
yarn expo run:android
yarn expo run:ios

# pnpm
pnpm expo prebuild --clean
pnpm expo run:android
pnpm expo run:ios

# bun
bun expo prebuild --clean
bun expo run:android
bun expo run:ios
```

After that, you'll see the [Expo Modules API overview page](/modules/overview.md) rendered. If the changes aren't reflected, try reinstalling the app.

## Add a prop to set the URL

To set a prop on the view, define the prop name and setter inside `ExpoWebViewModule`. In this case, you can access the `webView` property directly for convenience. However, in real-world scenarios, keep the logic inside the `ExpoWebView` class to minimize how much `ExpoWebViewModule` knows about its internals.

Use the [Prop definition component](/modules/module-api.md#prop) to define the prop. In the prop setter block, you can access both the view and the prop. Specify that the URL is of type `URL` — the Expo modules API will convert strings to the native `URL` type.

### Android module

```kotlin
package expo.modules.webview

import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition
import java.net.URL

class ExpoWebViewModule : Module() {
  override fun definition() = ModuleDefinition {
    Name("ExpoWebView")

    View(ExpoWebView::class) {
      Prop("url") { view: ExpoWebView, url: URL? ->
        view.webView.loadUrl(url.toString())
      }
    }
  }
}
```

### iOS module

```swift
import ExpoModulesCore

public class ExpoWebViewModule: Module {
  public func definition() -> ModuleDefinition {
    Name("ExpoWebView")

    View(ExpoWebView.self) {
      Prop("url") { (view, url: URL) in
        if view.webView.url != url {
          let urlRequest = URLRequest(url: url)
          view.webView.load(urlRequest)
        }
      }
    }
  }
}
```

### TypeScript module

Next, add the `url` prop to the `Props` type.

```tsx
import { ViewProps } from 'react-native';
import { requireNativeViewManager } from 'expo-modules-core';
import * as React from 'react';

export type Props = {
  url?: string;
} & ViewProps;

const NativeView: React.ComponentType<Props> = requireNativeViewManager('ExpoWebView');

export default function ExpoWebView(props: Props) {
  return <NativeView {...props} />;
}
```

### Example app

Finally, pass a `URL` to your `WebView` component in the example app.

```tsx
import { WebView } from 'expo-web-view';

export default function App() {
  return <WebView style={{ flex: 1 }} url="https://expo.dev" />;
}
```

Rebuild the example app:

```sh
# npm
npx expo prebuild --clean
npx expo run:android
npx expo run:ios

# yarn
yarn expo prebuild --clean
yarn expo run:android
yarn expo run:ios

# pnpm
pnpm expo prebuild --clean
pnpm expo run:android
pnpm expo run:ios

# bun
bun expo prebuild --clean
bun expo run:android
bun expo run:ios
```

After that, you'll see the [Expo homepage](https://expo.dev) in the WebView.

## Add an event to notify when the page has loaded

[View callbacks](/modules/module-api.md#view-callbacks) allow developers to listen for events on components. They are typically registered through props on the component, for example: `<Image onLoad={...} />`. Use the [Events definition component](/modules/module-api.md#events) to define an event for your WebView. Call it `onLoad`.

### Android view and module

On Android, override the `onPageFinished` function. Then, call the `onLoad` event handler that you defined in the module.

```kotlin
package expo.modules.webview

import android.content.Context
import android.webkit.WebView
import android.webkit.WebViewClient
import expo.modules.kotlin.AppContext
import expo.modules.kotlin.viewevent.EventDispatcher
import expo.modules.kotlin.views.ExpoView

class ExpoWebView(context: Context, appContext: AppContext) : ExpoView(context, appContext) {
  private val onLoad by EventDispatcher()

  internal val webView = WebView(context).also {
    it.layoutParams = LayoutParams(
      LayoutParams.MATCH_PARENT,
      LayoutParams.MATCH_PARENT
    )

    it.webViewClient = object : WebViewClient() {
      override fun onPageFinished(view: WebView, url: String) {
        onLoad(mapOf("url" to url))
      }
    }

    addView(it)
  }
}
```

Indicate in `ExpoWebViewModule` that the `View` has an `onLoad` event.

```kotlin
package expo.modules.webview

import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition
import java.net.URL

class ExpoWebViewModule : Module() {
  override fun definition() = ModuleDefinition {
    Name("ExpoWebView")

    View(ExpoWebView::class) {
      Events("onLoad")

      Prop("url") { view: ExpoWebView, url: URL? ->
        view.webView.loadUrl(url.toString())
      }
    }
  }
}
```

### iOS view and module

On iOS, implement `webView(_:didFinish:)` and make `ExpoWebView` extend `WKNavigationDelegate`. Then, call `onLoad` from that delegate method.

```swift
import ExpoModulesCore
import WebKit

class ExpoWebView: ExpoView, WKNavigationDelegate {
  let webView = WKWebView()
  let onLoad = EventDispatcher()

  required init(appContext: AppContext? = nil) {
    super.init(appContext: appContext)
    clipsToBounds = true
    webView.navigationDelegate = self
    addSubview(webView)
  }

  override func layoutSubviews() {
    webView.frame = bounds
  }

  func webView(_ webView: WKWebView, didFinish navigation: WKNavigation!) {
    if let url = webView.url {
      onLoad([
        "url": url.absoluteString
      ])
    }
  }
}
```

Indicate in `ExpoWebViewModule` that the `View` has an `onLoad` event.

```swift
import ExpoModulesCore

public class ExpoWebViewModule: Module {
  public func definition() -> ModuleDefinition {
    Name("ExpoWebView")

    View(ExpoWebView.self) {
      Events("onLoad")

      Prop("url") { (view, url: URL) in
        if view.webView.url != url {
          let urlRequest = URLRequest(url: url)
          view.webView.load(urlRequest)
        }
      }
    }
  }
}
```

### TypeScript module

Event payloads are included within the `nativeEvent` property of the event. To access the `url` from the `onLoad` event, read `event.nativeEvent.url`.

```tsx
import { ViewProps } from 'react-native';
import { requireNativeViewManager } from 'expo-modules-core';
import * as React from 'react';

export type OnLoadEvent = {
  url: string;
};

export type Props = {
  url?: string;
  onLoad?: (event: { nativeEvent: OnLoadEvent }) => void;
} & ViewProps;

const NativeView: React.ComponentType<Props> = requireNativeViewManager('ExpoWebView');

export default function ExpoWebView(props: Props) {
  return <NativeView {...props} />;
}
```

### Example app

Update the example app to show an alert when the page has loaded. Copy the following code, then rebuild and run your app, and you'll see the alert!

```tsx
import { WebView } from 'expo-web-view';

export default function App() {
  return (
    <WebView
      style={{ flex: 1 }}
      url="https://expo.dev"
      onLoad={event => alert(`loaded ${event.nativeEvent.url}`)}
    />
  );
}
```

## Bonus: Build a web browser UI around it

Now that you have a WebView, build a web browser UI around it. Try rebuilding a browser UI, and feel free to add new native capabilities as needed (for example, support for back or reload buttons). If you need inspiration, see the example below.

#### example/App.tsx

```tsx
import { useState } from 'react';
import { ActivityIndicator, Platform, Text, TextInput, View } from 'react-native';
import { WebView } from 'expo-web-view';

export default function App() {
  const [inputUrl, setInputUrl] = useState('https://docs.expo.dev/modules/');
  const [url, setUrl] = useState(inputUrl);
  const [isLoading, setIsLoading] = useState(true);

  return (
    <View style={{ flex: 1, paddingTop: Platform.OS === 'ios' ? 80 : 30 }}>
      <TextInput
        value={inputUrl}
        onChangeText={setInputUrl}
        returnKeyType="go"
        autoCapitalize="none"
        onSubmitEditing={() => {
          if (inputUrl !== url) {
            setUrl(inputUrl);
            setIsLoading(true);
          }
        }}
        keyboardType="url"
        style={{
          color: '#fff',
          backgroundColor: '#000',
          borderRadius: 10,
          marginHorizontal: 10,
          paddingHorizontal: 20,
          height: 60,
        }}
      />

      <WebView
        url={url.startsWith('https://') || url.startsWith('http://') ? url : `https://${url}`}
        onLoad={() => setIsLoading(false)}
        style={{ flex: 1, marginTop: 20 }}
      />
      <LoadingView isLoading={isLoading} />
    </View>
  );
}

function LoadingView({ isLoading }: { isLoading: boolean }) {
  if (!isLoading) {
    return null;
  }

  return (
    <View
      style={{
        position: 'absolute',
        bottom: 0,
        left: 0,
        right: 0,
        height: 80,
        backgroundColor: 'rgba(0,0,0,0.5)',
        paddingBottom: 10,
        justifyContent: 'center',
        alignItems: 'center',
        flexDirection: 'row',
      }}>
      <ActivityIndicator animating={isLoading} color="#fff" style={{ marginRight: 10 }} />
      <Text style={{ color: '#fff' }}>Loading...</Text>
    </View>
  );
}
```

Congratulations! You've created your first Expo module with a native view for Android and iOS.

## Next steps

[Expo Modules API Reference](/modules/module-api.md) — Create native modules using Kotlin and Swift.

[Tutorial: Creating a native module](/modules/native-module-tutorial.md) — A tutorial on creating a native module that persists settings with Expo Modules API.
