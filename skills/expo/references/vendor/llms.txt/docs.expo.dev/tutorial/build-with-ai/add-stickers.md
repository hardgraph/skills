---
modificationDate: July 08, 2026
title: Add stickers
description: In this chapter, direct your AI agent to add an emoji picker modal and make the sticker respond to drag and tap gestures.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/build-with-ai/add-stickers/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/build-with-ai/add-stickers/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > Build with AI tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/build-with-ai/introduction.md)
- [Set up your tools](https://docs.expo.dev/tutorial/build-with-ai/set-up-your-tools.md)
- [Create your first app](https://docs.expo.dev/tutorial/build-with-ai/create-your-first-app.md)
- [Build the home screen](https://docs.expo.dev/tutorial/build-with-ai/build-the-home-screen.md)
- [Add stickers](https://docs.expo.dev/tutorial/build-with-ai/add-stickers.md) (this page)
- [Save your creation](https://docs.expo.dev/tutorial/build-with-ai/save-your-creation.md)
- [Finishing touches](https://docs.expo.dev/tutorial/build-with-ai/finishing-touches.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Add stickers

In this chapter, direct your AI agent to add an emoji picker modal and make the sticker respond to drag and tap gestures.

In this chapter, the app earns its name: you'll pick an emoji from a sliding panel, smash it onto your photo, and move it around with your fingers.

## Add the emoji picker

Once a photo is chosen, the app should switch from "picking a photo" mode to "decorating it" mode. Paste the following prompt into your agent:

```text
When a photo has been chosen (either picked from the library or by tapping "Use this photo"), replace the two buttons with a row of options: a "Reset" option on the left that brings back the original buttons, and a circular "+" button in the middle. Tapping "+" should slide up a modal from the bottom titled "Choose a sticker" that shows the emoji images from the assets/images folder in a horizontally scrolling list. When I tap an emoji, close the modal and place that emoji on top of the photo as a sticker.
```

**What you should see**: after choosing a photo, the buttons change to the new option row. Tapping **+** slides up a panel of emoji; you can scroll the list sideways. Tapping one closes the panel and the emoji appears on your photo:

## Move and resize the sticker

A sticker stuck in one spot isn't much fun. Paste the following prompt into your agent:

```text
Make the sticker interactive using the react-native-gesture-handler and react-native-reanimated libraries: I want to drag the sticker around with my finger to place it anywhere on the photo, and double-tap it to double its size (double-tapping again should shrink it back). The movement and resizing should animate smoothly.
```

**What you should see**: drag the emoji around the photo with your finger, and double-tap it to grow and shrink it.

This step is a good reminder of your role: the agent can read its own code, but it can't feel a gesture. Only you can confirm that dragging feels right. If the sticker lags, jumps, or snaps back, describe exactly that to the agent.

> If a result doesn't match what you expected, tell the agent what you see. The [When something looks wrong](/tutorial/build-with-ai/create-your-first-app.md#when-something-looks-wrong) section has the playbook.

## Summary

Chapter 4: Add stickers

Your photo now wears a draggable, resizable emoji sticker. All that's missing is a way to keep your creation.

In the next chapter, you'll save the decorated photo to your phone's photo library.

[Next: Chapter 5: Save your creation](/tutorial/build-with-ai/save-your-creation.md)
