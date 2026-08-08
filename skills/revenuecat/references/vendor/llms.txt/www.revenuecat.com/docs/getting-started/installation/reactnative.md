---
id: "getting-started/installation/reactnative"
title: "React Native"
description: "What is RevenueCat?"
permalink: "/docs/getting-started/installation/reactnative"
slug: "reactnative"
version: "current"
original_source: "docs/getting-started/installation/reactnative.mdx"
---

> **AI agents:** This is the Markdown version of a RevenueCat documentation page. For the complete documentation index, see [llms.txt](https://www.revenuecat.com/docs/llms.txt).

## What is RevenueCat?

RevenueCat provides a backend and SDKs that wrap StoreKit, Google Play Billing, the Amazon Appstore, the Samsung Galaxy Store, and [RevenueCat Billing](https://www.revenuecat.com/docs/web/overview) to make implementing in-app and web purchases and subscriptions easy. With our SDK, you can build and manage your app business on any platform without having to maintain IAP infrastructure. You can read more about [how RevenueCat fits into your app](https://www.revenuecat.com/blog/where-does-revenuecat-fit-in-your-app) or you can [sign up free](https://app.revenuecat.com/signup) to start building.

## Installation

[![Release](https://img.shields.io/github/release/RevenueCat/react-native-purchases.svg?filter=!*beta*\&style=flat)](https://github.com/RevenueCat/react-native-purchases/releases)

Make sure that the deployment target for iOS is at least 13.4 and Android is at least 6.0 (API 23) [as defined here](https://github.com/facebook/react-native#-requirements).

### React Native package

Purchases for React Native can be installed either via npm or yarn.

We recommend using the latest version of React Native, or making sure that the version is at least greater than 0.64.

#### Option 1.1: Using auto-linking

Recent versions of React Native will automatically link the SDK, so all that's needed is to install the library.

**npm**

```shell
npm install --save react-native-purchases
```

**yarn**

```shell
yarn add react-native-purchases
```

#### Option 1.2: Manual linking

**npm**

```shell
npm install --save react-native-purchases
```

**yarn**

```shell
yarn add react-native-purchases
```

After that, you should link the library to the native projects by doing:

```shell
react-native link react-native-purchases
```

### Using Expo

Use Expo to rapidly iterate on your app by using JavaScript/TypeScript exclusively, while letting Expo take care of everything else.

See [Using RevenueCat with Expo](https://www.revenuecat.com/docs/getting-started/installation/expo) to get started.

### Set the correct launchMode for Android

Depending on your user's payment method, they may be asked by Google Play to verify their purchase in their (banking) app. This means they will have to background your app and go to another app to verify the purchase. If your Activity's `launchMode` is set to anything other than `standard` or `singleTop`, backgrounding your app can cause the purchase to get cancelled. To avoid this, set the `launchMode` of your Activity to `standard` or `singleTop` in your Android app's `AndroidManifest.xml` file, like so:

```xml
<activity 
    android:name="com.your.Activity"
    android:launchMode="standard" />  <!-- or singleTop -->
```

You can find Android's documentation on the various `launchMode` options [here](https://developer.android.com/guide/topics/manifest/activity-element#lmode).

### Amazon Appstore for Android

To build a React Native Android app for the Amazon Appstore, configure the SDK with your Amazon Appstore API key and set `useAmazon` to `true`:

```jsx
import Purchases from 'react-native-purchases';

Purchases.configure({
  apiKey: <revenuecat_project_amazon_api_key>,
  useAmazon: true,
});
```

Adding support for Amazon also requires adding a `.pem` public key to your project. You can configure this key by following Amazon's guide [here](https://developer.amazon.com/es/docs/in-app-purchasing/integrate-appstore-sdk.html#configure_key).

Due to some limitations, RevenueCat will only validate purchases made in production or in Live App Testing and won't validate purchases made with the Amazon App Tester.

### Galaxy Store for Android

Galaxy Store support is available in React Native SDK versions `10.3.0` and above.

To build a React Native Android app for the Galaxy Store, install the Galaxy Store add-on package in addition to `react-native-purchases`.

**npm**

```shell
npm install --save react-native-purchases-store-galaxy
```

**yarn**

```shell
yarn add react-native-purchases-store-galaxy
```

Configure the SDK with your Galaxy Store API key and set `store` to `GALAXY`:

```jsx
import Purchases from 'react-native-purchases';
import { GALAXY_BILLING_MODE } from 'react-native-purchases-store-galaxy';

Purchases.configure({
  apiKey: <revenuecat_project_galaxy_api_key>,
  store: 'GALAXY',
  // Optional. Defaults to PRODUCTION.
  galaxyBillingMode: GALAXY_BILLING_MODE.TEST,
});
```

`galaxyBillingMode` is optional and defaults to `GALAXY_BILLING_MODE.PRODUCTION`. Use `GALAXY_BILLING_MODE.TEST` for test purchases or `GALAXY_BILLING_MODE.ALWAYS_FAIL` to test failure scenarios. Only use production mode when submitting your app for beta or production distribution.

Test purchases for the Galaxy Store can only be made on a physical Galaxy device signed in with a Samsung account. The Galaxy Store does not support making test purchases in emulators.

Before testing purchases, make sure you have also [set up your Galaxy Store app](https://www.revenuecat.com/docs/platform-resources/galaxy-platform-resources/galaxy-setup-guide) and [create Galaxy Store products](https://www.revenuecat.com/docs/getting-started/entitlements/galaxy-products).

## Import Purchases

You should now be able to import `Purchases`.

```jsx
import Purchases from 'react-native-purchases';
```

:::info\[Include BILLING permission for Android projects]
Don't forget to include the `BILLING` permission in your AndroidManifest.xml file
:::

```xml
<uses-permission android:name="com.android.vending.BILLING" />
```

:::info\[Enable In-App Purchase capability for your iOS project]
Don't forget to enable the In-App Purchase capability for your project under `Project Target -> Capabilities -> In-App Purchase`
:::

## Android Build Issues

### R8 Dependencies Conflict

If you encounter build failures related to R8 (Android's code shrinker) when using `react-native-purchases-ui`, you may see errors like:

```
Execution failed for task ':app:mergeExtDexDevDebug'.
> Could not resolve all files for configuration ':app:devDebugRuntimeClasspath'.
```

This issue occurs due to a bug in earlier versions of Android Gradle Plugin (AGP) that affects R8 dependency resolution. To fix this, add the following to your project-level `build.gradle` file (not `app/build.gradle`):

```gradle
buildscript {
    repositories {
        mavenCentral()
        maven {
            url = uri("https://storage.googleapis.com/r8-releases/raw")
        }
    }
    dependencies {
        classpath("com.android.tools:r8:8.1.44")
    }
}
```

This solution forces the use of a specific R8 version that resolves the dependency conflicts. For more details, see the [Google Issue Tracker](https://issuetracker.google.com/issues/342522142#comment8).

## React Native Web Configuration

RevenueCat's React Native SDK supports web platforms, allowing you to manage subscriptions across React Native web, mobile, and desktop apps using the same SDK.

### Web Product Configuration

To enable web purchases in your React Native app, you'll need to configure products using a [RevenueCat Billing app](https://www.revenuecat.com/docs/web/overview).

1. **Create a RevenueCat Billing App** in your RevenueCat project dashboard
2. **Configure your products** for web purchases

For detailed instructions on setting up web products and configuring RevenueCat Billing, see the [RevenueCat Billing Overview](https://www.revenuecat.com/docs/web/overview).

:::info\[RevenueCat Billing vs In-App Purchases]
RevenueCat Billing is RevenueCat's billing engine for web purchases, which uses Stripe as the payment processor. This is separate from iOS/Android in-app purchases but integrates with the same RevenueCat entitlements system, allowing unified subscription management across platforms.
:::

### Current Limitations

When using the React Native SDK on web, keep in mind the following:

- **RevenueCat Billing Required**: Web purchases require RevenueCat Billing setup. Native iOS/Android in-app purchases cannot be processed through the web platform.
- **Payment Processing**: RevenueCat Billing purchases use Stripe as the payment processor through RevenueCat Billing.
- **Customer Portal**: Users can manage their web subscriptions through the RevenueCat-provided customer portal.
- **Platform Separation**: Web products must be configured separately from iOS/Android products in the RevenueCat dashboard, though entitlements can be shared across platforms.
- **User Identity**: For unified cross-platform subscriptions, ensure you're using the same `appUserID` across web and mobile platforms.
- **Unsupported operations**: There are some unsupported operations. Mainly operations `getProducts`, `purchaseProduct` or `restorePurchases` won't work on web environments.

## Next Steps

- Now that you've installed the Purchases SDK in your React Native app, get started by [initializing an instance of Purchases →](https://www.revenuecat.com/docs/getting-started/quickstart#2-initialize-and-configure-the-revenuecat-sdk)
