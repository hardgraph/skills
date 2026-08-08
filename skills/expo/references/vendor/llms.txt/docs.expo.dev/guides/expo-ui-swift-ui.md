---
modificationDate: July 28, 2026
title: Building SwiftUI apps with Expo UI
description: Learn how to use Expo UI to integrate SwiftUI into your Expo apps.
platforms: ['ios', 'macos', 'tvos']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/expo-ui-swift-ui/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/expo-ui-swift-ui/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Expo UI
Pages in this section:
- [Expo UI and SwiftUI](https://docs.expo.dev/guides/expo-ui-swift-ui.md) (this page)
- [Extending with SwiftUI](https://docs.expo.dev/guides/expo-ui-swift-ui/extending.md)
- [Extending with Jetpack Compose](https://docs.expo.dev/guides/expo-ui-jetpack-compose/extending.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Building SwiftUI apps with Expo UI

Learn how to use Expo UI to integrate SwiftUI into your Expo apps.
iOS, macOS, tvOS

> Available in **SDK 54 and later**.

Expo UI brings SwiftUI to React Native. You can use modern SwiftUI primitives to build your apps.

This guide covers the basics of using Expo UI to integrate SwiftUI into your Expo apps.

[Expo UI iOS Liquid Glass Tutorial](https://www.youtube.com/watch?v=2wXYLWz3YEQ) — Learn how to build real SwiftUI views in your React Native app with the new Expo UI.

## Features

-   **SwiftUI primitives**: Expo UI is not another UI library. It brings SwiftUI primitives to Expo.
-   **1-to-1 mapping**: The components in Expo UI have a 1-to-1 mapping to SwiftUI views. You can easily explore available views in the SwiftUI ecosystem, such as [Explore SwiftUI](https://exploreswiftui.com/) or the [Libraried app](https://apps.apple.com/us/app/libraried-ui-components/id1642862540), and find the corresponding Expo UI component.
-   **Full-app support**: Expo UI is designed to be used throughout the entire app. You can write your app entirely in Expo UI, while maintaining flexibility at the same time. The integration works at the component level. You can also mix [React Native components](https://reactnative.dev/docs/components-and-apis), [Expo UI components](/versions/latest/sdk/ui.md), [DOM components](/guides/dom-components.md), or custom 2D components using [`react-native-skia`](https://shopify.github.io/react-native-skia/).

## Installation

You'll need to install the `@expo/ui` package in your Expo project. Run the following command to install it:

```sh
npx expo install @expo/ui
```

## Expo Skills for AI agents

If you use an AI agent, install [Expo Skills](/skills.md) to teach it how to build native-feeling screens:

[expo-ui](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-ui/SKILL.md) — Build native UI with the @expo/ui package: real SwiftUI on iOS and Jetpack Compose on Android rendered from React in an Expo or React Native app.

[expo-native-ui](https://github.com/expo/skills/blob/main/plugins/expo/skills/expo-native-ui/SKILL.md) — Build beautiful, native-feeling Expo screens.

## Usage

Expo UI has several SwiftUI components available. You can use them in your app by importing them from `@expo/ui/swift-ui`. However, to cross the boundary from React Native (UIKit) to SwiftUI, you need to use the [`Host`](/versions/latest/sdk/ui/swift-ui/host.md) component. The [`Host`](/versions/latest/sdk/ui/swift-ui/host.md) is the container for SwiftUI views. You can think of it like [`<svg>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Element/svg) in the DOM or [`<Canvas>`](https://shopify.github.io/react-native-skia/docs/canvas/overview/) in [`react-native-skia`](https://shopify.github.io/react-native-skia/). Under the hood, it uses [`UIHostingController`](https://developer.apple.com/documentation/swiftui/uihostingcontroller) to render SwiftUI views in UIKit.

### Basic usage with `Host`

#### Code

```tsx
import { CircularProgress, Host } from '@expo/ui/swift-ui';
import { View, Text } from 'react-native';

export default function LoadingView() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Host matchContents>
        <CircularProgress />
      </Host>
      <Text>Loading...</Text>
    </View>
  );
}
```

#### Preview

### Using `HStack` and `VStack`

You can also use the `HStack` and `VStack` components to build the entire layout in SwiftUI.

#### Code

```tsx
import { CircularProgress, Host, HStack, LinearProgress, VStack } from '@expo/ui/swift-ui';

export default function LoadingView() {
  return (
    <Host style={{ flex: 1, margin: 32 }}>
      <VStack spacing={32}>
        <HStack spacing={32}>
          <CircularProgress />
          <CircularProgress color="orange" />
        </HStack>
        <LinearProgress progress={0.5} />
        <LinearProgress color="orange" progress={0.7} />
      </VStack>
    </Host>
  );
}
```

#### Preview

### Modifiers

[SwiftUI modifier](https://developer.apple.com/documentation/swiftui/view/modifier\(_:\)) is a powerful way to customize the appearance and behavior of SwiftUI components. Expo UI also provides modifiers for SwiftUI components. You can import modifiers from `@expo/ui/swift-ui/modifiers` and pass them as an array to the `modifiers` prop. In the following example, the [`expo-mesh-gradient`](/versions/latest/sdk/mesh-gradient.md) and `glassEffect` modifier are combined to create Liquid Glass text.

#### Code

> **Note**: `glassEffect` modifier requires Xcode 26+ and iOS 26+.

```tsx
import { Host, Text } from '@expo/ui/swift-ui';
import { glassEffect, padding } from '@expo/ui/swift-ui/modifiers';
import { MeshGradientView } from 'expo-mesh-gradient';
import { View } from 'react-native';

export default function Page() {
  return (
    <View style={{ flex: 1 }}>
      <MeshGradientView
        style={{ flex: 1 }}
        columns={3}
        rows={3}
        colors={['red', 'purple', 'indigo', 'orange', 'white', 'blue', 'yellow', 'green', 'cyan']}
        points={[
          [0.0, 0.0],
          [0.5, 0.0],
          [1.0, 0.0],
          [0.0, 0.5],
          [0.5, 0.5],
          [1.0, 0.5],
          [0.0, 1.0],
          [0.5, 1.0],
          [1.0, 1.0],
        ]}
      />
      <Host style={{ position: 'absolute', top: 0, right: 0, left: 0, bottom: 0 }}>
        <Text
          size={32}
          modifiers={[
            padding({
              all: 16,
            }),
            glassEffect({
              glass: {
                variant: 'clear',
              },
            }),
          ]}>
          Glass effect text
        </Text>
      </Host>
    </View>
  );
}
```

#### Preview

### iOS Settings app example

Combining the Expo UI components and modifiers, you can build a UI like iOS Settings app.

#### Code

```tsx
import {
  Button,
  Form,
  Host,
  HStack,
  Image,
  Section,
  Spacer,
  Toggle,
  Text,
} from '@expo/ui/swift-ui';
import { background, buttonStyle, foregroundStyle, clipShape, frame } from '@expo/ui/swift-ui/modifiers';
import { Link } from 'expo-router';
import { useState } from 'react';

export default function SettingsView() {
  const [isAirplaneMode, setIsAirplaneMode] = useState(true);

  return (
    <Host style={{ flex: 1 }}>
      <Form>
        <Section>
          <HStack spacing={8}>
            <Image
              systemName="airplane"
              color="white"
              size={18}
              modifiers={[
                frame({ width: 28, height: 28 }),
                background('#ffa500'),
                clipShape('roundedRectangle'),
              ]}
            />
            <Text>Airplane Mode</Text>
            <Spacer />
            <Toggle isOn={isAirplaneMode} onIsOnChange={setIsAirplaneMode} />
          </HStack>

          <Link href="/wifi" asChild>
            {/* Use buttonStyle('plain') to prevent default blue button styling */}
            <Button modifiers={[buttonStyle('plain')]}>
              <HStack spacing={8}>
                <Image
                  systemName="wifi"
                  color="white"
                  size={18}
                  modifiers={[
                    frame({ width: 28, height: 28 }),
                    background('#007aff'),
                    clipShape('roundedRectangle'),
                  ]}
                />
                {/* When Text is wrapped in a Link, the color needs to be specified explicitly */}
                <Text modifiers={[foregroundStyle({type: 'color', color: 'black'})]}>Wi-Fi</Text>
                <Spacer />
                <Image systemName="chevron.right" size={14} color="secondary" />
              </HStack>
            </Button>
          </Link>
        </Section>
      </Form>
    </Host>
  );
}
```

#### Preview

### Secondary text styling

Use `foregroundStyle` to apply a [hierarchical style](/versions/latest/sdk/ui/swift-ui/modifiers.md#foregroundstylestyle), which will make text appear lighter and more subtle.

#### Code

```tsx
import { Button, Form, Host, HStack, Image, List, Section, Spacer, Text } from '@expo/ui/swift-ui';
import { buttonStyle, font, foregroundStyle, padding } from '@expo/ui/swift-ui/modifiers';

export default function SecondaryTextExample() {
  return (
    <Host style={{ flex: 1 }}>
      <Form>
        <Section>
          <List>
            <Button onPress={() => console.log('Navigate')} modifiers={[buttonStyle('plain')]}>
              <HStack>
                <Text>Night Shift</Text>
                <Spacer />
                <Text
                  modifiers={[
                    foregroundStyle({type: 'hierarchical', style: 'secondary'}),
                    padding({ trailing: 8 }),
                  ]}>
                  22:00 to 07:00
                </Text>
                <Image systemName="chevron.right" size={14} color="#C7C7CC" />
              </HStack>
            </Button>
          </List>
          <List>
            <Text modifiers={[foregroundStyle({type: 'hierarchical', style: 'secondary'}), font({ size: 14 })]}>
              Save up to 280.7 MB. This will permanently delete all photos and videos kept in the
              "Recently Deleted" album.
            </Text>
          </List>
        </Section>
      </Form>
    </Host>
  );
}
```

#### Preview

### Slider with icons

A common pattern for brightness or volume controls is to flank a `Slider` with icons.

#### Code

```tsx
import { useState } from 'react';
import {
  Form,
  Host,
  HStack,
  Image,
  List,
  Section,
  Slider,
  Spacer,
  Text,
  Toggle,
} from '@expo/ui/swift-ui';
import { padding } from '@expo/ui/swift-ui/modifiers';

export default function SliderWithIconsExample() {
  const [brightness, setBrightness] = useState(0.5);
  const [trueToneEnabled, setTrueToneEnabled] = useState(true);

  return (
    <Host style={{ flex: 1 }}>
      <Form>
        <Section
          header={<Text>Brightness</Text>}
          footer={
            <Text>
              Automatically adapt iPhone display based on ambient lighting
              conditions to make colors appear consistent in different
              environments.
            </Text>
          }
        >
          <List>
            <HStack modifiers={[padding({ vertical: 6 })]}>
              <Image systemName="sun.min.fill" size={22} color="#8E8E93" />
              <Spacer />
              <Slider value={brightness} onValueChange={setBrightness} />
              <Spacer />
              <Image systemName="sun.max.fill" size={22} color="#8E8E93" />
            </HStack>
            <Toggle
              label="True Tone"
              isOn={trueToneEnabled}
              onIsOnChange={setTrueToneEnabled}
            />
          </List>
        </Section>
      </Form>
    </Host>
  );
}
```

#### Preview

### Multi-line list items

Use `VStack` with `alignment="leading"` for list items with title and subtitle.

#### Code

```tsx
import {
  Button,
  Form,
  Host,
  HStack,
  Image,
  List,
  Section,
  Spacer,
  Text,
  VStack,
} from '@expo/ui/swift-ui';
import { buttonStyle, font, foregroundStyle, padding } from '@expo/ui/swift-ui/modifiers';

export default function MultiLineListItemExample() {
  return (
    <Host style={{ flex: 1 }}>
      <Form>
        <Section>
          <List>
            <HStack>
              <Image
                systemName="safari"
                size={22}
                modifiers={[padding({ trailing: 6 })]}
              />
              <Spacer />
              <Button
                onPress={() => console.log('Navigate')}
                modifiers={[buttonStyle('plain'), padding({ vertical: 6 })]}
              >
                <VStack spacing={4} alignment="leading">
                  <Text>Chrome</Text>
                  <Text modifiers={[foregroundStyle({type: 'hierarchical', style: 'secondary'}), font({ size: 14 })]}>
                    Last used: Today
                  </Text>
                </VStack>
                <Spacer />
                <Text
                  modifiers={[
                    foregroundStyle({type: 'hierarchical', style: 'secondary'}),
                    font({ size: 16 }),
                  ]}
                >
                  1.57 GB
                </Text>
                <Image systemName="chevron.right" size={14} color="#C7C7CC" />
              </Button>
            </HStack>
          </List>
        </Section>
      </Form>
    </Host>
  );
}
```

#### Preview

## Common questions

#### Can I use flexbox or other styles in SwiftUI components?

Flexbox styles can be applied to the `Host` component itself. Once you're inside the SwiftUI context, however, [`Yoga`](https://www.yogalayout.dev/) is not available — layouts should be defined using `<HStack>` and `<VStack>` instead.

#### What's the Host component?

`Host` is the container for SwiftUI views. You can think of it like [`<svg>`](https://developer.mozilla.org/en-US/docs/Web/SVG/Reference/Element/svg) in the DOM or [`<Canvas>`](https://shopify.github.io/react-native-skia/docs/canvas/overview/) in [`react-native-skia`](https://shopify.github.io/react-native-skia/). Under the hood, it uses [`UIHostingController`](https://developer.apple.com/documentation/swiftui/uihostingcontroller) to render SwiftUI views in UIKit.

#### How is Expo UI different from libraries like react-native-paper or react-native-elements?

Expo UI is not "yet another" UI library and not an opinionated design kit. Instead, it's a primitives library. It exposes native SwiftUI and Jetpack Compose components directly to JavaScript, rather than re-implementing or simulating UI in JavaScript.

#### Can I use @expo/ui/swift-ui on Android or web?

The first milestone for Expo UI is achieving a 1-to-1 mapping from SwiftUI to Expo UI. Universal support will come in the next stage of the roadmap. Our priority is to establish strong SwiftUI support first, and then expand to Jetpack Compose on Android and DOM support on the Web.

#### Can I use React Native components inside SwiftUI components?

Yes, you can place React Native components as JSX children of Expo UI components. Expo UI automatically creates a [`UIViewRepresentable`](https://developer.apple.com/documentation/swiftui/uiviewrepresentable) wrapper for you. However, keep in mind that the SwiftUI layout system works differently from UIKit and has some limitations. According to Apple's documentation:

> SwiftUI fully controls the layout of the UIKit view's [`center`](https://developer.apple.com/documentation/UIKit/UIView/center), [`bounds`](https://developer.apple.com/documentation/UIKit/UIView/bounds), [`frame`](https://developer.apple.com/documentation/UIKit/UIView/frame), and [`transform`](https://developer.apple.com/documentation/UIKit/UIView/transform) properties. Don't directly set these layout-related properties on the view managed by a [`UIViewRepresentable`](https://developer.apple.com/documentation/swiftui/uiviewrepresentable) instance from your own code because that conflicts with SwiftUI and results in undefined behavior.

Also note that once you render React Native components, you're leaving the SwiftUI context. If you want to add Expo UI components again, you'll need to reintroduce a `Host` wrapper.

We recommend keeping SwiftUI layouts self-contained. Interop is possible, but it works best when boundaries are clearly defined.

#### I'm a SwiftUI developer. Why should I learn Expo UI?

Because React's promise of _"learn once, write anywhere"_, it now extends to SwiftUI and Jetpack Compose. With Expo UI, you can apply your SwiftUI knowledge to build apps that run in the React Native ecosystem, extend to the Web through [DOM components](/guides/dom-components.md), and even integrate [2D](https://shopify.github.io/react-native-skia/) and [3D](https://github.com/wcandillon/react-native-webgpu) rendering. The system is flexible enough that different parts of your app can use different approaches — giving you seamless integration at the component level.

## Additional resources

[Expo UI reference](/versions/latest/sdk/ui.md) — For information on API components, methods, and more, see the Expo UI reference.

[Expo UI example](https://github.com/expo/expo/tree/main/apps/native-component-list/src/screens/UI) — Our latest Expo UI examples.

[Hot Chocolate app example](https://github.com/expo/hot-chocolate) — An example app replicating the YVR Hot Chocolate Fest app with Expo UI.

[Expo UI multiplatform demo](https://github.com/react-native-tvos/ExpoUITV) — A project demonstrating the production-ready Expo UI package available in SDK 56 and later. The project works on both TV (Android TV, Apple TV) and mobile (Android, iOS). Demo screens include most of the available components for both Jetpack Compose and Swift UI.
