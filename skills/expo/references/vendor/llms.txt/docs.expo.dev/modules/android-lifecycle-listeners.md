---
modificationDate: July 21, 2026
title: Android lifecycle listeners
description: Learn about the mechanism that allows your library to hook into Android Activity and Application functions using Expo modules API.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/android-lifecycle-listeners/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/android-lifecycle-listeners/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Modules API > Reference
Pages in this section:
- [Module API](https://docs.expo.dev/modules/module-api.md)
- [Inline modules reference](https://docs.expo.dev/modules/inline-modules-reference.md)
- [Type generation reference](https://docs.expo.dev/modules/type-generation-reference.md)
- [Android lifecycle listeners](https://docs.expo.dev/modules/android-lifecycle-listeners.md) (this page)
- [iOS AppDelegate subscribers](https://docs.expo.dev/modules/appdelegate-subscribers.md)
- [Autolinking](https://docs.expo.dev/modules/autolinking.md)
- [Shared objects](https://docs.expo.dev/modules/shared-objects.md)
- [expo-module.config.json](https://docs.expo.dev/modules/module-config.md)
- [Mocking native calls](https://docs.expo.dev/modules/mocking.md)
- [Design considerations](https://docs.expo.dev/modules/design.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Android lifecycle listeners

Learn about the mechanism that allows your library to hook into Android Activity and Application functions using Expo modules API.

To respond to certain Android system events relevant to an app, such as inbound links and configuration changes, it is necessary to override the corresponding lifecycle callbacks in **MainActivity.java** and/or **MainApplication.java**.

The React Native module API does not provide any mechanism to hook into these, and so setup instructions for React Native libraries often include steps to copy code into these files. To simplify and automate setup and maintenance, the Expo Modules API provides a mechanism that allows your library to hook into `Activity` or `Application` functions.

## Get started

First, you need to have created an Expo module or integrated the Expo modules API in the library using the React Native module API. See [what is the Expo Modules API](/modules/overview.md#what-is-the-expo-modules-api).

Inside your module, create a concrete class that implements the [`Package`](https://github.com/expo/expo/tree/main/packages/expo-modules-core/android/src/main/java/expo/modules/core/interfaces/Package.java) interface. For most cases, you only need to implement the `createReactActivityLifecycleListeners` or `createApplicationLifecycleListeners` methods.

## `Activity` lifecycle listeners

You can hook into the `Activity` lifecycle using `ReactActivityLifecycleListener`. `ReactActivityLifecycleListener` hooks into React Native's `ReactActivity` lifecycle using its `ReactActivityDelegate` and provides a similar experience to the Android `Activity` lifecycle.

The following `Activity` lifecycle callbacks are currently supported:

-   `onCreate`
-   `onResume`
-   `onPause`
-   `onDestroy`
-   `onNewIntent`
-   `onBackPressed`

To create a `ReactActivityLifecycleListener`, you should implement `createReactActivityLifecycleListeners` in your derived `Package` class. For example, `MyLibPackage`.

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/mylib/MyLibPackage.kt
package expo.modules.mylib

import android.content.Context
import expo.modules.core.interfaces.Package
import expo.modules.core.interfaces.ReactActivityLifecycleListener

class MyLibPackage : Package {
  override fun createReactActivityLifecycleListeners(activityContext: Context): List<ReactActivityLifecycleListener> {
    return listOf(MyLibReactActivityLifecycleListener())
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/mylib/MyLibPackage.java
package expo.modules.mylib;

import android.content.Context;
import expo.modules.core.interfaces.Package;
import expo.modules.core.interfaces.ReactActivityLifecycleListener;

import java.util.Collections;
import java.util.List;

public class MyLibPackage implements Package {
  @Override
  public List<? extends ReactActivityLifecycleListener> createReactActivityLifecycleListeners(Context activityContext) {
    return Collections.singletonList(new MyLibReactActivityLifecycleListener());
  }
}
```

`MyLibReactActivityLifecycleListener` is a `ReactActivityLifecycleListener` derived class that you can hook into the lifecycles. You can only override the methods you need.

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/mylib/MyLibReactActivityLifecycleListener.kt
package expo.modules.mylib

import android.app.Activity
import android.os.Bundle
import expo.modules.core.interfaces.ReactActivityLifecycleListener

class MyLibReactActivityLifecycleListener : ReactActivityLifecycleListener {
  override fun onCreate(activity: Activity, savedInstanceState: Bundle?) {
    // Your setup code in `Activity.onCreate`.
    doSomeSetupInActivityOnCreate(activity)
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/mylib/MyLibReactActivityLifecycleListener.java
package expo.modules.mylib;

import android.app.Activity;
import android.os.Bundle;

import expo.modules.core.interfaces.ReactActivityLifecycleListener;

public class MyLibReactActivityLifecycleListener implements ReactActivityLifecycleListener {
  @Override
  public void onCreate(Activity activity, Bundle savedInstanceState) {
    // Your setup code in `Activity.onCreate`.
    doSomeSetupInActivityOnCreate(activity);
  }
}
```

You can also override other lifecycle methods. The example below shows how to override multiple lifecycle methods in a single listener class. It is based on `expo-linking` module, which uses different lifecycle methods to handle deep links. You can implement only the methods you need for your use case:

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/mylib/MyLibReactActivityLifecycleListener.kt
package expo.modules.mylib

import android.app.Activity
import android.content.Intent
import android.os.Bundle
import expo.modules.core.interfaces.ReactActivityLifecycleListener

class MyLibReactActivityLifecycleListener : ReactActivityLifecycleListener {
  override fun onCreate(activity: Activity?, savedInstanceState: Bundle?) {
    // Called when the activity is first created
    // Initialize your setup here, for example handling deep links
    val deepLinkUrl = activity?.intent?.data
    if (deepLinkUrl != null) {
      handleDeepLink(deepLinkUrl.toString())
    }
  }

  override fun onResume(activity: Activity) {
    // Called when the activity comes to the foreground
    // For example, track when user returns to the app
    trackAppStateChange("active")
  }

  override fun onPause(activity: Activity) {
    // Called when the activity goes to the background
    // For example, pause ongoing operations such as track analytics
    trackAppStateChange("inactive")
  }

  override fun onDestroy(activity: Activity) {
    // Called when the activity is being destroyed
    // Clean up resources here
    cleanup()
  }

  override fun onNewIntent(intent: Intent?): Boolean {
    // Called when app receives a new intent while already running
    // For example, handle new deep links while app is open
    val newUrl = intent?.data
    if (newUrl != null) {
      handleDeepLink(newUrl.toString())
      return true
    }
    return false
  }

  override fun onBackPressed(): Boolean {
    // Called when user presses the back button
    // Return true to prevent default back behavior
    return handleCustomBackNavigation()
  }

  // Now, you can add private functions to handle
  // your logic for deep links, app state tracking, clean up, and so on.
}
```

#### Java

```java
// android/src/main/java/expo/modules/mylib/MyLibReactActivityLifecycleListener.java
package expo.modules.mylib;

import android.app.Activity;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import expo.modules.core.interfaces.ReactActivityLifecycleListener;

public class MyLibReactActivityLifecycleListener implements ReactActivityLifecycleListener {
  @Override
  public void onCreate(Activity activity, Bundle savedInstanceState) {
    // Called when the activity is first created
    // Initialize your setup here, for example handling deep links
    Uri deepLinkUrl = activity.getIntent().getData();
    if (deepLinkUrl != null) {
      handleDeepLink(deepLinkUrl.toString());
    }
  }

  @Override
  public void onResume(Activity activity) {
    // Called when the activity comes to the foreground
    // For example, track when user returns to the app
    trackAppStateChange("active");
  }

  @Override
  public void onPause(Activity activity) {
    // Called when the activity goes to the background
    // For example, pause ongoing operations such as track analytics
    trackAppStateChange("inactive");
  }

  @Override
  public void onDestroy(Activity activity) {
    // Called when the activity is being destroyed
    // Clean up resources here
    cleanup();
  }

  @Override
  public boolean onNewIntent(Intent intent) {
    // Called when app receives a new intent while already running
    // For example, handle new deep links while app is open
    Uri newUrl = intent.getData();
    if (newUrl != null) {
      handleDeepLink(newUrl.toString());
      return true;
    }
    return false;
  }

  @Override
  public boolean onBackPressed() {
    // Called when user presses the back button
    // Return true to prevent default back behavior
    return handleCustomBackNavigation();
  }

  // Now, you can add private functions to handle
  // your logic for deep links, app state tracking, clean up, and so on.
}
```

## Lifecycle listeners to JavaScript event flow

Lifecycle listeners are singleton classes that exist independently of your Expo module instances. To communicate between a lifecycle listener and your module (for example, to send events to your app's JavaScript code), you need to observe events from your module and notify the lifecycle listener when events occur. A typical flow may consist of the following steps:

-   **System integration**: Lifecycle listeners capture Android intents with URL data
-   **Observer pattern**: Singleton lifecycle listeners communicate with module instances
-   **Event bridging**: Module sends structured events to JavaScript
-   **Memory management**: Weak references prevent memory leaks
-   **Type safety and React integration**: TypeScript support with proper event types and a custom hook provides easy access to deep link events

Your custom module implementation might not need all of the above from the event flow. However, you can adapt this pattern for other system events like app state changes, configuration changes, or custom business logic that needs to bridge Android lifecycle events to your React Native app.

The following example demonstrates how to use lifecycle listeners to bridge Android system events to your React Native app. It is based on [`expo-linking`](https://github.com/expo/expo/tree/main/packages/expo-linking), which uses lifecycle listeners to create a deep link handler that captures URLs when an app is opened or receives new intents.

### Module registration

Start by creating a module class that registers your lifecycle listener:

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/deeplinkhandler/DeepLinkHandlerPackage.kt
package expo.modules.deeplinkhandler

import android.content.Context
import expo.modules.core.interfaces.Package
import expo.modules.core.interfaces.ReactActivityLifecycleListener

class DeepLinkHandlerPackage : Package {
  override fun createReactActivityLifecycleListeners(activityContext: Context?): List<ReactActivityLifecycleListener> {
    return listOf(DeepLinkHandlerActivityLifecycleListener())
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/deeplinkhandler/DeepLinkHandlerPackage.java
package expo.modules.deeplinkhandler;

import android.content.Context;
import expo.modules.core.interfaces.Package;
import expo.modules.core.interfaces.ReactActivityLifecycleListener;
import java.util.Collections;
import java.util.List;

public class DeepLinkHandlerPackage implements Package {
  @Override
  public List<? extends ReactActivityLifecycleListener> createReactActivityLifecycleListeners(Context activityContext) {
    return Collections.singletonList(new DeepLinkHandlerActivityLifecycleListener());
  }
}
```

### Activity lifecycle listener with observer notifications

Create a lifecycle listener that captures deep links and notifies the module observers:

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/deeplinkhandler/DeepLinkHandlerActivityLifecycleListener.kt
package expo.modules.deeplinkhandler

import android.app.Activity
import android.content.Intent
import android.net.Uri
import android.os.Bundle
import expo.modules.core.interfaces.ReactActivityLifecycleListener

class DeepLinkHandlerActivityLifecycleListener : ReactActivityLifecycleListener {
  override fun onCreate(activity: Activity?, savedInstanceState: Bundle?) {
    handleIntent(activity?.intent)
  }

  override fun onNewIntent(intent: Intent?): Boolean {
    handleIntent(intent)
    return true
  }

  private fun handleIntent(intent: Intent?) {
    val url = intent?.data
    if (url != null) {
      // Store the initial URL for later retrieval
      DeepLinkHandlerModule.initialUrl = url

      // Notify all observers about the new deep link
      DeepLinkHandlerModule.urlReceivedObservers.forEach { observer ->
        observer(url)
      }
    }
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/deeplinkhandler/DeepLinkHandlerActivityLifecycleListener.java
package expo.modules.deeplinkhandler;

import android.app.Activity;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import expo.modules.core.interfaces.ReactActivityLifecycleListener;

public class DeepLinkHandlerActivityLifecycleListener implements ReactActivityLifecycleListener {
  @Override
  public void onCreate(Activity activity, Bundle savedInstanceState) {
    handleIntent(activity.getIntent());
  }

  @Override
  public boolean onNewIntent(Intent intent) {
    handleIntent(intent);
    return true;
  }

  private void handleIntent(Intent intent) {
    if (intent == null) return;

    Uri url = intent.getData();
    if (url != null) {
      // Store the initial URL for later retrieval
      DeepLinkHandlerModule.initialUrl = url;

      // Notify all observers about the new deep link
      for (java.util.function.Consumer<Uri> observer : DeepLinkHandlerModule.urlReceivedObservers) {
        observer.accept(url);
      }
    }
  }
}
```

### Expo module with event sending

Create a module that maintains observers and sends events to JavaScript:

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/deeplinkhandler/DeepLinkHandlerModule.kt
package expo.modules.deeplinkhandler

import android.net.Uri
import androidx.core.os.bundleOf
import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition
import java.lang.ref.WeakReference

class DeepLinkHandlerModule : Module() {
  companion object {
    var initialUrl: Uri? = null
    var urlReceivedObservers: MutableSet<((Uri) -> Unit)> = mutableSetOf()
  }

  private var urlReceivedObserver: ((Uri) -> Unit)? = null

  override fun definition() = ModuleDefinition {
    Name("DeepLinkHandler")

    Events("onUrlReceived")

    Function("getInitialUrl") {
      initialUrl?.toString()
    }

    OnStartObserving("onUrlReceived") {
      val weakModule = WeakReference(this@DeepLinkHandlerModule)
      val observer: (Uri) -> Unit = { uri ->
        weakModule.get()?.sendEvent(
          "onUrlReceived",
          bundleOf(
            "url" to uri.toString(),
            "scheme" to uri.scheme,
            "host" to uri.host,
            "path" to uri.path
          )
        )
      }
      urlReceivedObservers.add(observer)
      urlReceivedObserver = observer
    }

    OnStopObserving("onUrlReceived") {
      urlReceivedObservers.remove(urlReceivedObserver)
    }
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/deeplinkhandler/DeepLinkHandlerModule.java
package expo.modules.deeplinkhandler;

import android.net.Uri;
import androidx.core.os.Bundle;
import expo.modules.kotlin.modules.Module;
import expo.modules.kotlin.modules.ModuleDefinition;
import java.lang.ref.WeakReference;
import java.util.HashSet;
import java.util.Set;
import java.util.function.Consumer;

public class DeepLinkHandlerModule extends Module {
  public static Uri initialUrl = null;
  public static Set<Consumer<Uri>> urlReceivedObservers = new HashSet<>();

  private Consumer<Uri> urlReceivedObserver;

  @Override
  public ModuleDefinition definition() {
    return ModuleDefinition.create()
      .name("DeepLinkHandler")
      .events("onUrlReceived")
      .function("getInitialUrl", () -> {
        return initialUrl != null ? initialUrl.toString() : null;
      })
      .onStartObserving("onUrlReceived", () -> {
        WeakReference<DeepLinkHandlerModule> weakModule = new WeakReference<>(this);
        Consumer<Uri> observer = uri -> {
          DeepLinkHandlerModule module = weakModule.get();
          if (module != null) {
            Bundle bundle = new Bundle();
            bundle.putString("url", uri.toString());
            bundle.putString("scheme", uri.getScheme());
            bundle.putString("host", uri.getHost());
            bundle.putString("path", uri.getPath());
            module.sendEvent("onUrlReceived", bundle);
          }
        };
        urlReceivedObservers.add(observer);
        urlReceivedObserver = observer;
      })
      .onStopObserving("onUrlReceived", () -> {
        urlReceivedObservers.remove(urlReceivedObserver);
      });
  }
}
```

### TypeScript interface and React usage

Define a TypeScript interface for your module to bridge the Android lifecycle events to JavaScript:

```ts
import { requireNativeModule, NativeModule } from 'expo-modules-core';

export type DeepLinkEvent = {
  url: string;
  scheme?: string;
  host?: string;
  path?: string;
};

type DeepLinkHandlerModuleEvents = {
  onUrlReceived(event: DeepLinkEvent): void;
};

declare class DeepLinkHandlerNativeModule extends NativeModule<DeepLinkHandlerModuleEvents> {
  getInitialUrl(): string | null;
}

const DeepLinkHandler = requireNativeModule<DeepLinkHandlerNativeModule>('DeepLinkHandler');
export default DeepLinkHandler;
```

Create a React hook for an easy access to the deep link events:

```tsx
import { useEffect, useState } from 'react';
import DeepLinkHandler, { DeepLinkEvent } from './DeepLinkHandler';

export function useDeepLinkHandler(): {
  initialUrl: string | null;
  url: string | null;
  event: DeepLinkEvent | null;
} {
  const [initialUrl] = useState<string | null>(DeepLinkHandler.getInitialUrl());
  const [event, setEvent] = useState<DeepLinkEvent | null>(null);

  useEffect(() => {
    const subscription = DeepLinkHandler.addListener('onUrlReceived', event => {
      setEvent(event);
    });

    return () => subscription.remove();
  }, []);

  return {
    initialUrl,
    url: event?.url ?? initialUrl,
    event,
  };
}
```

Use it in your React component:

```tsx
import { Text, View, StyleSheet } from 'react-native';
import { useDeepLinkHandler } from './useDeepLinkHandler';

export function App() {
  const { initialUrl, url, event } = useDeepLinkHandler();

  return (
    <View style={styles.container}>
      <Text>Initial URL: {initialUrl || 'None'}</Text>
      <Text>Current URL: {url || 'None'}</Text>
      {event && (
        <View style={styles.textContainer}>
          <Text>Latest Deep Link:</Text>
          <Text>Scheme: {event.scheme}</Text>
          <Text>Host: {event.host}</Text>
          <Text>Path: {event.path}</Text>
        </View>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  textContainer: {
    marginTop: 20,
  },
});
```

### Module configuration

Finally, configure your module in **expo-module.config.json** to connect the module to the lifecycle listener:

```json
{
  "platforms": ["android"],
  "android": {
    "modules": ["expo.modules.deeplinkhandler.DeepLinkHandlerModule"]
  }
}
```

## `Application` lifecycle listeners

You can hook into the `Application` lifecycle using `ApplicationLifecycleListener`.

The following `Application` lifecycle callbacks are currently supported:

-   `onCreate`
-   `onConfigurationChanged`

To create an `ApplicationLifecycleListener`, you should implement `createApplicationLifecycleListeners` in your derived `Package` class. For example, `MyLibPackage`.

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/mylib/MyLibPackage.kt
package expo.modules.mylib

import android.content.Context
import expo.modules.core.interfaces.ApplicationLifecycleListener
import expo.modules.core.interfaces.Package

class MyLibPackage : Package {
  override fun createApplicationLifecycleListeners(context: Context): List<ApplicationLifecycleListener> {
    return listOf(MyLibApplicationLifecycleListener())
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/mylib/MyLibPackage.java
import android.content.Context;

import java.util.Collections;
import java.util.List;

import expo.modules.core.interfaces.ApplicationLifecycleListener;
import expo.modules.core.interfaces.Package;

public class MyLibPackage implements Package {
  @Override
  public List<? extends ApplicationLifecycleListener> createApplicationLifecycleListeners(Context context) {
    return Collections.singletonList(new MyLibApplicationLifecycleListener());
  }
}
```

`MyLibApplicationLifecycleListener` is an `ApplicationLifecycleListener` derived class that can hook into the `Application` lifecycle callbacks. You should only override the methods you need ([due to possible maintenance costs](/modules/android-lifecycle-listeners.md#interface-stability)).

#### Kotlin

```kotlin
// android/src/main/java/expo/modules/mylib/MyLibApplicationLifecycleListener.kt
package expo.modules.mylib

import android.app.Application
import expo.modules.core.interfaces.ApplicationLifecycleListener

class MyLibApplicationLifecycleListener : ApplicationLifecycleListener {
  override fun onCreate(application: Application) {
    // Your setup code in `Application.onCreate`.
    doSomeSetupInApplicationOnCreate(application)
  }
}
```

#### Java

```java
// android/src/main/java/expo/modules/mylib/MyLibApplicationLifecycleListener.java
package expo.modules.mylib;

import android.app.Application;

import expo.modules.core.interfaces.ApplicationLifecycleListener;

public class MyLibApplicationLifecycleListener implements ApplicationLifecycleListener {
  @Override
  public void onCreate(Application application) {
    // Your setup code in `Application.onCreate`.
    doSomeSetupInApplicationOnCreate(application);
  }
}
```

## Known issues

### Why there are no `onStart` and `onStop` Activity listeners

In the current implementation, we do not set up the hooks from `MainActivity` but from [`ReactActivityDelegate`](https://github.com/facebook/react-native/blob/400902093aa3ccfc05712a996c592a86f342253a/ReactAndroid/src/main/java/com/facebook/react/ReactActivityDelegate.java). There are some slight differences between `MainActivity` and `ReactActivityDelegate`. Since `ReactActivityDelegate` does not have `onStart` and `onStop`, we don't yet support them here.

### Interface stability

The listener interfaces may change from time to time between Expo SDK releases. Our strategy for backward compatibility is always to add new interfaces and add `@Deprecated` annotation for interfaces we plan to remove. Our interfaces are all based on Java 8 interface default method; you don't have to and should not implement all methods. Doing this will benefit your module's maintenance cost between Expo SDKs.
