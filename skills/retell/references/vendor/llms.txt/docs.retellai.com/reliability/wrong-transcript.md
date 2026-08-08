> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Increase transcription accuracy

> Increase Retell transcription accuracy — use boosted keywords for niche terms, choose the right ASR provider, and tune transcription modes for clarity.

We take transcription quality seriously and understand its crucial importance for our customers. Our transcription accuracy primarily depends on the AI models we use, carefully selected to balance both accuracy and processing speed.

### Common Transcription Issues and Solutions

#### 1. Wrong Transcript for Special Words or Terms

**Issue**: Specific words (like "retell") or domain-specific terms (such as medical terminology) are missing from transcripts.

**Solution**: Use Boosted Keywords

* Add custom keywords to enhance the model's vocabulary
* Support for up to 100 custom keywords

<Frame>
  <img height="700" src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/reliability/images/boosted_keywords.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=c5c601a35e7783783215a1b26c0626c3" alt="Boosted Keywords Configuration" data-path="reliability/images/boosted_keywords.png" />
</Frame>

#### 2. Transcription error due to background noise / speech

Play with [denoising mode setting](/build/handle-background-noise) to see if it helps.

#### 3. Transcription error due to sentence being cut off

Sometimes the transcription quality can be impacted if the sentence was cut off (the transcription spits out the finalized sentence before it should). In this case, you can turn on [transcription mode](/build/transcription-mode) to be optimized for accuracy.

## FAQ

<AccordionGroup>
  <Accordion title="The agent is missing short responses like 'sure' or 'yes'. How do I fix this?">
    If the background noise level is not particularly high, this is often caused by the denoising mode filtering out short, low-energy responses. Try setting the [denoising mode](/build/handle-background-noise) to **No Denoising** — this preserves more of the raw audio signal and can significantly improve ASR accuracy for brief utterances when the environment is relatively quiet.
  </Accordion>
</AccordionGroup>
