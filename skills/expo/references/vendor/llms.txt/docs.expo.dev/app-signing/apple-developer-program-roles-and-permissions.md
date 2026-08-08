---
modificationDate: July 29, 2026
title: Apple Developer Program roles and permissions for EAS Build
description: Learn about the Apple Developer account membership requirements for creating an EAS Build.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/app-signing/apple-developer-program-roles-and-permissions/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/app-signing/apple-developer-program-roles-and-permissions/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: EAS > EAS Build > App signing
Pages in this section:
- [App credentials](https://docs.expo.dev/app-signing/app-credentials.md)
- [Automatically managed credentials](https://docs.expo.dev/app-signing/managed-credentials.md)
- [Local credentials](https://docs.expo.dev/app-signing/local-credentials.md)
- [Existing credentials](https://docs.expo.dev/app-signing/existing-credentials.md)
- [Sync credentials between remote and local sources](https://docs.expo.dev/app-signing/syncing-credentials.md)
- [Security](https://docs.expo.dev/app-signing/security.md)
- [Apple Developer Program roles and permissions](https://docs.expo.dev/app-signing/apple-developer-program-roles-and-permissions.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Apple Developer Program roles and permissions for EAS Build

Learn about the Apple Developer account membership requirements for creating an EAS Build.

An Apple Developer account with permissions to create [app signing credentials](/app-signing/managed-credentials.md#generating-app-signing-credentials), such as certificates, identifiers, and provisioning profiles, is required when using EAS Build to create iOS device builds. These credentials can be generated when submitting the build by logging into your Apple Account from the EAS CLI, or they can be uploaded to your Expo account by an authorized user, so users without Apple Developer account access can create builds using the uploaded credentials.

On individual Apple Developer accounts, only the Account Holder role can generate app signing credentials. On an organization Apple Developer account, the Account Holder and Admin roles can always generate app signing credentials, and the App Manager role can generate credentials when a user with this role has **Access to Certificates, Identifiers, and Profiles** enabled in their App Store Connect user permissions.

Access to Certificates, Identifiers, and Profiles settings in App Store Connect.

This guide provides steps that an authorized user can follow to ensure app signing credentials are generated and available to their team members who use EAS. It also provides steps for the team developer to create an EAS Build by using pre-generated credentials.

> See [Apple's documentation on Program Roles](https://developer.apple.com/support/roles/) for details on the different roles and their permissions based on the type of Developer account and the permissions that are required for each role.

## Steps for Apple Developer account's authorized user

The authorized user of the Apple Developer account needs to generate the following credentials:

-   **Distribution signing certificate**: Required to sign development and release builds that are installed on an iOS device.
-   **Ad hoc provisioning profile**: Required to sign builds that are installed on a device outside of the Apple App Store.
-   **Distribution provisioning profile**: Required to sign the build that is submitted to the Apple App Store.
-   **Push key**: Required when using a push notification service.

For details on Distribution certificate, Provision profiles, and Push keys, see [required iOS app credentials](/app-signing/app-credentials.md#ios).

With EAS CLI, all of the above credentials can be created and synced automatically with the Apple Developer account. Once the authorized user logs in to their [Expo account](/accounts/account-types.md), they can create or update the provisioning profile by running `eas credentials` using the EAS CLI.

```sh
eas login
eas credentials
```

The CLI will prompt for selecting a [build profile](/build/eas-json.md#build-profiles) to use for the EAS Build. If the Apple Developer account's authorized user is creating a production build, follow these steps to [create a distribution provisioning profile](/tutorial/eas/ios-production-build.md#create-a-distribution-provisioning-profile). To create a developer build, follow these steps to [create an ad hoc provisioning profile](/tutorial/eas/ios-development-build-for-devices.md#provisioning-profile).

This ensures that the provisioning profile associated with the Expo account has necessary permissions.

> For projects with existing credentials, see [Using existing credentials](/app-signing/existing-credentials.md) for details on how to sync these to EAS or manage them manually.

## Steps for the team developer

As a developer on the team, when running `eas build -p ios` in the terminal window, the EAS CLI asks you to login to an Apple Developer account.

```sh
? Do you want to log in to your Apple account? > (Y/n)
No problem! 👌 If any of the next steps will require Apple account access we will ask you again about it.
```

Press N to skip logging into Apple Developer account if you don't have access (and avoid logging into your personal Apple Developer account, if any). The CLI displays message about skipping provisioning profile validation and other app signing credential validation and will continue creating the EAS Build with existing credentials

The EAS CLI needs to use the provisioning profile associated with the Expo account to create a build for iOS. When you skip login, the EAS Build will use the last provisioning profile and other credentials that were updated by the Apple Developer account's authorized user in your organization's Expo account.

## Additional information

### Uploading pre-generated Apple credentials

Some development teams may choose to generate distribution certificates and provisioning profiles outside of EAS. These credentials can be added by any EAS user with Developer or higher permissions using `eas credentials` or under **Select your project** > **Project settings** > **Configuration** > **Credentials** using the EAS dashboard.

When uploading the credentials, you will need the **.p12** and **.mobileprovision** files, and any passwords set when generating the distribution certificate.

### Provisioning profile expiry and updates

The associated provisioning profile needs to be updated if certain [iOS capabilities](/build-reference/ios-capabilities.md) (such as, entitlements) are added or removed, or at the annual expiry of the profile. This step is handled by the Apple Developer account's authorized user.

### Federated Apple Developer accounts

#### EAS Build

EAS CLI can only accept an Apple account's email and password to login into your Apple Developer account. You cannot login into [Federated Apple Developer account](https://support.apple.com/en-in/guide/apple-business-manager/axmb19317543/web) and make updates to the distribution certificate or provisioning profile. If your build credentials do not require any changes, you can skip logging in. Then, you can proceed with the build and EAS CLI will continue using your current uploaded credentials.

However, you can provide an Apple Store Connect (ASC) API token with Admin access to check and update Apple credentials when running `eas build` command. Follow the steps in [Provide an ASC API token for your Apple team](/build/building-on-ci.md#optional-provide-an-asc-api-token-for-your-apple-team) to create a build by passing the required token value to the `eas build` command.

#### EAS Submit

EAS Submit uses the ASC API token for submitting to TestFlight. If you have a Federated Apple Developer account, you can follow the standard EAS Submit setup. It lets you automatically submit your builds using `eas build --auto-submit`.
