---
modificationDate: July 08, 2026
title: Save your creation
description: In this chapter, direct your AI agent to save the decorated photo to your phone, and optionally make it work in the browser too.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/build-with-ai/save-your-creation/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/build-with-ai/save-your-creation/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > Build with AI tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/build-with-ai/introduction.md)
- [Set up your tools](https://docs.expo.dev/tutorial/build-with-ai/set-up-your-tools.md)
- [Create your first app](https://docs.expo.dev/tutorial/build-with-ai/create-your-first-app.md)
- [Build the home screen](https://docs.expo.dev/tutorial/build-with-ai/build-the-home-screen.md)
- [Add stickers](https://docs.expo.dev/tutorial/build-with-ai/add-stickers.md)
- [Save your creation](https://docs.expo.dev/tutorial/build-with-ai/save-your-creation.md) (this page)
- [Finishing touches](https://docs.expo.dev/tutorial/build-with-ai/finishing-touches.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Save your creation

In this chapter, direct your AI agent to save the decorated photo to your phone, and optionally make it work in the browser too.

In this chapter, you'll add the final core feature: saving the photo, sticker and all, to your phone's photo library.

## Save to your photo library

Paste the following prompt into your agent:

```text
Add a "Save" option to the row of options, with a download icon. When I tap it, capture just the photo with the sticker on it (not the whole screen) using the react-native-view-shot library, and save the result to my phone's photo library using the expo-media-library library. Ask for permission the first time if needed, and show an alert confirming that the image was saved.
```

**What you should see**: tapping **Save** asks for permission to save to your photos the first time. Tap **Allow**, and the app shows a confirmation. Open your phone's photo gallery: your photo is there with the sticker baked in, ready to share anywhere.

## Make it work on the web (optional)

Your app isn't only a phone app. The same project runs in a web browser. In the terminal window running the development server, press W and the app opens in your browser. Everything works, except **Save**: the library that captures the image on phones doesn't work on the web.

#### Want to fix the Save option on the web?

Paste the following prompt into your agent:

```text
Make the Save option also work when the app runs in a web browser. The react-native-view-shot library doesn't support the web, so when the platform is web, use the dom-to-image library to capture the image and download it as a file instead.
```

**What you should see**: in the browser, pick a photo, add a sticker, and click **Save**. The image downloads as a file. This is how platform differences are handled in Expo apps: one app, with small platform-specific branches where needed.

> If a result doesn't match what you expected, tell the agent what you see. The [When something looks wrong](/tutorial/build-with-ai/create-your-first-app.md#when-something-looks-wrong) section has the playbook.

## Summary

Chapter 5: Save your creation

StickerSmash is feature-complete: pick a photo, smash a sticker on it, and save the result, on your phone and even in the browser. One chapter to go.

In the next chapter, you'll polish the status bar, splash screen, and app icon, and take your next steps.

[Next: Chapter 6: Finishing touches](/tutorial/build-with-ai/finishing-touches.md)
