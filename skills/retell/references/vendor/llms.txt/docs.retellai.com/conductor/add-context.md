> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Give Conductor context

> Attach conversation steps, past calls, test cases, and files to your Conductor messages so its suggestions are accurate and tailored to your Retell agent.

Conductor gives better suggestions when it knows exactly what you're referring to. You can attach context to any message — a specific step, a real call, a test case, or a file — so Conductor works from the right information.

## How to attach context

1. In the Conductor message box, click the **attach** (+) button.
2. Choose what you want to add.
3. Pick the specific item from the list.
4. Type your message and send. The attached item appears as a tag on your message.

To attach files, you can also **drag and drop** them directly onto the message box — see [A file](#what-you-can-attach) below.

<Frame>
  <img src="https://mintcdn.com/retellai/3a75AO214qaM767u/images/conductor/conductor-attach-menu.png?fit=max&auto=format&n=3a75AO214qaM767u&q=85&s=307bbc3eab052187af911c606e1cc7d8" alt="The attach menu in the Conductor message box" width="690" height="636" data-path="images/conductor/conductor-attach-menu.png" />
</Frame>

## What you can attach

<AccordionGroup>
  <Accordion title="A conversation step (node)">
    Point Conductor at a specific step in your agent so it edits the right place.

    *Example:* attach the "Collect email" step and say "Make this step confirm the spelling back to the caller."
  </Accordion>

  <Accordion title="A past call">
    Attach a real call so Conductor can see what actually happened and suggest improvements.

    *Example:* attach a call where things went wrong and ask "Why did the agent end the call here, and how do I fix it?"
  </Accordion>

  <Accordion title="A test case">
    Attach a saved test case to refine how your agent handles that scenario.

    *Example:* attach a test case and say "Make sure the agent passes this every time."
  </Accordion>

  <Accordion title="A file">
    Upload a file (such as a document or screenshot) to give Conductor extra information to work from.

    You can add files in two ways: click the **attach** (+) button and choose your files, or **drag and drop** them straight onto the message box. While you drag files over it, a **Drop files to attach** overlay appears; release to attach them.

    **Supported file types:** images (JPG, JPEG, PNG, GIF, WebP), PDF, and text-based files (TXT, CSV, HTML, HTM, MD, RTF, JSON).

    **Limits:** up to 10 files per message, and each file must be 50 MB or smaller. If a file isn't a supported type, is too large, or would exceed the 10-file limit, Conductor shows a message explaining which files it couldn't attach.
  </Accordion>
</AccordionGroup>

## Tips

* **Attach the call or step you're talking about** instead of describing it — it's faster and more accurate.
* **You can attach more than one item** to a single message.
* **Remove an attachment** before sending by clicking the **x** on its tag.

<Note>
  Steps and test cases shown in the picker come from the agent you currently have open. Calls come from that agent too — unless you're on the [home page](/conductor/home), where calls (and an **Agents** tab) span your whole workspace instead of a single agent.
</Note>
