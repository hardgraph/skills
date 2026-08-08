---
modificationDate: July 20, 2026
title: Color themes
description: Learn how to support light and dark modes in your app.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/develop/user-interface/color-themes/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/develop/user-interface/color-themes/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Home > Develop > User interface
Pages in this section:
- [Splash screen and app icon](https://docs.expo.dev/develop/user-interface/splash-screen-and-app-icon.md)
- [Safe areas](https://docs.expo.dev/develop/user-interface/safe-areas.md)
- [System bars](https://docs.expo.dev/develop/user-interface/system-bars.md)
- [Fonts](https://docs.expo.dev/develop/user-interface/fonts.md)
- [Assets](https://docs.expo.dev/develop/user-interface/assets.md)
- [Color themes](https://docs.expo.dev/develop/user-interface/color-themes.md) (this page)
- [Animation](https://docs.expo.dev/develop/user-interface/animation.md)
- [Store data](https://docs.expo.dev/develop/user-interface/store-data.md)
- [Next steps](https://docs.expo.dev/develop/user-interface/next-steps.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Color themes

Learn how to support light and dark modes in your app.

It's common for apps to support light and dark color schemes. Here is an example of how supporting both modes looks in an Expo project:

## Configuration

> For Android and iOS projects, additional configuration is required to support switching between light and dark mode. For web, no additional configuration is required.

To configure supported appearance styles, you can use the [`userInterfaceStyle`](/versions/latest/config/app.md#userinterfacestyle) property in your project's [app config](/versions/latest/config/app.md). By default, this property is set to `automatic` when you create a new project with the [default template](/get-started/create-a-project.md).

Here is an example configuration:

```json
{
  "expo": {
    "userInterfaceStyle": "automatic"
  }
}
```

You can also configure `userInterfaceStyle` property for a specific platforms by setting either [`android.userInterfaceStyle`](/versions/latest/config/app.md#userinterfacestyle-2) or [`ios.userInterfaceStyle`](/versions/latest/config/app.md#userinterfacestyle-1) to the preferred value.

> The app will default to the `light` style if this property is absent.

When you are creating a development build, you have to install [`expo-system-ui`](/versions/latest/sdk/system-ui.md#installation) to support the appearance styles for Android. Otherwise, the `userInterfaceStyle` property is ignored.

```sh
# npm
npx expo install expo-system-ui

# yarn
yarn expo install expo-system-ui

# pnpm
pnpm expo install expo-system-ui

# bun
bun expo install expo-system-ui
```

If the project is misconfigured and doesn't have `expo-system-ui` installed, the following warning will be shown in the terminal:

```sh
» android: userInterfaceStyle: Install expo-system-ui in your project to enable this feature.
```

You can also use the following command to check if the project is misconfigured:

```sh
# npm
npx expo config --type introspect

# yarn
yarn expo config --type introspect

# pnpm
pnpm expo config --type introspect

# bun
bun expo config --type introspect
```

#### Using an existing React Native project?

### Android

Ensure that the `uiMode` flag is present on your `MainActivity` (and any other activities where this behavior is desired) in **AndroidManifest.xml**:

```xml
<activity android:configChanges="keyboard|keyboardHidden|orientation|screenSize|uiMode">
```

Implement the `onConfigurationChanged` method in **MainActivity.java**:

```java
import android.content.Intent;
import android.content.res.Configuration;
public class MainActivity extends ReactActivity {
  ... 

  @Override
  public void onConfigurationChanged(Configuration newConfig) {
    super.onConfigurationChanged(newConfig);
    Intent intent = new Intent("onConfigurationChanged");
    intent.putExtra("newConfig", newConfig);
    sendBroadcast(intent);
  }
  ... 
}
```

### iOS

You can configure supported styles with the [`UIUserInterfaceStyle`](https://developer.apple.com/documentation/bundleresources/information_property_list/uiuserinterfacestyle) key in your app **Info.plist**. Use `Automatic` to support both light and dark modes.

### Supported appearance styles

The `userInterfaceStyle` property supports the following values:

-   `automatic`: Follow system appearance settings and notify about any change the user makes.
-   `light`: Restrict the app to support light theme only.
-   `dark`: Restrict the app to support dark theme only.

## Detect the color scheme

To detect the color scheme in your project, use `Appearance` or `useColorScheme` from `react-native`:

```tsx
import { Appearance, useColorScheme } from 'react-native';
```

Then, you can use `useColorScheme()` hook as shown below:

```tsx
function MyComponent() {
  let colorScheme = useColorScheme();

  if (colorScheme === 'dark') {
    // render some dark thing
  } else {
    // render some light thing
  }
}
```

In some cases, you will find it helpful to get the current color scheme imperatively with [`Appearance.getColorScheme()` or listen to changes with `Appearance.addChangeListener()`](https://reactnative.dev/docs/appearance).

## Additional information

### Minimal example

```tsx
import { Text, StyleSheet, View, useColorScheme } from 'react-native';
import { StatusBar } from 'expo-status-bar';

export default function App() {
  const colorScheme = useColorScheme();

  const themeTextStyle = colorScheme === 'light' ? styles.lightThemeText : styles.darkThemeText;
  const themeContainerStyle =
    colorScheme === 'light' ? styles.lightContainer : styles.darkContainer;

  return (
    <View style={[styles.container, themeContainerStyle]}>
      <Text style={[styles.text, themeTextStyle]}>Color scheme: {colorScheme}</Text>
      <StatusBar />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  text: {
    fontSize: 20,
  },
  lightContainer: {
    backgroundColor: '#d0d0c0',
  },
  darkContainer: {
    backgroundColor: '#242c40',
  },
  lightThemeText: {
    color: '#242c40',
  },
  darkThemeText: {
    color: '#d0d0c0',
  },
});
```

### Tips

While you are developing your project, you can change your simulator's or device's appearance by using the following shortcuts:

-   If using an Android Emulator, you can run `adb shell "cmd uimode night yes"` to enable dark mode, and `adb shell "cmd uimode night no"` to disable dark mode.
-   If using a physical Android device or an Android Emulator, you can toggle the system dark mode setting in the device's settings.
-   If working with an iOS emulator locally, you can use the Cmd ⌘ + Shift + A shortcut to toggle between light and dark modes.
