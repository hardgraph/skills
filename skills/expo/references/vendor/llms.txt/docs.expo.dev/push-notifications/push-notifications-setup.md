---
modificationDate: July 28, 2026
title: Expo push notifications setup
description: Learn how to set up push notifications, get credentials for development and production, and send a testing push notification.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/push-notifications/push-notifications-setup/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/push-notifications/push-notifications-setup/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Push notifications
Pages in this section:
- [Overview](https://docs.expo.dev/push-notifications/overview.md)
- [About notification types](https://docs.expo.dev/push-notifications/what-you-need-to-know.md)
- [Expo push notifications setup](https://docs.expo.dev/push-notifications/push-notifications-setup.md) (this page)
- [Send notifications with the Expo Push Service](https://docs.expo.dev/push-notifications/sending-notifications.md)
- [Handle incoming notifications](https://docs.expo.dev/push-notifications/receiving-notifications.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo push notifications setup

Learn how to set up push notifications, get credentials for development and production, and send a testing push notification.

To utilize Expo push notification service, you must configure your app by installing a set of libraries, implement functions to handle notifications, and set up credentials for Android and iOS.

Complete the steps outlined in this guide or follow the more detailed video below. At the end, you'll be able to send a push notification and receive it on a device.

[Expo Notifications with EAS | Complete Guide](https://www.youtube.com/watch?v=BCCjGtKtBjE) — Learn how to set up push notifications in an Expo project. This video covers configuring Firebase for FCM v1 on Android, setting up Android and iOS credentials on EAS, building with EAS Build, and testing with Expo Notifications tool.

  

To get the client-side ready for push notifications, the following things are required:

-   The user's permission to send them push notifications.
-   The app's [`ExpoPushToken`](/versions/latest/sdk/notifications.md#expopushtoken).

  

#### Do you want to use FCM / APNs directly, instead of the Expo push notification service?

If you need finer-grained control over your notifications, communicating directly with FCM and APNs may be necessary. Expo does not lock you into using Expo Application Services, and the `expo-notifications` API is push-service agnostic. Learn how to ["Send notifications with FCM and APNs"](/push-notifications/sending-notifications-custom.md).

#### Prerequisites

##### A device or emulator/simulator that supports push

You can test push notifications on a physical Android or iOS device, on an Android Emulator with Google Play services, or on an iOS Simulator running on Xcode 14 or later (macOS 13+, iOS 16+).

The following steps in this guide use [EAS Build](/build/introduction.md). This is the easiest way to set up notifications since your EAS project will also contain the [notification credentials](/push-notifications/push-notifications-setup.md#get-credentials-for-development-builds). However, you can use the `expo-notifications` library without EAS Build by building [your project locally](/guides/local-app-development.md).

## Install libraries

Run the following command to install the `expo-notifications` and `expo-constants` libraries:

```sh
# npm
npx expo install expo-notifications expo-constants

# yarn
yarn expo install expo-notifications expo-constants

# pnpm
pnpm expo install expo-notifications expo-constants

# bun
bun expo install expo-notifications expo-constants
```

-   [`expo-notifications`](/versions/latest/sdk/notifications.md) library is used to request a user's permission and to obtain the `ExpoPushToken` for sending push notifications.
-   [`expo-constants`](/versions/latest/sdk/constants.md) is used to get the `projectId` value from the app config.

## Add config plugin

Add the `expo-notifications` plugin in the `plugins` array of your [app config](/workflow/configuration.md):

```json
{
  "expo": {
    ... 
    "plugins": [
      ... 
      "expo-notifications"
      ]
  }
}
```

## Add a minimal working example

The code below shows a working example of how to register for, send, and receive push notifications in a React Native app. Copy and paste it into your project:

```tsx
import { useState, useEffect } from 'react';
import { Text, View, Button, Platform } from 'react-native';
import * as Notifications from 'expo-notifications';
import Constants from 'expo-constants';

Notifications.setNotificationHandler({
  handleNotification: async () => ({
    shouldPlaySound: true,
    shouldSetBadge: true,
    shouldShowBanner: true,
    shouldShowList: true,
  }),
});

async function sendPushNotification(expoPushToken: string) {
  const message = {
    to: expoPushToken,
    sound: 'default',
    title: 'Original Title',
    body: 'And here is the body!',
    data: { someData: 'goes here' },
  };

  await fetch('https://exp.host/--/api/v2/push/send', {
    method: 'POST',
    headers: {
      Accept: 'application/json',
      'Accept-encoding': 'gzip, deflate',
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(message),
  });
}

function handleRegistrationError(errorMessage: string) {
  alert(errorMessage);
  throw new Error(errorMessage);
}

async function registerForPushNotificationsAsync() {
  if (Platform.OS === 'android') {
    await Notifications.setNotificationChannelAsync('default', {
      name: 'default',
      importance: Notifications.AndroidImportance.MAX,
      vibrationPattern: [0, 250, 250, 250],
      lightColor: '#FF231F7C',
    });
  }

  const { status: existingStatus } = await Notifications.getPermissionsAsync();
  let finalStatus = existingStatus;
  if (existingStatus !== 'granted') {
    const { status } = await Notifications.requestPermissionsAsync();
    finalStatus = status;
  }
  if (finalStatus !== 'granted') {
    handleRegistrationError('Permission not granted to get push token for push notification!');
    return;
  }
  const projectId = Constants?.expoConfig?.extra?.eas?.projectId ?? Constants?.easConfig?.projectId;
  if (!projectId) {
    handleRegistrationError('Project ID not found');
  }
  try {
    const pushTokenString = (
      await Notifications.getExpoPushTokenAsync({
        projectId,
      })
    ).data;
    console.log(pushTokenString);
    return pushTokenString;
  } catch (e: unknown) {
    handleRegistrationError(`${e}`);
  }
}

export default function App() {
  const [expoPushToken, setExpoPushToken] = useState('');
  const [notification, setNotification] = useState<Notifications.Notification | undefined>(
    undefined
  );

  useEffect(() => {
    registerForPushNotificationsAsync()
      .then(token => setExpoPushToken(token ?? ''))
      .catch((error: any) => setExpoPushToken(`${error}`));

    const notificationListener = Notifications.addNotificationReceivedListener(notification => {
      setNotification(notification);
    });

    const responseListener = Notifications.addNotificationResponseReceivedListener(response => {
      console.log(response);
    });

    return () => {
      notificationListener.remove();
      responseListener.remove();
    };
  }, []);

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'space-around' }}>
      <Text>Your Expo push token: {expoPushToken}</Text>
      <View style={{ alignItems: 'center', justifyContent: 'center' }}>
        <Text>Title: {notification && notification.request.content.title} </Text>
        <Text>Body: {notification && notification.request.content.body}</Text>
        <Text>Data: {notification && JSON.stringify(notification.request.content.data)}</Text>
      </View>
      <Button
        title="Press to Send Notification"
        onPress={async () => {
          await sendPushNotification(expoPushToken);
        }}
      />
    </View>
  );
}
```

### Configure `projectId`

Using the previous example, when you are registering for push notifications, you need to use [`projectId`](/versions/latest/sdk/constants.md#easconfig). This property is used to attribute Expo push token to the specific project. For projects using EAS, the `projectId` property represents the Universally Unique Identifier (UUID) of that project.

`projectId` is set automatically when you create a development build. However, **we recommend setting it manually in your project's code**. To do so, you can use [`expo-constants`](/versions/latest/sdk/constants.md) to get the `projectId` value from the app config.

```ts
const projectId = Constants?.expoConfig?.extra?.eas?.projectId ?? Constants?.easConfig?.projectId;
const pushTokenString = (await Notifications.getExpoPushTokenAsync({ projectId })).data;
```

One advantage of attributing the Expo push token to your project's ID is that it doesn't change when a project is transferred between different accounts or the existing account gets renamed.

## Get credentials for development builds

For Android and iOS, there are different requirements to set up your credentials.

#### Android

For Android, you need to configure **Firebase Cloud Messaging (FCM)** to get credentials and set up your Expo project.

Follow the steps in [Add Android FCM V1 credentials](/push-notifications/fcm-credentials.md) to set up your credentials.

#### iOS

> A paid Apple Developer Account is required to generate credentials.

For iOS, make sure you have [registered your iOS device](/develop/development-builds/introduction.md?buildenv=build-with-eas#how-would-you-like-to-build-your-development-build) on which you want to test before running the `eas build` command for the first time.

If you create a development build for the first time, you'll be asked to enable push notifications. Answer yes to the following questions when prompted by the EAS CLI:

-   Setup Push Notifications for your project
-   Generating a new Apple Push Notifications service key

  

> If you are not using EAS Build, run `eas credentials` manually.

## Build the app

```sh
eas build
```

## Test using the push notifications tool

After creating and installing the development build, you can use [Expo push notifications tool](https://expo.dev/notifications) to quickly send a test notification to your device.

1.  Start the development server for your project:
    
    ```sh
    # npm
    npx expo start
    
    # yarn
    yarn expo start
    
    # pnpm
    pnpm expo start
    
    # bun
    bun expo start
    ```
    
2.  Open the development build on your device.
    
3.  After the `ExpoPushToken` is generated, enter the value in the Expo push notifications tool with other details (for example, a message title and body).
    
4.  Click on the **Send a Notification** button.
    

After sending the notification from the tool, you should see the notification on your device. Below is an example of an Android device receiving a push notification.
