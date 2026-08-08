---
id: "getting-started/installation/ios"
title: "iOS & Apple Platforms"
description: "This tutorial shows you how to install the RevenueCat SDK in your iOS or other Apple-platform app. By the end, you'll have the SDK added to your Xcode project, verified with a successful build, and be ready to configure the SDK."
permalink: "/docs/getting-started/installation/ios"
slug: "ios"
version: "current"
original_source: "docs/getting-started/installation/ios.mdx"
---

> **AI agents:** This is the Markdown version of a RevenueCat documentation page. For the complete documentation index, see [llms.txt](https://www.revenuecat.com/docs/llms.txt).

This tutorial shows you how to install the RevenueCat SDK in your iOS or other Apple-platform app. By the end, you'll have the SDK added to your Xcode project, verified with a successful build, and be ready to [configure the SDK](https://www.revenuecat.com/docs/getting-started/configuring-sdk).

:::info Building for macOS or Mac Catalyst?
The installation steps are the same. RevenueCat supports [universal purchases](https://developer.apple.com/support/universal-purchase/) for macOS apps, letting you share purchases across the iOS, iPadOS, tvOS, watchOS, and macOS versions of your app. If you need support for legacy Mac App Store purchases, contact support.
:::

## Prerequisites

You need the following before you start:

- A [RevenueCat account](https://app.revenuecat.com/).
- A [project](https://www.revenuecat.com/docs/projects/overview) in your RevenueCat account.
- Your app open in Xcode.

## 1. Install the SDK

[![Release](https://img.shields.io/github/release/RevenueCat/purchases-ios.svg?filter=!*beta*\&style=flat)](https://github.com/RevenueCat/purchases-ios/releases)

Install the RevenueCat SDK with [Swift Package Manager](#install-via-swift-package-manager), the dependency manager built into Xcode. CocoaPods and Carthage are also supported for projects that already depend on them — see [Other installation methods](#other-installation-methods).

### Install via Swift Package Manager

1. In Xcode, select **File > Add Package Dependencies**. The package dialog appears.
2. Paste the repository URL into the search field at the top right:

```text
https://github.com/RevenueCat/purchases-ios-spm.git
```

3. Set **Dependency Rule** to **Up to Next Major Version**, from `5.0.0`.
4. Click **Add Package**. The **Choose Package Products for purchases-ios-spm.git** sheet appears.
5. Set your app as the target for the **RevenueCat** and **RevenueCatUI** package products.
   ![Choose Package Products sheet showing the RevenueCat and RevenueCatUI products selected](https://www.revenuecat.com/docs_images/sdk/spm-integration.png)
6. Click **Add Package**. The libraries you selected appear under **Package Dependencies** in the project navigator.

Your project now includes the SDK. Continue to [Import the SDK](#2-import-the-sdk) to verify the installation.

### Other installation methods

Swift Package Manager is the recommended way to install the SDK. Use CocoaPods or Carthage only if your project already manages its dependencies with them.

Install via CocoaPods

1. Add the SDK to your Podfile. To always use the latest release:

```ruby
pod 'RevenueCat'
```

Alternatively, pin to a specific minor version:

```ruby
pod 'RevenueCat', '~> 5.2'
```

2. Run the install:

```ruby
pod install
```

This adds `RevenueCat.framework` to your workspace.

Install via Carthage

1. Add the SDK to your Cartfile. To always use the latest release:

```text
github "revenuecat/purchases-ios"
```

Alternatively, pin to a specific minor version:

```text
github "revenuecat/purchases-ios" ~> 5.2.3
```

2. Build with XCFrameworks (Carthage 0.37 or later). This skips build-phase setup entirely:

```shell
carthage update --use-xcframeworks
```

For more on using XCFrameworks with Carthage, see the [Carthage documentation](https://github.com/carthage/Carthage/#building-platform-independent-xcframeworks-xcode-12-and-above).

If you're on an older Carthage version, build with regular frameworks instead:

```text
carthage update
```

:::warning
Under certain configurations, using `po` to print objects to the console while debugging might result in the error "Couldn't IRGen Expression". If you run into this, add a single empty Objective-C file to your project and create a bridging header. For more information, see this [write-up on the IRGen error](https://steipete.me/posts/2020/couldnt-irgen-expression/).
:::

## 2. Import the SDK

1. Add the import to any source file that uses the SDK:

**Swift**

```swift
import RevenueCat
```

**Objective-C**

```objectivec
@import RevenueCat;

// or

#import "Purchases.h"
```

2. Build your project (**Product > Build**). If the build succeeds with the import in place, the SDK is installed correctly.

## 3. Enable the In-App Purchase capability

Your app needs the In-App Purchase capability to make purchases through the App Store, in both the sandbox and production environments. Purchases made through the [Test Store](https://www.revenuecat.com/docs/test-and-launch/sandbox/test-store) don't go through the App Store, so if you're starting there, you can come back to this step when you connect the App Store.

Before you add the capability, sign in to Xcode with your Apple Account and assign your project to a team, so Xcode can create a provisioning profile that includes the capability. With the default automatic signing, Xcode handles this configuration for you.

1. In the Project navigator, select your project, then select your app target in the project editor sidebar.
2. Open the **Signing & Capabilities** tab.
3. Click **+ Capability** to open the Capabilities library. The library only lists capabilities available to your platform and Apple Developer Program membership.
4. Search for and double-click **In-App Purchase**. The capability appears below the Signing section, and Xcode updates your signing assets automatically.

For more detail, see Apple's [Adding capabilities to your app](https://developer.apple.com/documentation/xcode/adding-capabilities-to-your-app).

:::info Purchases also require App Store Connect setup
Before purchases can work, the Account Holder must accept the Paid Apps Agreement and provide banking and tax information in [App Store Connect](https://appstoreconnect.apple.com/), and your app's bundle identifier in Xcode must match your App Store Connect app record. See our [App Store product setup](https://www.revenuecat.com/docs/getting-started/entitlements/ios-products) guide.
:::

## Next steps

You've installed the RevenueCat SDK and verified your project builds.

- [Configure the SDK](https://www.revenuecat.com/docs/getting-started/configuring-sdk) with your project's API key — the next step in the [Quickstart](https://www.revenuecat.com/docs/getting-started/quickstart).
- Testing without an App Store Connect setup? Purchases work out of the box with the [Test Store](https://www.revenuecat.com/docs/test-and-launch/sandbox/test-store).
