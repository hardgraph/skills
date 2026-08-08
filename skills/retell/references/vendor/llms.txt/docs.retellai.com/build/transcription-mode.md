> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Balance between transcription accuracy and latency

> Pick the right Retell transcription mode to balance accuracy against latency — interim results for speed or context-aware results for higher accuracy.

<Note> This guide only applies to cascading agents, if you are using speech to speech models, this feature does not apply. </Note>

Real time transcription is often a trade off between latency and accuracy. When relying on interim results, you get the lowest latency but with a higher chance of errors due to less context. When relying on results generated with more context, you risk waiting longer after the user stops speaking.

## Transcription modes

<img src="https://mintcdn.com/retellai/pRGcctz_zOqy0mSt/images/transcription-mode.jpeg?fit=max&auto=format&n=pRGcctz_zOqy0mSt&q=85&s=46447194022d07992db5fc65dc2b931d" width="254" height="108" data-path="images/transcription-mode.jpeg" />

* optimize for speed: uses the latest interim results with a low endpointing setting for downstream processing.
* optimize for accuracy: uses the results with a higher endpointing setting for downstream processing, essentially waiting longer with more context to generate more accurate transcripts. It will incur \~200ms latency.

## Which mode to use?

From our benchmarking, we found that the `optimize for speed` mode and `optimize for accuracy` mode have similar WER (Word Error Rate). The difference mainly lies in capturing entities like numbers and dates. If your use case relies heavily on capturing these entities well, you should use the `optimize for accuracy` mode. Otherwise you can use the `optimize for speed` mode for best latency.
