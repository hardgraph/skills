---
modificationDate: July 28, 2026
title: Handle incoming notifications
description: Learn how to respond to a notification received by your app and take action based on the event.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/push-notifications/receiving-notifications/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/push-notifications/receiving-notifications/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Push notifications
Pages in this section:
- [Overview](https://docs.expo.dev/push-notifications/overview.md)
- [About notification types](https://docs.expo.dev/push-notifications/what-you-need-to-know.md)
- [Expo push notifications setup](https://docs.expo.dev/push-notifications/push-notifications-setup.md)
- [Send notifications with the Expo Push Service](https://docs.expo.dev/push-notifications/sending-notifications.md)
- [Handle incoming notifications](https://docs.expo.dev/push-notifications/receiving-notifications.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Handle incoming notifications

Learn how to respond to a notification received by your app and take action based on the event.

The [`expo-notifications`](/versions/latest/sdk/notifications.md) library contains event listeners that handle how your app responds when receiving a notification.

## Notification event listeners

The [`addNotificationReceivedListener`](/versions/latest/sdk/notifications.md#addnotificationreceivedlistenerlistener) and [`addNotificationResponseReceivedListener`](/versions/latest/sdk/notifications.md#addnotificationresponsereceivedlistenerlistener) event listeners receive an object when a notification is received or interacted with.

These listeners allow you to add behavior when notifications are received while your app is open and foregrounded and when your app is backgrounded or closed and the user taps on the notification.

```js
useEffect(() => {
  registerForPushNotificationsAsync().then(token => setExpoPushToken(token));

  const notificationListener = Notifications.addNotificationReceivedListener(notification => {
    console.log(notification);
  });

  const responseListener = Notifications.addNotificationResponseReceivedListener(response => {
    console.log(response);
  });

  return () => {
    notificationListener.remove();
    responseListener.remove();
  };
}, []);
```

#### Android notification object example from addNotificationReceivedListener

Sample of the `notification` object received by the callback function when using `Notifications.addNotificationReceivedListener`:

```json
// console.log(notification);
{
  "request": {
    "trigger": {
      "remoteMessage": {
        "originalPriority": 2,
        "sentTime": 1724782348210,
        "notification": {
          "usesDefaultVibrateSettings": false,
          "color": null,
          "channelId": null,
          "visibility": null,
          "sound": null,
          "tag": null,
          "bodyLocalizationArgs": null,
          "imageUrl": null,
          "title": "Chat App",
          "ticker": null,
          "eventTime": null,
          "body": "New message from John Doe",
          "titleLocalizationKey": null,
          "notificationPriority": null,
          "icon": null,
          "usesDefaultLightSettings": false,
          "sticky": false,
          "link": null,
          "titleLocalizationArgs": null,
          "bodyLocalizationKey": null,
          "usesDefaultSound": false,
          "clickAction": null,
          "localOnly": false,
          "lightSettings": null,
          "notificationCount": null
        },
        "data": {
          "channelId": "default",
          "message": "New message from John Doe",
          "title": "Chat App",
          "body": "{\"senderId\":\"user123\",\"senderName\":\"John Doe\",\"messageId\":\"msg789\",\"conversationId\":\"conversation-456\",\"messageType\":\"text\",\"timestamp\":1724766427}",
          "scopeKey": "@betoatexpo/expo-notifications-app",
          "experienceId": "@betoatexpo/expo-notifications-app",
          "projectId": "51092087-87a4-4b12-8008-145625477434"
        },
        "to": null,
        "ttl": 0,
        "collapseKey": "dev.expo.notificationsapp",
        "messageType": null,
        "priority": 2,
        "from": "115310547649",
        "messageId": "0:1724782348220771%0f02879c0f02879c"
      },
      "channelId": "default",
      "type": "push"
    },
    "content": {
      "autoDismiss": true,
      "title": "Chat App",
      "badge": null,
      "sticky": false,
      "sound": "default",
      "body": "New message from John Doe",
      "subtitle": null,
      "data": {
        "senderId": "user123",
        "senderName": "John Doe",
        "messageId": "msg789",
        "conversationId": "conversation-456",
        "messageType": "text",
        "timestamp": 1724766427
      }
    },
    "identifier": "0:1724782348220771%0f02879c0f02879c"
  },
  "date": 1724782348210
}
```

You can directly access the notification custom data by logging the `notification.request.content.data` object:

```json
// console.log(notification.request.content.data);
{
  "senderId": "user123",
  "senderName": "John Doe",
  "messageId": "msg789",
  "conversationId": "conversation-456",
  "messageType": "text",
  "timestamp": 1724766427
}
```

#### iOS notification object example from addNotificationReceivedListener

Sample of the `notification` object received by the callback function when using `Notifications.addNotificationReceivedListener`:

```json
// console.log(notification);
{
  "request": {
    "trigger": {
      "class": "UNPushNotificationTrigger",
      "type": "push",
      "payload": {
        "experienceId": "@betoatexpo/expo-notifications-app",
        "projectId": "51092087-87a4-4b12-8008-145625477434",
        "scopeKey": "@betoatexpo/expo-notifications-app",
        "aps": {
          "thread-id": "",
          "category": "",
          "badge": 1,
          "alert": {
            "subtitle": "Hey there! How's your day going?",
            "title": "Chat App",
            "launch-image": "",
            "body": "New message from John Doe"
          },
          "sound": "default"
        },
        "body": {
          "messageId": "msg789",
          "timestamp": 1724766427,
          "messageType": "text",
          "senderId": "user123",
          "senderName": "John Doe",
          "conversationId": "conversation-456"
        }
      }
    },
    "identifier": "3AEB849E-9059-4D09-BC3B-9A0B104CF062",
    "content": {
      "body": "New message from John Doe",
      "sound": "default",
      "launchImageName": "",
      "badge": 1,
      "subtitle": "Hey there! How's your day going?",
      "title": "Chat App",
      "data": {
        "conversationId": "conversation-456",
        "senderName": "John Doe",
        "senderId": "user123",
        "messageType": "text",
        "timestamp": 1724766427,
        "messageId": "msg789"
      },
      "summaryArgument": null,
      "categoryIdentifier": "",
      "attachments": [],
      "interruptionLevel": "active",
      "threadIdentifier": "",
      "targetContentIdentifier": null,
      "summaryArgumentCount": 0
    }
  },
  "date": 1724798493.0589335
}
```

You can directly access the notification custom data by logging the `notification.request.content.data` object:

```json
// console.log(notification.request.content.data);
{
  "senderId": "user123",
  "senderName": "John Doe",
  "messageId": "msg789",
  "conversationId": "conversation-456",
  "messageType": "text",
  "timestamp": 1724766427
}
```

For more information on these objects, see [`Notification`](/versions/latest/sdk/notifications.md#notification) documentation.

## Foreground notification behavior

To handle the behavior when notifications are received when your app is **foregrounded**, use [`Notifications.setNotificationHandler`](/versions/latest/sdk/notifications.md#present-incoming-notifications-when-the-app-is) with the `handleNotification()` callback to set the following options:

-   `shouldPlaySound`
-   `shouldSetBadge`
-   `shouldShowBanner`
-   `shouldShowList`

```jsx
Notifications.setNotificationHandler({
  handleNotification: async () => ({
    shouldPlaySound: false,
    shouldSetBadge: false,
    shouldShowBanner: true,
    shouldShowList: true,
  }),
});
```

## Closed notification behavior

On Android, users can set certain OS-level settings, usually revolving around performance and battery optimization, that can prevent notifications from being delivered when the app is closed. For example, one such setting is the **Deep Clear** option on OnePlus devices using Android 9 and lower versions.
