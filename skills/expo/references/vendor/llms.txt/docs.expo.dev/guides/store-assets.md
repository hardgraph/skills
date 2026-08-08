---
modificationDate: July 29, 2026
title: Create app store assets
description: Learn how to create screenshots and previews for your app's store pages.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/store-assets/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/store-assets/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Assorted
Pages in this section:
- [Authentication with OAuth or OpenID providers](https://docs.expo.dev/guides/authentication.md)
- [Using Hermes](https://docs.expo.dev/guides/using-hermes.md)
- [iOS Developer Mode](https://docs.expo.dev/guides/ios-developer-mode.md)
- [Expo Vector Icons](https://docs.expo.dev/guides/icons.md)
- [Localization](https://docs.expo.dev/guides/localization.md)
- [Using Bun](https://docs.expo.dev/guides/using-bun.md)
- [Edit rich text](https://docs.expo.dev/guides/editing-richtext.md)
- [App store assets](https://docs.expo.dev/guides/store-assets.md) (this page)
- [Local-first](https://docs.expo.dev/guides/local-first.md)
- [Keyboard handling](https://docs.expo.dev/guides/keyboard-handling.md)
- [Controlled components](https://docs.expo.dev/guides/controlled-components.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create app store assets

Learn how to create screenshots and previews for your app's store pages.

Before submitting your app to the Google Play Store and Apple App Store, you need to provide some assets for your store listing page. The goal of these images and videos is to give your prospective users an idea of what your app experience is going to be like.

You will need to upload app screenshots for both app stores. Even though they are called "app screenshots" on both stores, they _do_ have to include accurate visuals of your app. There is no rule citing that these images have to be screenshots taken on specific devices.

Both app stores have requirements on image format and size. However, within these restrictions, you can be creative. For example, a common approach is to design the assets using a design tool such as Figma and incorporate actual app screenshots (or designs) with supplementary messaging.

## Different approaches for creating "screenshots"

There are three commonly used approaches to creating your store screenshots. You can choose whichever approach works best for your app's needs and resources.

### Option 1: actual screenshots

The most straightforward option — open up your app on a real device and take screenshots.

**Pros**: Straightforward to do. Most accurate representation of your app.

**Cons**: Need to load the app on different devices for a full range of screenshots.

Screenshots of the Expo Go Apple App Store listing page.

### Option 2: screenshots within a design

The majority of apps use this approach. It involves taking screenshots of the app (or in some cases, using existing designs instead of actual screenshots) and embedding them in the store assets along with appropriate messaging.

**Pros:** Allows to convey additional messaging within the assets.

**Cons:** The assets need to be created using a design program.

Screenshots of the Brex Apple App Store listing page.

### Option 3: make it fancy

In this option, on the app store pages, you can incorporate elements of your app design and creative messaging to highlight your product.

**Pros:** You can make your store page creative and fun.

**Cons:** Need an experienced designer to create and maintain the assets.

Screenshots of the MS Office Apple Store listing page.

## Google Play Store asset requirements

Google has specific requirements for the store asset format and dimensions, which are different from Apple's. For the most up-to-date specifications, see [official documentation](https://support.google.com/googleplay/android-developer/answer/9866151) for detailed requirements for your Google Play Store assets.

[Store assets Figma template](https://www.figma.com/community/file/1352686667495694112) — See our template for a summary of the minimum asset requirements.

### App icon

Unlike on the Apple App Store, where the app icon is always taken automatically from the app bundle, on the Google Play Store, you must also upload a separate App Icon for your store listing.

### Feature graphic

A feature graphic must be provided to publish your Store Listing. It is a banner that is displayed at the top of your store listing page.

### Screenshots

You need to upload at least four screenshots to publish your app.

### Video (optional)

You can add one preview video to your Store Listing. The video needs to be uploaded to YouTube, and you can add it by entering a YouTube URL in the **preview video** field.

## iOS App Store asset requirements

For the iOS App Store, you can upload screenshots (images) and previews (videos). For each, Apple requires specific widths and heights. Make sure to reference the [Screenshot specifications from Apple](https://developer.apple.com/help/app-store-connect/reference/screenshot-specifications/) for the exact sizing. Even a single pixel-off will mean you cannot submit the images.

[Store assets Figma template](https://www.figma.com/community/file/1352686667495694112) — See our template for a summary of the minimum asset requirements.

### Screenshots

At a minimum, Apple requires you to upload screenshots for iPhone with the dynamic island (6.9 inch). You can optionally upload additional screenshots for other screen sizes. If specific screenshots are not provided, scaled down screenshots from the closest uploaded size will be used instead.

If your app runs on iPad, you must also provide one set of iPad screenshots (13 inch).

You can upload up to ten screenshots per localization. If your app is available in multiple languages and your screenshots include text, you should upload screenshots with the appropriate language for each localization.

Screenshots can be portrait or landscape.

### Preview (optional)

You can include An app preview video to demonstrate how your app works. You can add up to three app previews for each screen size.

See [App Preview Specifications](https://developer.apple.com/help/app-store-connect/reference/app-preview-specifications/) from Apple documentation for a summary of video size and format.

## Bare minimum

Below is the bare minimum needed to publish your apps.

### Play Store - Android

| Type | Amount | Dimensions | Requirements |
| --- | --- | --- | --- |
| App Icon | 1 | 512 × 512 | 32-bit PNG (with alpha); Maximum file size: 1024 KB |
| Feature Graphic | 1 | 1024 × 500 | JPEG or 24-bit PNG (no alpha) |
| Screenshots | 4-10 | minimum: 1024 × 500maximum width: 3840px9:16 aspect ratio | JPEG or 24-bit PNG (no alpha) |

### App Store - iPhone

| Type | Amount | Dimensions (choose one) | Requirements |
| --- | --- | --- | --- |
| Screenshots (iPhone with the dynamic island) | 2-10 | 1320 × 28681290 × 2796 | JPG or PNG (no alpha) |

### App Store - iPad

If your app also runs on an iPad, you need to supply additional screenshots.

| Type | Amount | Dimensions (choose one) | Requirements |
| --- | --- | --- | --- |
| Screenshots | 2-10 | 2064 × 27522048 × 2732 | JPG or PNG (no alpha) |
