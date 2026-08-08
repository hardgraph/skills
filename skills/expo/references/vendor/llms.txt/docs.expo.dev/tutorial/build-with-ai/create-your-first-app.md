---
modificationDate: July 08, 2026
title: Create your first app
description: In this chapter, direct your AI agent to create a new Expo app and see it running on your phone.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/tutorial/build-with-ai/create-your-first-app/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/tutorial/build-with-ai/create-your-first-app/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Learn > Build with AI tutorial
Pages in this section:
- [Introduction](https://docs.expo.dev/tutorial/build-with-ai/introduction.md)
- [Set up your tools](https://docs.expo.dev/tutorial/build-with-ai/set-up-your-tools.md)
- [Create your first app](https://docs.expo.dev/tutorial/build-with-ai/create-your-first-app.md) (this page)
- [Build the home screen](https://docs.expo.dev/tutorial/build-with-ai/build-the-home-screen.md)
- [Add stickers](https://docs.expo.dev/tutorial/build-with-ai/add-stickers.md)
- [Save your creation](https://docs.expo.dev/tutorial/build-with-ai/save-your-creation.md)
- [Finishing touches](https://docs.expo.dev/tutorial/build-with-ai/finishing-touches.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Create your first app

In this chapter, direct your AI agent to create a new Expo app and see it running on your phone.

In this chapter, your agent creates a new Expo app, you get it running on your phone, and you make your first change. This is the loop you'll repeat for the rest of the tutorial.

## Start your agent in a new folder

Your agent works inside one folder at a time, so create a dedicated folder for this project and start the agent there. In your terminal, run:

```sh
mkdir StickerSmash
cd StickerSmash
```

Then start your agent in that folder: run `claude` or `codex`. If you're using Cursor, create a new folder named **StickerSmash**, open it with **File** > **Open Folder**, and open the agent panel.

## Create the app

Paste the following prompt into your agent:

```text
Create a new Expo app in this folder (the current directory) by running npx create-expo-app@latest and choosing the SDK 54 template — I will test the app with Expo Go on my phone, which uses SDK 54. After it is created, run the project's reset-project script so we start from a minimal app, and delete the app-example folder that the script leaves behind. Don't start the development server; I will run that myself.
```

The agent takes a few minutes to download the template and install its dependencies. When it finishes, it should report that the project is ready, with a minimal **app** folder containing a couple of files.

## Run the app on your phone

This is the one command you'll run yourself throughout the tutorial. It starts the development server that streams your app to your phone. Open a **new terminal window** (leave your agent running in the first one), then run:

```sh
cd StickerSmash
npx expo start
```

A QR code appears in the terminal. To open the app:

-   **Android**: open Expo Go and tap **Scan QR code**.
-   **iOS**: open the default camera app and point it at the QR code.

You should see a mostly empty screen on your phone. That's your app. Keep this terminal window open and your phone nearby for the rest of the tutorial.

#### Phone can't connect?

Your phone and computer must be on the same Wi-Fi network. If they are and the app still doesn't load, stop the server with Ctrl + C and restart it in tunnel mode, which works across networks:

```sh
npx expo start --tunnel
```

You can also find your project under the **Projects** tab in Expo Go when you're signed in to the same Expo account on your phone and in the terminal.

## Make your first change

Now close the loop: ask the agent for a visible change and watch it land on your phone. Paste the following prompt into your agent:

```text
Change the home screen so it shows the text "Home screen" in white, centered on a dark background with the color #25292e.
```

**What you should see**: within seconds of the agent finishing, the app on your phone reloads to a dark screen with white "Home screen" text in the middle:

That's the whole workflow — you prompt, the agent builds, your phone shows the result.

## When something looks wrong

Sooner or later a result won't match what you asked for. That's normal, and fixing it is part of the workflow:

-   **Describe, don't diagnose.** Tell the agent exactly what you see and what you expected: "The text is centered but the background is still white." You don't need to guess why.
-   **Paste errors verbatim.** If your phone shows a red error screen or the terminal prints an error, copy the whole message into the agent. Error messages are written for programmers, and your agent is one.
-   **Ask the agent to check its work.** A prompt like "Something is broken — review the change you just made, find the problem, and fix it" goes a long way.
-   **If the app gets stuck**, shake your phone and tap **Reload** in the menu that appears. If that doesn't help, stop the development server with Ctrl + C and run `npx expo start` again.

The next chapters end with a short reminder linking back to this section.

## Summary

Chapter 2: Create your first app

You created an app, ran it on your phone, and changed it with a single prompt.

In the next chapter, you'll turn this empty screen into the real thing: navigation tabs, the photo viewer, and an image picker.

[Next: Chapter 3: Build the home screen](/tutorial/build-with-ai/build-the-home-screen.md)
