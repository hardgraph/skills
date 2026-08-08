---
title: AgeRange
description: A library that provides access to age range information using Play Age Signals API on Android and Declared Age Range framework on iOS.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-age-range'
packageName: 'expo-age-range'
platforms: ['android', 'ios', 'expo-go']
isAlpha: true
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/age-range/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/age-range/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo AgeRange

A library that provides access to age range information using Play Age Signals API on Android and Declared Age Range framework on iOS.
Android, iOS, Included in Expo Go

> **This library is currently in [alpha](/more/release-statuses.md#alpha) and will frequently experience breaking changes.**

`expo-age-range` provides access to user age range information. It uses Google's [Play Age Signals API](https://developer.android.com/google/play/age-signals/use-age-signals-api) on Android and Apple's [Declared Age Range framework](https://developer.apple.com/documentation/declaredagerange/) on iOS.

This library allows you to request age range information from your app users to help you comply with age-appropriate content regulations (such as in [Texas, USA](https://developer.apple.com/news/?id=btkirlj8)) and provide age-appropriate experiences in your app.

### Limitations

We strongly recommend testing the functionality on a real device, as simulator runtimes may not work as expected.

## Installation

```sh
# npm
npx expo install expo-age-range

# yarn
yarn expo install expo-age-range

# pnpm
pnpm expo install expo-age-range

# bun
bun expo install expo-age-range
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Configuration in app config

### Setup iOS project

To use the age range API on iOS, you need to build your project with Xcode 26.0 or later. The `com.apple.developer.declared-age-range` entitlement is required. Add it to your [app config](/versions/latest/config/app.md) file:

```json
{
  "expo": {
    "ios": {
      "entitlements": {
        "com.apple.developer.declared-age-range": true
      }
    }
  }
}
```

#### Are you using this library in an existing React Native app?

For existing React Native projects, add the entitlement to your project's **ios/[app]/[app].entitlements** file:

```xml
<key>com.apple.developer.declared-age-range</key>
<true/>
```

## Usage

```tsx
import * as AgeRange from 'expo-age-range';
import { useState } from 'react';
import { StyleSheet, Text, View, Button } from 'react-native';

export default function App() {
  const [result, setResult] = useState<AgeRange.AgeRangeResponse | { error: string } | null>(null);

  const requestAgeRange = async () => {
    try {
      const ageRange = await AgeRange.requestAgeRangeAsync({
        threshold1: 10,
        threshold2: 13,
        threshold3: 18,
      });
      setResult(ageRange);
    } catch (error) {
      setResult({ error: error.message });
    }
  };

  return (
    <View style={styles.container}>
      <Button title="Request age range" onPress={requestAgeRange} />
      {result && (
        <Text style={styles.result}>
          {'error' in result ? `Error: ${result.error}` : `Lower age bound: ${result.lowerBound}`}
        </Text>
      )}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    padding: 20,
  },
  result: {
    marginTop: 20,
    fontSize: 16,
  },
});
```

## Additional resources

-   [Play Age Signals API](https://developer.android.com/google/play/age-signals/use-age-signals-api): Android documentation for age signals
-   [Declared Age Range framework](https://developer.apple.com/documentation/declaredagerange/): iOS documentation for declared age range

## API

```ts
import * as AgeRange from 'expo-age-range';
```

## Methods

### `AgeRange.getRequiredRegulatoryFeaturesAsync()`

Supported platforms: iOS 26.4+.

Returns the set of regulatory features that the OS reports as required for the current user.

Use this to discover which age-assurance obligations apply.

Resolves with `null` on iOS earlier than 26.4 and on Android and web — treat `null` as "unknown" rather than "no features required".

Returns: `Promise<agerangeregulatoryfeature[]>`

### `AgeRange.isEligibleForAgeFeaturesAsync()`

Supported platforms: iOS 26.2+.

Asks the OS whether age-assurance regulation applies to the current user. Apple uses this to signal that the account region is covered by a law such as Utah's or Louisiana's age-assurance requirements, so apps can avoid gating users in jurisdictions where the rules do not apply.

-   Resolves with `true` only when Apple confirms regulation applies.
-   Resolves with `false` when the OS confirms regulation does not apply.
-   Resolves with `null` on iOS earlier than 26.2, and on Android and web. Treat `null` as "unknown" rather than a definitive `false`.
-   Rejects when the request fails — see [AgeRangeService.Error](https://developer.apple.com/documentation/declaredagerange/agerangeservice/error) for more information. Treat rejection as "unknown" and fall through to [`requestAgeRangeAsync`](#agerangerequestagerangeasyncoptions) or your own gating logic.

Recommended pattern: call this first and only prompt the user for their age range when the result is not `false`. When it is `false`, the user is outside a regulated jurisdiction and you can skip the age gate entirely.

Returns: `Promise<boolean>`

Example

```ts
try {
  const eligible = await isEligibleForAgeFeaturesAsync();
  if (eligible === false) {
    // Regulation does not apply — no age gate needed.
    return;
  }
} catch {
  // Treat errors as "unknown" and fall through to the prompt below or your own gating logic.
}

const ageRange = await requestAgeRangeAsync({ threshold1: 18 });
```

### `AgeRange.requestAgeRangeAsync(options)`

Supported platforms: Android, iOS 26.0+.

| Parameter | Type |
| --- | --- |
| `options` | [AgeRangeRequest](#agerangerequest) |

  

Prompts the user to share their age range with the app. Responses may be cached by the OS for future requests.

Returns: `Promise<agerangeresponse>`

A promise that resolves with user's age range response, or rejects with an error. The user needs to be signed in on the device to get a valid response. When not supported (earlier than iOS 26 and web), the call returns `lowerBound: 18`, which is equivalent to the response of an adult user.

### `AgeRange.showSignificantUpdateAcknowledgmentAsync(updateDescription)`

Supported platforms: iOS 26.4+.

| Parameter | Type | Description |
| --- | --- | --- |
| `updateDescription` | `string` | A description of the significant update to show to the user. |

  

Displays a system-provided interface for people to acknowledge a significant app update.

Only on iOS 26.4+, this presents an update acknowledgement dialog and resolves once the user confirms it, or rejects with an error. On unsupported platforms this resolves immediately without showing any UI.

Call [`getRequiredRegulatoryFeaturesAsync`](#agerangegetrequiredregulatoryfeaturesasync) first to determine whether the user actually needs to acknowledge a significant change — only invoke this function when the returned features include `'significantAppChangeRequiresAdultNotification'`. Doing so avoids prompting users who are not subject to the regulation.

Returns: `Promise<void>`

## Types

### `AgeRangeRegulatoryFeature`

Supported platforms: iOS 26.4+.

Literal type: `string`

A regulatory feature that your app may need to support for the current user.

Mirrors [`AgeRangeService.RegulatoryFeature`](https://developer.apple.com/documentation/declaredagerange/agerangeservice/regulatoryfeature).

Acceptable values are: `'declaredAgeRangeRequired'` | `'significantAppChangeRequiresAdultNotification'` | `'significantAppChangeRequiresParentalConsent'`

### `AgeRangeRequest`

Supported platforms: iOS.

Options for requesting age range information from the user.

| Property | Type | Description |
| --- | --- | --- |
| threshold1 | `number` | The required minimum age for your app. |
| threshold2(optional) | `number` | An optional additional minimum age for your app. |
| threshold3(optional) | `number` | An optional additional minimum age for your app. |

### `AgeRangeResponse`

Supported platforms: Android, iOS.

Response containing the user's age range information.

Contains age boundaries and platform-specific metadata.

| Property | Type | Description |
| --- | --- | --- |
| activeParentalControls(optional) | `string[]` | Supported platforms: iOS. List of parental controls enabled and shared as a part of age range declaration. |
| ageRangeDeclaration(optional) | `'selfDeclared' | 'guardianDeclared' | 'confirmed' | null` | Supported platforms: iOS. Indicates how the age range was declared:
-   `'selfDeclared'` — declared by the user themselves.
-   `'guardianDeclared'` — declared by someone else (parent, guardian, or Family Organizer in a Family Sharing group).
-   `'confirmed'` — confirmed by the system (for example, verified against a government ID or payment method). Only reported on iOS 26.5+.

 |
| installId(optional) | `string | null` | Supported platforms: Android. An ID assigned to supervised user installs by Google Play, used to notify you of revoked app approval. |
| lowerBound | `number | null` | The lower limit of the person’s age range. |
| mostRecentApprovalDate(optional) | `number | null` | Supported platforms: Android. The effective date (timestamp) of the most recent significant change that was approved. |
| upperBound | `number | null` | The upper limit of the person’s age range. |
| userStatus(optional) | `'VERIFIED' | 'SUPERVISED' | 'SUPERVISED_APPROVAL_PENDING' | 'SUPERVISED_APPROVAL_DENIED' | 'DECLARED' | 'UNKNOWN' | null` | Supported platforms: Android. The user's age verification or supervision status. |

## Error codes

Available in the `code` property of any error thrown by the native module. For Android-specific error codes, see "Handle API error codes" in [Use Play Age Signals API docs](https://developer.android.com/google/play/age-signals/use-age-signals-api#handle-api-errors).

| Code | Platform | Description |
| --- | --- | --- |
| `ERR_AGE_RANGE_USER_DECLINED` | iOS | User declined to share their age range. |
| `ERR_AGE_RANGE_NOT_AVAILABLE` | iOS | Age range not available. The most likely cause is that user is not signed in to their Apple account on the device. |
| `ERR_AGE_RANGE_INVALID_REQUEST` | iOS | The provided params were invalid. The age ranges need to be minimum 2 years apart. |
