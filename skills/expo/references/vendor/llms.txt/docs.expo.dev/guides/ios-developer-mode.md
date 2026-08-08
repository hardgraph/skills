---
modificationDate: July 29, 2026
title: iOS Developer Mode
description: Learn how to enable iOS Developer Mode setting on iOS 16 and above to run internal distribution builds and local development builds.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/ios-developer-mode/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/ios-developer-mode/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Assorted
Pages in this section:
- [Authentication with OAuth or OpenID providers](https://docs.expo.dev/guides/authentication.md)
- [Using Hermes](https://docs.expo.dev/guides/using-hermes.md)
- [iOS Developer Mode](https://docs.expo.dev/guides/ios-developer-mode.md) (this page)
- [Expo Vector Icons](https://docs.expo.dev/guides/icons.md)
- [Localization](https://docs.expo.dev/guides/localization.md)
- [Using Bun](https://docs.expo.dev/guides/using-bun.md)
- [Edit rich text](https://docs.expo.dev/guides/editing-richtext.md)
- [App store assets](https://docs.expo.dev/guides/store-assets.md)
- [Local-first](https://docs.expo.dev/guides/local-first.md)
- [Keyboard handling](https://docs.expo.dev/guides/keyboard-handling.md)
- [Controlled components](https://docs.expo.dev/guides/controlled-components.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# iOS Developer Mode

Learn how to enable iOS Developer Mode setting on iOS 16 and above to run internal distribution builds and local development builds.

> This does not apply to builds signed using enterprise provisioning or to any builds installed on an iOS Simulator.

Devices running iOS 16 and above need to enable OS-level **Developer Mode** setting before they can run [internal distribution](/build/internal-distribution.md) builds (including those built with EAS) or local development builds after installing them on the device.

There are two ways you can enable Developer Mode on your device:

-   Directly on an iOS device
-   By connecting an iOS device with a Mac that has Xcode installed

#### Prerequisites

##### A device running iOS 16 or later

Developer Mode is only required on devices running iOS 16 and later.

The instructions below need to be followed only once per device.

## Enable Developer Mode

### Directly on an iOS device

To follow the steps below, **install your development build on your device before enabling the Developer Mode.** When the build is created, follow the instructions on the EAS dashboard to install it on your iOS device.

Once the build is installed on your device, press the app icon. This will open an alert asking you to enable Developer Mode. Press **OK**.

Go to the Settings app, and navigate to **Privacy & Security** > **Developer Mode**.

Enable the toggle. You will receive a prompt from iOS to restart your device. Press **Restart**.

After the device restarts, unlock your device. A system alert should appear. Press **Turn On** and then, when prompted, enter your device's passcode.

Developer Mode is now enabled. You can now interact with your internal distribution builds and local development builds.

You can turn off Developer Mode at any time. However, you'll need to repeat this same process to re-enable it.

### Connect an iOS device with a Mac

> **Note:** Xcode must be installed on the Mac device before following the steps below.

You don't need to install the development build on your iOS device first to enable Developer Mode by connecting it to a Mac. You can:

Connect your iOS device to a Mac using a USB cable. Press **Trust** on your iOS device when **Trust This Computer?** alert is prompted.

Open Xcode, and from the menu bar, navigate to **Window** > **Devices and Simulators**.

Under **Devices**, you'll see a warning "Previous preparation error: Developer Mode disabled" with instructions on enabling Developer Mode on the iOS device.

On the iOS device, open **Settings** > **Privacy & Security** > **Developer Mode**.

Enable the toggle. You will receive a prompt from iOS to restart your device. Press **Restart**.

After the device restarts, unlock your device. A system alert should appear. Press **Turn On**, and enter your device's passcode when prompted.

Developer Mode is now enabled. You can now interact with your internal distribution builds and local development builds.

You can turn off Developer Mode at any time. However, you'll need to repeat this same process to re-enable it.
