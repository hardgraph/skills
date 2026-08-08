---
title: SMS
description: A library that provides access to the system's UI/app for sending SMS messages.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-sms'
packageName: 'expo-sms'
iconUrl: '/static/images/packages/expo-sms.png'
platforms: ['android', 'ios', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/sms/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/sms/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo SMS

A library that provides access to the system's UI/app for sending SMS messages.
Android, iOS, Included in Expo Go

`expo-sms` provides access to the system's UI/app for sending SMS messages.

## Installation

```sh
# npm
npx expo install expo-sms

# yarn
yarn expo install expo-sms

# pnpm
pnpm expo install expo-sms

# bun
bun expo install expo-sms
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## API

```js
import * as SMS from 'expo-sms';
```

## Methods

### `SMS.isAvailableAsync()`

Supported platforms: Android, iOS.

Determines whether SMS is available. Always returns `false` in the iOS simulator, and in browser.

Returns: `Promise<boolean>`

Returns a promise that fulfils with a `boolean`, indicating whether SMS is available on this device.

Example

```ts
const isAvailable = await SMS.isAvailableAsync();
if (isAvailable) {
  // do your SMS stuff here
} else {
  // misfortune... there's no SMS available on this device
}
```

### `SMS.sendSMSAsync(addresses, message, options)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `addresses` | `string | string[]` | An array of addresses (phone numbers) or single address passed as strings. Those would appear as recipients of the prepared message. |
| `message` | `string` | Message to be sent. |
| `options`(optional) | [SMSOptions](#smsoptions) | A `SMSOptions` object defining additional SMS configuration options. |

  

Opens the default UI/app for sending SMS messages with prefilled addresses and message.

Returns: `Promise<smsresponse>`

Returns a Promise that fulfils with the SMS action is invoked by the user, with corresponding result:

-   If the user cancelled the SMS sending process: `{ result: 'cancelled' }`.
-   If the user has sent/scheduled message for sending: `{ result: 'sent' }`.
-   If the status of the SMS message cannot be determined: `{ result: 'unknown' }`.

Android does not provide information about the status of the SMS message, so on Android devices the Promise will always resolve with `{ result: 'unknown' }`.

> Note: The only feedback collected by this module is whether any message has been sent. That means we do not check actual content of message nor recipients list.

Example

```ts
const { result } = await SMS.sendSMSAsync(
  ['0123456789', '9876543210'],
  'My sample HelloWorld message',
  {
    attachments: {
      uri: 'path/myfile.png',
      mimeType: 'image/png',
      filename: 'myfile.png',
    },
  }
);
```

## Types

### `SMSAttachment`

Supported platforms: Android, iOS.

An object that is used to describe an attachment that is included with a SMS message.

| Property | Type | Description |
| --- | --- | --- |
| filename | `string` | The filename of the attachment. |
| mimeType | `string` | The mime type of the attachment such as `image/png`. |
| uri | `string` | The content URI of the attachment. The URI needs be a content URI so that it can be accessed by other applications outside of Expo. See [FileSystem.getContentUriAsync](/versions/latest/sdk/filesystem.md#filesystemgetcontenturiasyncfileuri)). |

### `SMSOptions`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| attachments(optional) | [SMSAttachment](#smsattachment) | [SMSAttachment[]](#smsattachment) | - |

### `SMSResponse`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| result | `'unknown' | 'sent' | 'cancelled'` | Status of SMS action invoked by the user. |
