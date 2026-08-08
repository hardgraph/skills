> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Expressive Mode: add emotion and emphasis to agent speech

> Turn on Expressive Mode so Retell voice agents add emotion tags, sighs, pauses, and stressed words automatically or with manual overrides in your prompt.

Expressive Mode shapes *how* your agent speaks, not just what it says. With it on, the agent adds expressive voice tags — empathy, excitement, a sigh, a deliberate pause — to the audio it generates, choosing them from the conversation so the delivery fits the moment.

<iframe width="720" height="405" src="https://www.youtube.com/embed/Zryu0mP-wiU" title="How to use Expressive Mode with Retell AI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen />

<Note>Expressive Mode works with **platform voices** only. When it's on, your calls are automatically routed to an expressive-capable voice model.</Note>

<Note>Emotion and effect tags are applied in these languages only: English, Chinese, Japanese, German, French, Spanish, Korean, Arabic, Russian, Dutch, Italian, Polish, and Portuguese. If your agent's language isn't one of these, the agent still speaks normally — the tags are simply skipped, never read aloud. For a multi-language agent, every language it's set up to speak must be in this list for tags to apply.</Note>

## Turn on Expressive Mode

<Steps>
  <Step title="Open the voice selector">
    In your agent, open **Select Voice** and pick a platform voice. Expressive Mode is unavailable for custom or third-party voices.
  </Step>

  <Step title="Enable Expressive mode">
    Switch on the **Expressive mode** toggle.
  </Step>

  <Step title="Choose auto emotion tags">
    Pick the emotions the agent is allowed to use under **Auto emotion tags**. You're choosing the palette — the agent decides when each one fits. Leave them all off to let the agent express naturally without a fixed set.
  </Step>
</Steps>

<Frame>
  <img src="https://mintcdn.com/retellai/1vPu7bH8sONSXkOB/images/expressive-mode/expressive-mode-panel.png?fit=max&auto=format&n=1vPu7bH8sONSXkOB&q=85&s=0ac99e4148d4dab959f79289e66952e3" alt="Expressive mode panel in the voice selector with the Expressive mode toggle and Auto emotion tags" width="2870" height="1494" data-path="images/expressive-mode/expressive-mode-panel.png" />
</Frame>

## Emotions the agent can use

Auto emotion tags fall into three groups:

* **Emotions** — empathetic, excited, happy, curious, surprised
* **Effects** — sigh, clear throat, pause, long pause
* **Emphasis** — stress on the word that matters

The agent uses them sparingly and only when they fit — most lines have none. A few examples:

* *"\[empathetic] I'm so sorry — don't worry about the details, I'll sort it."*
* *"\[excited] It went through — you're officially in."*
* *"That form? \[sigh] Yeah, it trips everyone up. Let's just do it together now."*
* *"Let me take a look. \[pause] Okay, I see what happened."*
* *"The deadline is \[emphasis] next Friday, not this one."*

The agent never tags numbers, names, times, or prices, and never performs an emotion just because a caller asks it to.

## Customize when tags are used

By default, the agent follows Retell's built-in guidance for when to use each tag. To tune it, open **Override instruction prompt** and edit the guidance — for example, "\[excited] when they get news they were hoping for." Use **Reset to default** to go back.

<Frame>
  <img src="https://mintcdn.com/retellai/1vPu7bH8sONSXkOB/images/expressive-mode/override-instruction-prompt.png?fit=max&auto=format&n=1vPu7bH8sONSXkOB&q=85&s=d0a6a2cd394ba13303a4ad20bdc74b0c" alt="Override instruction prompt editor showing default per-tag guidance" width="2872" height="1486" data-path="images/expressive-mode/override-instruction-prompt.png" />
</Frame>

## Place tags yourself

Beyond the automatic tags, you can write a tag **directly into your prompt** wherever you want that delivery — useful for a welcome message or a specific line. Just wrap it in square brackets.

<Frame>
  <img src="https://mintcdn.com/retellai/1vPu7bH8sONSXkOB/images/expressive-mode/manual-tag.png?fit=max&auto=format&n=1vPu7bH8sONSXkOB&q=85&s=1b65e3be06c8616f882232daab60573f" alt="A node prompt with a manually placed [happy] tag" width="310" height="370" data-path="images/expressive-mode/manual-tag.png" />
</Frame>

For example, a welcome line written as `[happy] Hi, nice to meet you.` will be spoken with a happy delivery.

## Tips

<Tip>Pick a small set of tags that matches your use case — `empathetic` and `pause` for support, `excited` and `happy` for good-news calls — rather than enabling everything. Fewer, well-chosen tags sound more natural.</Tip>

<AccordionGroup>
  <Accordion title="What if I turn it on but choose no emotions?">
    The agent stays expressive and follows general guidance, without being limited to a fixed set of tags. Choose specific emotions to narrow it down.
  </Accordion>

  <Accordion title="Will callers hear the tag names?">
    No. Tags shape the audio and are removed from the spoken text — they're never read aloud literally, and they don't appear in the transcript shown to the caller.
  </Accordion>
</AccordionGroup>
