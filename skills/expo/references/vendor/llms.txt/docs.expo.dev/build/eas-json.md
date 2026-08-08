---
modificationDate: July 28, 2026
title: Configure EAS Build with eas.json
description: Learn how a project using EAS services is configured with eas.json.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/build/eas-json/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/build/eas-json/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build
Pages in this section:
- [Introduction](https://docs.expo.dev/build/introduction.md)
- [Create your first build](https://docs.expo.dev/build/setup.md)
- [Configure with eas.json](https://docs.expo.dev/build/eas-json.md) (this page)
- [Internal distribution](https://docs.expo.dev/build/internal-distribution.md)
- [Automate submissions](https://docs.expo.dev/build/automate-submissions.md)
- [Using EAS Update](https://docs.expo.dev/build/updates.md)
- [Trigger builds from CI](https://docs.expo.dev/build/building-on-ci.md)
- [Trigger builds from GitHub App](https://docs.expo.dev/build/building-from-github.md)
- [Expo Orbit](https://docs.expo.dev/build/orbit.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Configure EAS Build with eas.json

Learn how a project using EAS services is configured with eas.json.

**eas.json** is the configuration file for EAS CLI and services. It is generated when the [`eas build:configure` command](/build/setup.md#configure-the-project) runs for the first time in your project and is located next to **package.json** at the root of your project. Configuration for EAS Build all belongs under the `build` key.

The default configuration for **eas.json** generated in a new project is shown below:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal"
    },
    "production": {}
  }
}
```

## Build profiles

A build profile is a named group of configurations that describes the necessary parameters to perform a certain type of build.

The JSON object under the `build` key can contain multiple build profiles, and you can have custom build profile names. In the default configuration, there are three build profiles: `development`, `preview`, and `production`. However, these could have been named `foo`, `bar`, and `baz`.

To run a build with a specific profile, use the command as shown below with a `<profile-name>`:

```sh
eas build --profile
```

If you omit the `--profile` flag, EAS CLI will default to using the profile with the name `production` (if it exists).

### Platform-specific and common options

Inside each build profile, you can specify [`android`](/eas/json.md#android-specific-options) and [`ios`](/eas/json.md#ios-specific-options) fields that contain platform-specific configuration for the build. [Options that are available to both platforms](/eas/json.md#common-properties-for-native-platforms) can be provided on the platform-specific configuration object or the root of a profile.

### Sharing configuration between profiles

Build profiles can be extended to other build profile properties using the `extends` option.

For example, in the `preview` profile you might have `"extends": "production"`. This will make the `preview` profile inherit the configuration of the `production` profile.

You can keep chaining profile extensions up to the depth of 5 as long as you avoid making circular dependencies.

## Common use cases

Developers using Expo tools usually end up having three different types of builds: **development**, **preview**, and **production**.

### Development builds

By default, `eas build:configure` will create a `development` profile with `"developmentClient": true`. This indicates that this build depends on [`expo-dev-client`](/develop/development-builds/introduction.md). These builds include developer tools, and they are never submitted to an app store.

The `development` profile also defaults to [`"distribution": "internal"`](/build/internal-distribution.md). This will make it easy to distribute your app directly to physical Android and iOS devices.

You can also configure your development builds to run on the [iOS Simulator](/build-reference/simulators.md). To do this, use the following configuration for the `development` profile:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "ios": {
        "simulator": true
      }
    }
    ... 
  }
  ... 
}
```

> **Note:** For iOS, to create a build for internal distribution and another for the iOS Simulator, you can create a separate development profile for that build. You can give the profile a custom name. For example, `development-simulator`, and use the [iOS Simulator specific configuration](/build-reference/simulators.md#configuring-a-profile-to-build-for-simulators) on that profile instead of on `development`. No such configuration is required to run an [Android **.apk** on a device and an Android Emulator](/build-reference/apk.md) as the same **.apk** will run with both environments.

### Preview builds

These builds don't include developer tools. They are intended to be installed by your team and other stakeholders, to test out the app in production-like circumstances. In this way, they are similar to [production builds](/build/eas-json.md#production-builds). However, they are different from production builds because they are either not signed for distribution on app stores (ad hoc or enterprise provisioning on iOS), or are packaged in a way that is not optimal for store deployment (Android **.apk** is recommended for preview, **.aab** is recommended for Google Play Store).

A minimal `preview` profile example:

```json
{
  "build": {
    "preview": {
      "distribution": "internal"
    }
    ... 
  }
  ... 
}
```

Similar to [development builds](/build/eas-json.md#development-builds), you can configure a preview build to run on the [iOS Simulator](/build-reference/simulators.md) or create a variant of your preview profile for that purpose. No such configuration is required to run an [Android **.apk** on a device and an Android Emulator](/build-reference/apk.md) as the same **.apk** will run with both environments.

### Production builds

These builds are submitted to an app store, for release to the general public or as part of a store-facilitated testing process such as TestFlight.

Production builds must be installed through their respective app stores. They cannot be installed directly on your Android Emulator or device, or iOS Simulator or device. The only exception to this is if you explicitly set `"buildType": "apk"` for Android on your build profile. However, it is recommended to use **.aab** when submitting to stores, as this is the default configuration.

A minimal `production` profile example:

```json
{
  "build": {
    "production": {}
    ... 
  }
  ... 
}
```

### Installing multiple builds of the same app on a single device

It's common to have development and production builds installed simultaneously on the same device. See [Install app variants on the same device](/build-reference/variants.md).

## Configuring build tools

Every build depends either implicitly or explicitly on a specific set of versions of related tools that are needed to carry out the build process. These include but are not limited to: Node.js, npm, Yarn, Ruby, Bundler, CocoaPods, Fastlane, Xcode, and Android NDK.

### Selecting build tool versions

Versions for the most common build tools can be set on build profiles with fields corresponding to the names of the tools. For example [`node`](/eas/json.md#node):

```json
{
  "build": {
    "production": {
      "node": "18.18.0"
    }
    ... 
  }
  ... 
}
```

It's common to share build tool configurations between profiles. Use `extends` for that:

```json
{
  "build": {
    "production": {
      "node": "18.18.0"
    },
    "preview": {
      "extends": "production",
      "distribution": "internal"
    },
    "development": {
      "extends": "production",
      "developmentClient": true,
      "distribution": "internal"
    }
    ... 
  }
  ... 
}
```

### Selecting resource class

A resource class is the virtual machine resources configuration (CPU cores, RAM size) EAS Build provides to your jobs. By default, the resource class is set to `medium`, which is usually sufficient for both small and bigger projects. However, if your project requires a more powerful CPU or bigger memory, or if you want your builds to finish faster, you can switch to `large` workers.

For more details on resources provided to each class, see [`android.resourceClass`](/eas/json.md#resourceclass-1) and [`ios.resourceClass`](/eas/json.md#resourceclass-2) properties. To run your build on a worker of a specific resource class, configure this property in your build profile:

```json
{
  "build": {
    "production": {
      "android": {
        "resourceClass": "medium"
      },
      "ios": {
        "resourceClass": "large"
      },
    }
    ... 
  }
  ... 
}
```

> **Note**: Running jobs on a `large` worker requires a [paid EAS plan](https://expo.dev/accounts/%5Baccount%5D/settings/billing).

### Selecting a base image

The base image for the build job controls the default versions for a variety of dependencies, such as Node.js, Yarn, and CocoaPods. You can override them using the specific named fields as described in the previous section using `resourceClass`. However, the image includes specific versions of tools that can't be explicitly set any other way, such as the operating system version and Xcode version.

If you are building an app with Expo, EAS Build will pick the appropriate image to use with a reasonable set of dependencies for the SDK version that you are building for. Otherwise, it is recommended to see the list of available images on [Build server infrastructure](/build-reference/infrastructure.md).

### Examples

#### Schema

```json
{
  "cli": {
    "version": "SEMVER_RANGE",
    "requireCommit": boolean,
    "appVersionSource": string,
    "promptToConfigurePushNotifications": boolean,
  },
  "build": {
    "BUILD_PROFILE_NAME_1": {
      ...COMMON_OPTIONS,
      "android": {
        ...COMMON_OPTIONS,
        ...ANDROID_OPTIONS
      },
      "ios": {
        ...COMMON_OPTIONS,
        ...IOS_OPTIONS
      }
    },
    "BUILD_PROFILE_NAME_2": {},
	... 
  }
}
```

> You can specify [common properties](/eas/json.md#common-properties-for-native-platforms) both in the platform-specific configuration object or at the profile's root. The platform-specific options take precedence over globally-defined ones.

#### A Continuous Native Generation (CNG) project with several profiles

```json
{
  "build": {
    "base": {
      "node": "12.13.0",
      "yarn": "1.22.5",
      "env": {
        "EXAMPLE_ENV": "example value"
      },
      "android": {
        "image": "default",
        "env": {
          "PLATFORM": "android"
        }
      },
      "ios": {
        "image": "latest",
        "env": {
          "PLATFORM": "ios"
        }
      }
    },
    "development": {
      "extends": "base",
      "developmentClient": true,
      "env": {
        "ENVIRONMENT": "development"
      },
      "android": {
        "distribution": "internal",
        "withoutCredentials": true
      },
      "ios": {
        "simulator": true
      }
    },
    "staging": {
      "extends": "base",
      "env": {
        "ENVIRONMENT": "staging"
      },
      "distribution": "internal",
      "android": {
        "buildType": "apk"
      }
    },
    "production": {
      "extends": "base",
      "env": {
        "ENVIRONMENT": "production"
      }
    }
  }
}
```

#### An existing React Native project with several profiles

```json
{
  "build": {
    "base": {
      "env": {
        "EXAMPLE_ENV": "example value"
      },
      "android": {
        "image": "ubuntu-18.04-android-30-ndk-r19c",
        "ndk": "21.4.7075529"
      },
      "ios": {
        "image": "latest",
        "node": "12.13.0",
        "yarn": "1.22.5"
      }
    },
    "development": {
      "extends": "base",
      "env": {
        "ENVIRONMENT": "staging"
      },
      "android": {
        "distribution": "internal",
        "withoutCredentials": true,
        "gradleCommand": ":app:assembleDebug"
      },
      "ios": {
        "simulator": true,
        "buildConfiguration": "Debug"
      }
    },
    "staging": {
      "extends": "base",
      "env": {
        "ENVIRONMENT": "staging"
      },
      "distribution": "internal",
      "android": {
        "gradleCommand": ":app:assembleRelease"
      }
    },
    "production": {
      "extends": "base",
      "env": {
        "ENVIRONMENT": "production"
      }
    }
  }
}
```

## Environment variables

You can configure environment variables on your build profiles using the `"env"` field. These environment variables will be used to evaluate **app.config.js** locally when you run `eas build`, and they will also be set on the EAS Build builder.

```json
{
  "build": {
    "production": {
      "node": "16.13.0",
      "env": {
        "API_URL": "https://company.com/api"
      }
    },
    "preview": {
      "extends": "production",
      "distribution": "internal",
      "env": {
        "API_URL": "https://staging.company.com/api"
      }
    }
    ... 
  }
  ... 
}
```

The [Environment variables and secrets](/eas/environment-variables.md) reference explains this topic in greater detail, and the [Use EAS Update](/build/updates.md) guide provides considerations when using this feature alongside `expo-updates`.

## More

[EAS Build schema reference](/eas/json.md#eas-build) — See complete reference of available properties for EAS Build.
