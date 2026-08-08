> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Address metric issues

> Step-by-step fixes for common AI QA metric issues — hallucinations, knowledge base mistakes, overlapping speech, latency, sentiment, and tool errors.

## Overview

When AI QA analyzes your calls, it flags specific metrics that didn't meet expectations. This page provides actionable guidance for addressing each type of metric issue to improve your agent's performance. For detailed definitions of each metric, see [AI QA metrics](/ai-qa/terminologies).

## AI Accuracy

### High Agent Hallucination Rate

When the agent generates incorrect or fabricated information not supported by the conversation context or knowledge base.

**How to fix:** Use the call QA sheet to see whether each instance is Fabrication, Contradiction, or Confusion, then apply the right fix:

* **Fabrication** (inventing facts): Add the correct information to your knowledge base or system prompt so the agent has it instead of guessing
* **Contradiction** (conflicting with provided info): Simplify or clarify conflicting instructions in your system prompt
* **Confusion** (misunderstanding user intent): Break complex instructions into simpler steps, or use conversation flow nodes with focused prompts

### Low KB (Knowledge Base) Recall

When relevant knowledge base chunks are not being retrieved when they should be.

**How to fix:**

* Reduce the KB retrieval threshold and increase the number of chunks to reduce false negatives (missed relevant chunks)
* Adjust these in your agent's [Knowledge Base](/build/knowledge-base) configuration; make small changes and monitor impact in later QA runs

## Response-engine issues

### High Node Transition Inaccuracy

When node transitions are inaccurate, the agent is moving to the wrong conversation state. This problem is specific to conversation flow agents, because only that engine type uses nodes and transitions between them.

**How to fix:**

* Clarify the transition conditions in your conversation flow node prompts
* Add examples that demonstrate the correct transition behavior for edge cases
* Keep transition prompts unambiguous and avoid overlapping conditions between nodes; see the [conversation flow debug guide](/build/conversation-flow/debug-guide) for a step-by-step walkthrough

### High Tool Call Inaccuracy

When the agent calls the wrong tools, misses required tool calls, or passes incorrect arguments. This problem is specific to single-prompt and multi-prompt agents, because only those engine types decide freely when and which tools to call.

**How to fix:**

* In your agent prompt, spell out when to call which tools (and when not to)
* In your [tool definitions](/build/add-function-calling), use clear names and descriptions and add examples for parameters so the agent can choose and invoke tools correctly

<Info>
  Tool Call Inaccuracy measures whether the agent made the right **decision** about which tools to call and with what arguments. For issues with tool **execution** failing (e.g., endpoint errors), see [Custom Tool Failures](#custom-tool-failures) below.
</Info>

## Speech Quality

### Poor Agent Naturalness

When the agent sounds unnatural — issues like mispronunciation, robotic pacing, or audio artifacts.

**How to fix:**

* **Change voice**: Custom-cloned voices tend to have more naturalness issues; switching to a platform voice often improves stability
* **Adjust voice temperature**: Change the temperature setting to affect vocal expressiveness
* **Switch voice provider**: Different providers have different strengths (e.g., naturalness vs. specific languages or accents)

### Poor Agent Sentiment

When the agent's responses carry negative or inappropriate emotional tone.

**How to fix:**

* Adjust your system prompt with explicit tone guidelines (e.g., "respond warmly and helpfully")
* If using a conversation flow, check whether any node prompts produce overly terse or cold responses
* Reword dismissive phrases (e.g., "I can't help with that" → "Let me find another way to help")

## Transcription Quality

### High Word Error Rate (WER)

When the speech-to-text transcription has a high error rate, causing the agent to misunderstand what the user said.

**How to fix:**

* **Switch STT provider**: Choose a higher-accuracy speech-to-text provider for your use case
* **Check language settings**: Ensure the language setting matches the actual spoken language — mismatched settings significantly increase WER
* **Add custom vocabulary**: If your STT provider supports it, add frequently used names, technical terms, or domain-specific words as boosted keywords
* **Use Mistranscribed Entities feedback**: Review the mistranscribed entities surfaced by AI QA and add those specific terms as [boosted keywords](/reliability/wrong-transcript) in your STT configuration
* **Reduce background noise**: If the call environment is too noisy, try turning on background removal to improve transcription accuracy

## User Experience

### High User Negative Sentiment

When multiple user utterances show negative sentiment, the agent may not be handling the conversation well.

**How to fix:**

* Adjust your agent's system prompt to encourage more empathetic, friendly responses
* Add instructions for handling frustrated users (e.g., acknowledge concerns before offering solutions)

### High Overlapping Speech Count

Frequent overlapping speech may indicate latency or responsiveness issues. The fix depends on whether latency is also a problem:

| Scenario                          | How to fix                                                                                                                                                           |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **High latency (e2e p50 > 2.5s)** | Fix latency first. Choose faster models and lower-latency voice providers. When the agent takes too long to respond, users are more likely to talk over it.          |
| **Normal latency**                | Decrease agent responsiveness or increase interruption sensitivity. The agent may be starting to speak too quickly or not detecting when the user wants to continue. |

## Tool Execution

### Custom Tool Failures

When custom tool calls fail during a call.

**How to fix:**

* Check your tool endpoint logs for the failing call to see the specific error
* Ensure endpoints handle edge cases and return appropriate error responses
* Verify that tool response formats match the expected schema
* Add timeout handling and retry logic where appropriate; see [Function calling](/build/add-function-calling) for endpoint setup details

<Info>
  This metric measures whether your tool endpoints executed successfully. For issues with the agent choosing the wrong tools to call, see [High Tool Call Inaccuracy](#high-tool-call-inaccuracy) above.
</Info>

### Transfer Call Issues

When transfer calls fail.

**How to fix:**

* Check the error log for the specific error — it usually indicates the cause
* **Telephony issues** (e.g. connection or configuration): Change the relevant settings or contact your telephony provider to resolve
* **No one picking up**: Review staffing levels during peak times; verify transfer destination numbers and that someone is available to receive the call
* **Human detection not working**: If you use [Warm Transfer](/build/conversation-flow/call-transfer-node) and the system fails to detect when a human has answered, try switching to [Agentic Warm Transfer](/build/conversation-flow/call-transfer-node), which uses a transfer agent to converse with the transfer target before bridging.

## Performance

### High Latency

When end-to-end latency is too high (e.g., p50 exceeds 2.5 seconds).

**How to fix:**

* Use the [latency breakdown](/reliability/troubleshoot-latency) in the call dashboard to find the bottleneck (LLM inference, TTS, network, etc.)
* If LLM inference is the bottleneck, switch to a faster model
* If TTS is the bottleneck, choose a lower-latency voice provider
* If tool calls are slow, optimize tool endpoints or reduce response size

## Custom Evaluation

### Failed Custom Evaluation Criteria

When one or more AI Evaluated Conditions fail.

**How to fix:**

* Use the failure reason in the call QA sheet to identify the gap, then update your agent's system prompt or knowledge base as needed
* If the failure was actually correct behavior (e.g., intentional by design), use [calibration](/ai-qa/terminologies#calibration) to override the evaluation for that call

<Info>
  Not all failures are actionable — some may be caused by external factors (e.g., the user hanging up early) or intentional design decisions (e.g., transferring when a user requests a human). Focus on failures where the agent's behavior or configuration can be improved.
</Info>

**Calibration best practices:**

* Use calibration to correct edge cases where the automatic evaluation doesn't match your judgment
* If you find yourself calibrating many calls the same way, update your resolution criteria or metric thresholds instead — this is more efficient and applies to all future evaluations
* Add notes when calibrating to document why the override was needed, which helps your team maintain consistency

## Interpreting Your Results

<Tip>
  When interpreting metrics, consider them in context:

  * Compare metrics across similar cohorts or time periods
  * Look for trends rather than focusing on individual data points
  * Use multiple metrics together to get a complete picture of call quality
</Tip>
