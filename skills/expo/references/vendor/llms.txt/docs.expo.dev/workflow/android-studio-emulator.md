---
modificationDate: June 30, 2026
title: Android Studio Emulator
description: Learn how to set up the Android Emulator to test your app on a virtual Android device.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/workflow/android-studio-emulator/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/workflow/android-studio-emulator/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Development process > Reference
Pages in this section:
- [Work with monorepos](https://docs.expo.dev/guides/monorepos.md)
- [View logs](https://docs.expo.dev/workflow/logging.md)
- [Development and production modes](https://docs.expo.dev/workflow/development-mode.md)
- [Common development errors](https://docs.expo.dev/workflow/common-development-errors.md)
- [Android Studio Emulator](https://docs.expo.dev/workflow/android-studio-emulator.md) (this page)
- [iOS Simulator](https://docs.expo.dev/workflow/ios-simulator.md)
- [New Architecture](https://docs.expo.dev/guides/new-architecture.md)
- [React Compiler](https://docs.expo.dev/guides/react-compiler.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Android Studio Emulator

Learn how to set up the Android Emulator to test your app on a virtual Android device.

If you don't have an Android device available to test with, we recommend using the default emulator that comes with Android Studio. If you run into any problems setting it up, follow the steps in this guide.

## Install JDK

#### macOS

#### Prerequisites

Use a package manager such as [Homebrew](https://brew.sh/) to install the following dependency.

#### Install dependencies

> Installing Watchman is only required for projects on SDK 55 and earlier.

[Install Watchman](https://facebook.github.io/watchman/docs/install#macos) using a tool such as Homebrew:

```sh
brew install watchman
```

Install OpenJDK distribution called Azul Zulu using Homebrew. This distribution offers JDKs for both Apple Silicon and Intel Macs.

Run the following commands in a terminal:

```sh
brew install --cask zulu@17
```

After you install the JDK, add the `JAVA_HOME` environment variable in **~/.bash_profile** (or **~/.zshrc** if you use Zsh):

```bash
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home
```

#### Windows

#### Prerequisites

Use a package manager such as [Chocolatey](https://chocolatey.org/) to install the following dependencies.

#### Install dependencies

Install [Java SE Development Kit (JDK)](https://openjdk.org/):

```sh
choco install -y microsoft-openjdk17
```

#### Linux

#### Install dependencies

> Installing Watchman is only required for projects on SDK 55 and earlier.

Follow [instructions from the Watchman documentation](https://facebook.github.io/watchman/docs/install#linux) to compile and install it from the source.

Install [Java SE Development Kit (JDK)](https://openjdk.org/):

You can download and install [OpenJDK@17](http://openjdk.java.net/) from [AdoptOpenJDK](https://adoptopenjdk.net/) or your system packager.

## Set up Android Studio

#### macOS

Download and install [Android Studio](https://developer.android.com/studio).

Open the **Android Studio** app. On the first launch, the **Android Studio Setup Wizard** appears. Click **Next** on the **Welcome** screen. Then, under **Install Type**, select **Standard** and click **Next**.

Verify the settings and click **Next**. Then, accept the license agreement and click **Next** again. The wizard downloads and installs the Android SDK and its tools. Click **Finish** when the installation completes.

By default, Android Studio will install the latest version of the Android SDK. However, Android 16 (`Baklava`) SDK is required to compile a React Native app.

Open Android Studio, go to **Settings** > **Languages & Frameworks** > **Android SDK**. From the **SDK Platforms** tab, and under **Android 16 (`Baklava`)**, select **Android SDK Platform 36** and **Sources for Android 36**.

Then, click on the **SDK Tools** tab and make sure you have at least one version of the **Android SDK Build-Tools** and **Android Emulator** installed.

Copy or remember the path listed in the box that says **Android SDK Location**.

Add the following lines to your **/.zprofile** or **~/.zshrc** (if you are using bash, then **~/.bash_profile** or **~/.bashrc**) config file:

```sh
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

Reload the path environment variables in your current shell:

```sh
source $HOME/.zshrc
source $HOME/.bashrc
```

Finally, make sure that you can run `adb` from your terminal.

#### Troubleshooting: Android Studio not recognizing JDK

If Android Studio doesn't recognize your homebrew installed JDK, you can create a Gradle configuration file to explicitly set the Java path:

1.  Create a Gradle properties file in your home directory:
    
    ```sh
    touch ~/.gradle/gradle.properties
    ```
    
2.  Add the following line to the **gradle.properties** file, replacing the path with your actual Java installation path:
    
    ```bash
    java.home=/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home
    ```
    
3.  If you have an existing `.gradle` folder in your project directory, delete it and reopen your project in Android Studio:
    
    ```sh
    rm -rf .gradle
    ```
    

This should resolve issues with Android Studio not detecting your JDK installation.

#### Windows

Download [Android Studio](https://developer.android.com/studio).

Open **Android Studio Setup**. Under **Select components to install**, select Android Studio and Android Virtual Device. Then, click **Next**.

In the Android Studio Setup Wizard, under **Install Type**, select **Standard** and click **Next**.

The Android Studio Setup Wizard will ask you to verify the settings, such as the version of Android SDK, platform-tools, and so on. Click **Next** after you have verified.

In the next window, accept licenses for all available components.

By default, Android Studio will install the latest version of the Android SDK. However, Android 16 (`Baklava`) SDK is required to compile a React Native app.

Open Android Studio, go to **Settings** > **Languages & Frameworks** > **Android SDK**. From the **SDK Platforms** tab, and under **Android 16 (`Baklava`)**, select **Android SDK Platform 36** and **Sources for Android 36**.

Then, click on the **SDK Tools** tab and make sure you have at least one version of the **Android SDK Build-Tools** and **Android Emulator** installed.

After the tools installation is complete, configure the `ANDROID_HOME` environment variable. Go to **Windows Control Panel** > **User Accounts** > **User Accounts** (again) > **Change my environment variables** and click **New** to create a new `ANDROID_HOME` user variable. The value of this variable will point to the path to your Android SDK:

#### How to find installed SDK location?

By default, the Android SDK is installed at the following location:

```bash
%LOCALAPPDATA%\Android\Sdk
```

To find the location of the SDK in Android Studio manually, go to **Settings** > **Languages & Frameworks** > **Android SDK**. See the location next to **Android SDK Location**.

To verify that the new environment variable is loaded, open **PowerShell**, and copy and paste the following command:

```sh
Get-ChildItem -Path Env:
```

The command will output all user environment variables. In this list, see if `ANDROID_HOME` has been added.

To add platform-tools to the Path, go to **Windows Control Panel** > **User Accounts** > **User Accounts** (again) > **Change my environment variables** > **Path** > **Edit** > **New** and add the path to the platform-tools to the list as shown below:

#### How to find installed platform-tools location

By default, the platform-tools are installed at the following location:

```bash
%LOCALAPPDATA%\Android\Sdk\platform-tools
```

Finally, make sure that you can run `adb` from the PowerShell. For example, run the `adb --version` to see which version of the `adb` your system is running.

## Set up an emulator

On the Android Studio main screen, click **More Actions**, then **Virtual Device Manager** in the dropdown.

Click **Create virtual device**.

Under **Add device**, choose the type of hardware you'd like to emulate. We recommend testing against a variety of devices, but if you're unsure where to start, the newest device in the Pixel line could be a good choice.

Select an OS version to load on the emulator (probably one of the system images), and download the image (if required).

Change any other settings you'd like, and press **Finish** to create the emulator. You can now run this emulator anytime by pressing the Play button in the **Device Manager** window.

## Troubleshooting

### Multiple `adb` versions

Having multiple `adb` versions on your system can result in the following error:

```sh
adb server version (xx) doesn't match this client (xx); killing...
```

This is because the `adb` version on your system is different from the `adb` version on the Android SDK platform-tools.

Open the terminal and check the `adb` version on the system:

```sh
adb version
```

And from the Android SDK platform-tool directory:

```sh
cd ~/Library/Android/sdk/platform-tools
./adb version
```

Copy `adb` from Android SDK directory to `usr/bin` directory:

```sh
sudo cp ~/Library/Android/sdk/platform-tools/adb /usr/bin
```

### How do I install a specific version of Expo Go?

You can create a project with the desired SDK version and open it in a simulator to install the matching version of Expo Go.

```sh
# npm
npx create-expo-app --template blank@57
npx expo start --android

# yarn
yarn create expo-app --template blank@57
yarn expo start --android

# pnpm
pnpm create expo-app --template blank@57
pnpm expo start --android

# bun
bun create expo --template blank@57
bun expo start --android
```

Alternatively, you can download a specific version of Expo Go with the [`expo-go` CLI](https://www.npmjs.com/package/expo-go) by passing an SDK version, or use `latest` for the latest SDK version. This command downloads the Expo Go app to the current directory and caches it under **~/.expo**.

```sh
# npm
npx expo-go download android latest

# yarn
yarn dlx expo-go download android latest

# pnpm
pnpm dlx expo-go download android latest

# bun
bunx expo-go download android latest
```
