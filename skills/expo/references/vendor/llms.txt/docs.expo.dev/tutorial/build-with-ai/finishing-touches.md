---
modificationDate: July 08, 2026
title: Finishing touches
description: In this chapter, direct your AI agent to polish the status bar, splash screen, and app icon, then take your next steps as a builder.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/build-with-ai/finishing-touches/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/build-with-ai/finishing-touches/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

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
- [Save your creation](https://docs.expo.dev/tutorial/build-with-ai/save-your-creation.md)
- [Finishing touches](https://docs.expo.dev/tutorial/build-with-ai/finishing-touches.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Finishing touches

In this chapter, direct your AI agent to polish the status bar, splash screen, and app icon, then take your next steps as a builder.

The app works. Now make it feel finished. In this chapter, you'll polish the details users notice without realizing it: the status bar, the splash screen, and the app icon.

## Fix the status bar

Look at the very top of your app: the clock and battery icons are dark, which makes them hard to read against the dark background. Paste the following prompt into your agent:

```text
The phone's status bar (the clock and battery icons at the top of the screen) is hard to read against our dark background. Use the expo-status-bar library to make the status bar text light on every screen.
```

**What you should see**: the clock, battery, and signal icons at the top of the screen are now light and readable:

## Set the app icon and splash screen

The asset pack you downloaded earlier includes an app icon and a splash screen image. Paste the following prompt into your agent:

```text
Set the app's icon to assets/images/icon.png, and configure the splash screen using the expo-splash-screen config plugin so it shows assets/images/splash-icon.png centered on a #25292e background on Android, iOS, and web.
```

**What you should see**: these settings live in the app's configuration, so restart the development server to apply them: press Ctrl + C in the terminal, run `npx expo start` again, and reopen the app from Expo Go. You'll briefly see the splash screen while the project loads.

One caveat: the app icon on your home screen won't change, because you're running your project inside Expo Go, which is its own app with its own icon. Your icon takes over when you create a standalone build of your app. See [development builds](/develop/development-builds/introduction.md) when you're ready for that step.

> If a result doesn't match what you expected, tell the agent what you see — the [When something looks wrong](/tutorial/build-with-ai/create-your-first-app.md#when-something-looks-wrong) section has the playbook.

## You built an app

Take a second to appreciate what happened here: you set up a development environment from nothing, then directed an AI agent to build a real app with navigation, photo picking, gesture-driven stickers, and saving (for Android, iOS, and the web) without writing code.

More importantly, you learned the loop: prompt, build, verify, re-prompt. That loop doesn't end with this tutorial. Try it on your own ideas right now:

```text
Let me place more than one sticker on the photo.
```

```text
Add a share button that opens the system share sheet so I can send my creation to friends.
```

```text
Fill in the About screen: explain what the app does and credit me as the builder.
```

## Next steps

[Expo Skills](/skills.md) — Browse the full list of skills your agent can use, with example prompts for each.

[Expo MCP server](/mcp.md) — Explore everything your agent can do with Expo's tools, including triggering builds and reading crash reports.

Need help or want to show off what you built? Join the Expo community on [Discord](https://chat.expo.dev/).

## Summary

Chapter 6: Finishing touches

Congratulations! You built StickerSmash from scratch by directing an AI agent, and polished it with a status bar, splash screen, and app icon.

Curious what your agent actually wrote? The Expo tutorial builds the same app, explaining the code one step at a time.

[Next: Expo tutorial](/tutorial/introduction.md)
