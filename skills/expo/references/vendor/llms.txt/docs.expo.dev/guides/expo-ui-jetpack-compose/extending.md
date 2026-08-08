---
modificationDate: May 05, 2026
title: Extending with Jetpack Compose
description: Learn how to create custom Jetpack Compose components and modifiers that integrate with Expo UI.
platforms: ['android']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/expo-ui-jetpack-compose/extending/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/expo-ui-jetpack-compose/extending/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Expo UI
Pages in this section:
- [Expo UI and SwiftUI](https://docs.expo.dev/guides/expo-ui-swift-ui.md)
- [Extending with SwiftUI](https://docs.expo.dev/guides/expo-ui-swift-ui/extending.md)
- [Extending with Jetpack Compose](https://docs.expo.dev/guides/expo-ui-jetpack-compose/extending.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Extending with Jetpack Compose

Learn how to create custom Jetpack Compose components and modifiers that integrate with Expo UI.
Android

This guide explains how to create custom Jetpack Compose components and modifiers that integrate seamlessly with Expo UI.

#### Prerequisites

##### @expo/ui installed

See [Building Jetpack Compose apps with Expo UI](/versions/latest/sdk/ui/jetpack-compose.md) for more information.

```sh
npx expo install @expo/ui
```

##### A development build of your app

Expo UI is not available in Expo Go. Create a [development build](/develop/development-builds/introduction.md) of your app.

##### Basic familiarity with Expo Modules API and Jetpack Compose

Familiarity with [Expo Modules API](/modules/overview.md) and [Jetpack Compose](https://developer.android.com/jetpack/compose).

## Creating a custom component

### Project setup

Create a local Expo module in your project:

```sh
npx create-expo-module@latest --local my-ui
```

Update your module's **android/build.gradle** to enable Jetpack Compose and depend on `expo-ui`. The lines marked below are added on top of the default scaffold:

```groovy
// Pull in the Kotlin Compose compiler plugin classpath.
buildscript {
  repositories {
    mavenCentral()
  }
  dependencies {
    classpath("org.jetbrains.kotlin.plugin.compose:org.jetbrains.kotlin.plugin.compose.gradle.plugin:${kotlinVersion}")
  }
}

apply plugin: 'com.android.library'
apply plugin: 'expo-module-gradle-plugin'
apply plugin: 'org.jetbrains.kotlin.plugin.compose' // Apply the Compose compiler plugin.

// ... group / version

android {
  // ... namespace, defaultConfig

  // Turn on Jetpack Compose for this module.
  buildFeatures {
    compose true
  }
}

// Depend on `expo-ui` plus the Compose libraries you use.
dependencies {
  if (findProject(':expo-ui') != null) {
    implementation project(':expo-ui')
  } else {
    implementation 'expo.modules.ui:expo.modules.ui:+'
  }
  implementation 'androidx.compose.foundation:foundation-android:1.10.6'
  implementation 'androidx.compose.ui:ui-android:1.10.6'
  implementation 'androidx.compose.material3:material3:1.5.0-alpha17'
}
```

### Creating a Compose view

Create your Compose view. It has two parts:

1.  **Props data class**: annotated with `@OptimizedComposeProps`, implements `ComposeProps`, and includes a `modifiers: ModifierList` field for the [`modifiers`](/versions/latest/sdk/ui/jetpack-compose/modifiers.md) prop.
2.  **`@Composable` content function**: an extension on `FunctionalComposableScope` so it can call `ModifierRegistry.applyModifiers(...)` and render `Children(...)`.

```kotlin
package expo.modules.myui

import androidx.compose.foundation.layout.Column
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import expo.modules.kotlin.views.ComposeProps
import expo.modules.kotlin.views.FunctionalComposableScope
import expo.modules.kotlin.views.OptimizedComposeProps
import expo.modules.ui.ModifierList
import expo.modules.ui.ModifierRegistry
import expo.modules.ui.UIComposableScope

@OptimizedComposeProps
data class MyCustomViewProps(
  val title: String = "",
  val modifiers: ModifierList = emptyList()
) : ComposeProps

@Composable
fun FunctionalComposableScope.MyCustomViewContent(props: MyCustomViewProps) {
  Column(
    modifier = ModifierRegistry.applyModifiers(
      props.modifiers,
      appContext,
      composableScope,
      globalEventDispatcher
    )
  ) {
    Text(text = props.title, style = MaterialTheme.typography.titleMedium)
    Children(UIComposableScope()) // Renders React children
  }
}
```

Register the view in your module using `ExpoUIView`. This wires your `@Composable` content into the Expo modules view system and makes it available to JavaScript:

```kotlin
package expo.modules.myui

import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition
import expo.modules.ui.ExpoUIView

class MyUiModule : Module() {
  override fun definition() = ModuleDefinition {
    Name("MyUi")

    ExpoUIView<MyCustomViewProps>("MyCustomView") {
      Content { props ->
        MyCustomViewContent(props)
      }
    }
  }
}
```

Create a wrapper component that connects modifiers with event handling. The `createViewModifierEventListener` utility enables event-based modifiers like `clickable` and `onVisibilityChanged` to work with your custom view:

```tsx
import { type PrimitiveBaseProps } from '@expo/ui/jetpack-compose';
import { createViewModifierEventListener } from '@expo/ui/jetpack-compose/modifiers';
import { requireNativeView } from 'expo';

export interface MyCustomViewProps extends PrimitiveBaseProps {
  title: string;
  children?: React.ReactNode;
}

const NativeMyCustomView = requireNativeView<MyCustomViewProps>('MyUi', 'MyCustomView');

export function MyCustomView({ modifiers, ...restProps }: MyCustomViewProps) {
  return (
    <NativeMyCustomView
      modifiers={modifiers}
      {...(modifiers ? createViewModifierEventListener(modifiers) : undefined)}
      {...restProps}
    />
  );
}
```

### Using your custom component

Your custom component now works with all `@expo/ui` built-in modifiers:

```tsx
import { Host, Text } from '@expo/ui/jetpack-compose';
import { background, clip, paddingAll } from '@expo/ui/jetpack-compose/modifiers';
import { MyCustomView } from './modules/my-ui';

export default function App() {
  return (
    <Host style={{ flex: 1 }}>
      <MyCustomView
        title="Hello World"
        modifiers={[
          paddingAll(16),
          background('#f0f0f0'),
          clip({ type: 'roundedCorner', radius: 12 }),
        ]}>
        <Text>Child content</Text>
      </MyCustomView>
    </Host>
  );
}
```

## Creating custom modifiers

You can also create custom modifiers that work with any Expo UI component.

> Modifiers are Compose's way to configure layouts for styling, sizing, behavior, and more. Learn more in Android's [Compose modifiers documentation](https://developer.android.com/jetpack/compose/modifiers).

### Native modifier implementation

Define your modifier's parameters as an `@OptimizedRecord` data class, and a function that returns a `Modifier` from those params:

```kotlin
package expo.modules.myui

import android.graphics.Color
import androidx.compose.foundation.BorderStroke
import androidx.compose.foundation.border
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import expo.modules.kotlin.records.Field
import expo.modules.kotlin.records.Record
import expo.modules.kotlin.types.OptimizedRecord
import expo.modules.ui.compose

@OptimizedRecord
data class CustomBorderParams(
  @Field val color: Color? = null,
  @Field val width: Int = 2,
  @Field val cornerRadius: Int = 0
) : Record

fun customBorderModifier(params: CustomBorderParams): Modifier {
  return Modifier.border(
    border = BorderStroke(params.width.dp, params.color.compose),
    shape = RoundedCornerShape(params.cornerRadius.dp)
  )
}
```

`compose` is a Kotlin extension property on `android.graphics.Color?` defined in the `expo.modules.ui` package. Importing it with `import expo.modules.ui.compose` lets you call `params.color.compose` to convert the Android `Color` parsed from JS into the `androidx.compose.ui.graphics.Color` that Compose APIs (like `BorderStroke`) expect. It's the same helper Expo UI's built-in modifiers use.

Register your modifier with `ModifierRegistry` in your module definition. Use `OnCreate` to register and `OnDestroy` to unregister so the factory does not leak across module reloads:

```kotlin
package expo.modules.myui

import expo.modules.kotlin.modules.Module
import expo.modules.kotlin.modules.ModuleDefinition
import expo.modules.kotlin.records.recordFromMap
import expo.modules.ui.ExpoUIView
import expo.modules.ui.ModifierRegistry

class MyUiModule : Module() {
  override fun definition() = ModuleDefinition {
    Name("MyUi")

    OnCreate {
      ModifierRegistry.register("customBorder") { map, _, _, _ ->
        customBorderModifier(recordFromMap<CustomBorderParams>(map))
      }
    }

    OnDestroy {
      ModifierRegistry.unregister("customBorder")
    }

    ExpoUIView<MyCustomViewProps>("MyCustomView") {
      Content { props ->
        MyCustomViewContent(props)
      }
    }
  }
}
```

The `register` lambda receives the raw map sent from JavaScript, the current `ComposableScope` (use it for scope-dependent modifiers like `weight` or `align`), the `AppContext`, and an event dispatcher. Most modifiers only need `map` and convert it via `recordFromMap<T>(map)`.

### JavaScript modifier function

Create a TypeScript function that builds the modifier config:

```ts
import { createModifier } from '@expo/ui/jetpack-compose/modifiers';
import { type ColorValue } from 'react-native';

export const customBorder = (params: {
  color?: ColorValue;
  width?: number;
  cornerRadius?: number;
}) => createModifier('customBorder', params);
```

Export the modifier from your module:

```ts
export { MyCustomView, type MyCustomViewProps } from './src/MyCustomView';
export { customBorder } from './src/modifiers';
```

### Using custom modifiers

Your custom modifier works with any `@expo/ui` component:

```tsx
import { Column, Host, Text } from '@expo/ui/jetpack-compose';
import { paddingAll } from '@expo/ui/jetpack-compose/modifiers';
import { customBorder } from './modules/my-ui';

export default function App() {
  return (
    <Host style={{ flex: 1 }}>
      <Column
        modifiers={[paddingAll(20), customBorder({ color: '#FF6B35', width: 3, cornerRadius: 8 })]}>
        <Text>This has a custom border!</Text>
      </Column>
    </Host>
  );
}
```

## Next steps

Congratulations! You've learned how to extend Expo UI with custom Jetpack Compose components and modifiers. Your custom components now integrate seamlessly with the built-in modifier system.

Here are some ideas for what to build next:

-   Use the [built-in Jetpack Compose components](/versions/latest/sdk/ui/jetpack-compose.md) that come with Expo UI.
-   Build custom modifiers for app-specific styling patterns.
-   Wrap third-party Compose libraries for use in React Native.
-   Share your components as an npm package for others to use.
