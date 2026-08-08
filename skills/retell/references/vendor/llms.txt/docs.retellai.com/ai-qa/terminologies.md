> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# AI QA metrics

> Reference for every AI QA metric and term — average latency, hallucination rate, KB accuracy, overlapping speech, sentiment — and how each one is evaluated.

## Overview

This page explains every metric and term used in AI QA so you can interpret your call analysis results. For each metric, you’ll see what it measures and how it’s evaluated. When a metric fails your criteria, use [Address metric issues](/ai-qa/address-metric-issues) to find step-by-step guidance on how to fix it.

## Performance Metrics

### Latency

**Average Latency**: Measures the end-to-end delay between a user speaking and the Voice AI beginning its spoken response. Lower latency indicates more responsive interactions.

**Latency P50**: The 50th percentile (median) of latency measurements. This metric shows the typical response time, with half of all responses being faster and half being slower.

<Info>
  Latency is measured in seconds (s). Lower values indicate better performance.
</Info>

### Sentiment Analysis

**User Sentiment**: Represents the emotional state of the caller as inferred from speech content, tone, and pitch. Sentiment can be positive, negative, or neutral.

* **User Positive Sentiment Rate**: Percentage of user interactions with positive sentiment
* **User Negative Sentiment Rate**: Percentage of user interactions with negative sentiment
* **Negative Sentiment Rate**: Overall rate of negative sentiment detected in the conversation

**Agent Sentiment**: Represents the emotional tone expressed by the Voice AI during speech output. This metric helps ensure your agent maintains an appropriate tone throughout conversations.

* **Agent Positive Sentiment Rate**: Percentage of agent responses with positive sentiment
* **Agent Natural Tonality Rate**: Measures how natural and human-like the agent's tone sounds

### Transcription Metrics

**WER (Word Error Rate)**: Measures the accuracy of speech-to-text transcription by calculating the percentage of words that were incorrectly transcribed. Lower WER indicates better transcription accuracy.

<Note>
  WER is calculated as: (Substitutions + Insertions + Deletions) / Total Words in Reference × 100%
</Note>

<Accordion title="How is the WER reference transcript generated?">
  The reference is produced by re-transcribing the user's audio using a process that listens to the audio and uses the original STT transcript as reference. This usually yields a much more accurate transcript than the initial STT output, though the reference can still contain errors.

  WER measures how much the original STT transcript diverges from this reference.
</Accordion>

**Mistranscribed Entities**: Count of specific entities (names, dates, numbers, etc.) that were incorrectly transcribed during the call. Only critical factual errors that change meaning are counted.

### Call Quality Metrics

**Overlapping Speech**: Count of times the user and agent spoke at the same time during the conversation. Higher counts may indicate the agent is speaking too long or not responding appropriately.

**Avg. Overlapping Speech**: Average number of overlapping speech instances per call across the cohort.

**Agent Naturalness**: Measures how human-like the agent sounded, including pronunciation, intonation, pacing, turn-taking behavior, and the absence of robotic patterns. Higher values indicate more natural-sounding speech.

<Accordion title="How is Agent Naturalness evaluated?">
  Agent Naturalness is evaluated using **both the audio and the transcript**. The evaluation looks for issues that would affect how understandable or natural the agent sounds, such as:

  * Robotic glitches, distortion, or unexpected volume changes
  * Mispronunciation or word substitution that differs from what was intended
  * Slurring, mumbling, or dropped sounds
  * Speech speed or intonation that sounds unnatural or alters meaning

  Only clear issues that would confuse a listener are flagged; minor robotic tone or technical phrasing is not counted against the metric.

  **Natural Tonality Rate** = (utterances without naturalness issues / total agent utterances) × 100%
</Accordion>

**Natural Tonality Rate**: Percentage of agent speech that sounds natural and human-like in tone and delivery.

### AI Accuracy Metrics

**LLM Hallucination Rate**: Measures how often the Large Language Model (LLM) generated incorrect or fabricated information that wasn't supported by the conversation context or knowledge base.

**Agent Hallucination**: Measures how often the agent hallucinated during conversations. This is a critical metric for ensuring factual accuracy.

<Accordion title="How is Hallucination Rate calculated?">
  Hallucination is evaluated **per agent turn** — each response is checked against the agent's instructions, knowledge base, and conversation history. The rate is the percentage of agent turns that contain a hallucination.

  **Types of hallucination:**

  * **Fabrication**: Invents factual claims not supported by the context (e.g., making up a case number)
  * **Contradiction**: States something that conflicts with what was provided or said earlier
  * **Confusion**: Misunderstands instructions and gives factually irrelevant or logically wrong information

  Severity can be **major** (critical false information that could cause a failed resolution) or **minor** (non-critical discrepancies). The evaluation focuses on factual accuracy only, not style or tone.
</Accordion>

<Warning>
  High hallucination rates indicate the agent may be providing incorrect information to users, which can damage trust and lead to poor outcomes.
</Warning>

### Knowledge Base Metrics

**KB (Knowledge Base) Recall**: Measures how effectively the agent retrieved and used relevant information from the knowledge base. Higher recall indicates better knowledge base utilization.

<Accordion title="How is KB Recall calculated?">
  KB Recall is evaluated per retrieval turn. For each turn where the knowledge base is queried, the system determines which retrieved chunks were truly relevant to the user's question.

  * A **"Hit"** (full recall) means all relevant chunks were retrieved for that turn
  * A **"Miss"** means one or more relevant chunks were missed

  **KB Recall Rate** = (turns with full recall / total retrieval turns) × 100%

  Relevance is judged strictly: a chunk counts as relevant only if it provides specific, actionable information or instructions that apply to what the user was asking, not general or marketing-style content.
</Accordion>

### Tool and Function Metrics

**Tool Call Accuracy**: Measures the rate at which the agent correctly invoked tools or functions. Higher accuracy means the agent is using the right tools at the right time.

**Tool Call Inaccuracy**: Measures the rate at which the agent invoked incorrect tools. This is the inverse of Tool Call Accuracy.

**Custom Tool Success Rate**: Percentage of custom tool calls that completed successfully.

**Avg Custom Tool Latency**: Average time taken for custom tools to execute and return results.

### Conversation Flow Metrics

**Transition Accuracy**: Measures the accuracy of transitions between conversation nodes or states. Higher accuracy indicates the agent is following the intended conversation flow correctly.

**Node Transition Inaccuracy**: Measures incorrect node transitions in conversation flows. This metric helps identify when the agent moves to the wrong conversation state.

### Call Resolution Metrics

**Call Resolution Rate**: Percentage of calls that were successfully resolved according to your defined resolution criteria.

**Average Score**: Overall quality score for calls in the cohort, calculated based on your resolution criteria and weighted scoring configuration.

**Calls Analyzed**: Total number of calls that have been analyzed in the cohort.

### Transfer Metrics

**Transfer Success Rate**: Percentage of calls that were successfully transferred to another agent or system.

**Transfer Wait Time**: Average time users wait before a transfer is completed.

## Call-Level Data

### Call Identification

**Call ID**: Unique identifier for each individual call in the system.

**Call Start Time**: Timestamp indicating when a call began.

**Call Length**: Total duration of the call, typically measured in seconds or minutes.

### Evaluation Status

**Eval**: Evaluation status or score for individual calls, indicating whether the call met the defined resolution criteria.

## Statistical Terms

### Percentiles

**P50 (50th Percentile)**: The median value, where half of all measurements are above and half are below. Also known as the median.

<Info>
  Percentiles help understand the distribution of metrics. P50 shows typical performance, while P95 or P99 show worst-case scenarios.
</Info>

## Cohort Terms

### Cohort

A **Cohort** is a filtered set of calls that share common characteristics (agents, date range, call duration, etc.) and are analyzed together using the same resolution criteria.

### Sampling

**Sampling Percentage**: The percentage of calls matching your filters that will be included in the cohort for analysis.

**Weekly Max**: Maximum number of calls that can be analyzed per week, regardless of the sampling percentage.

## Resolution Criteria Terms

### AI Evaluated Condition

Custom criteria evaluated by AI based on call transcripts and context. These are qualitative assessments (e.g., "Call resolved", "Customer satisfied") rather than quantitative metrics.

### Performance Metric

Quantitative thresholds that calls must meet, such as latency below 2 seconds or sentiment above 80%.

### Weighted Scoring

A scoring system that assigns different weights to various resolution criteria, allowing you to prioritize certain conditions or metrics over others.

### Calibration

**Calibrate to Success / Calibrate to Failure**: Manual overrides that let you adjust automatic metric evaluations for a specific call.

<Accordion title="How does calibration work?">
  * **Calibrate to Success**: Mark a failed metric as passed — the failure no longer counts against the call's score
  * **Calibrate to Failure**: Mark a passed metric as failed — the pass no longer contributes to the call's score

  **Important:** Calibration updates the **per-call score only**. It does **not** change the underlying metric criteria or scoring logic for future calls. Each calibration applies to the specific call you are reviewing.
</Accordion>

<Warning>
  Some metrics may show "N/A" when there isn't sufficient data or when the metric doesn't apply to a particular call type (e.g., transfer metrics for non-transfer calls).
</Warning>
