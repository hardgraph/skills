> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Configure LLM Options

> Tune Retell LLM settings — temperature, fast tier, structured output, retries, timeouts — to balance reliability, latency, and cost for your voice agent.

## Overview

LLM (Large Language Model) configuration options allow you to fine-tune how your agent processes and responds to conversations. Different settings can dramatically impact your agent's behavior, reliability, and cost.

<Note>
  Not all options are available for every model. Check your dashboard for model-specific capabilities.
</Note>

<Frame>
  <img width="300" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/llm-options.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=0c099397152efe520f6069348ca7b3e0" alt="LLM configuration panel showing temperature, structured output, and fast tier options" data-path="images/llm-options.png" />
</Frame>

## Temperature Settings

### What is Temperature?

Temperature controls the randomness and creativity of your agent's responses. It's a value typically between 0 and 1 that affects how the model selects its next words.

### Temperature Guidelines

| Temperature   | Behavior                           | Best For                                             |
| ------------- | ---------------------------------- | ---------------------------------------------------- |
| **0.0 - 0.3** | Highly consistent, deterministic   | Function calling, data collection, technical support |
| **0.4 - 0.7** | Balanced consistency and variation | General customer service, sales calls                |
| **0.8 - 1.0** | Creative, varied responses         | Creative brainstorming, casual conversation          |

### Recommendations by Use Case

* **Appointment Booking**: Use 0.1-0.3 for accurate data capture
* **Customer Support**: Use 0.3-0.5 for consistent yet natural responses
* **Sales Outreach**: Use 0.5-0.7 for engaging but focused conversations
* **Virtual Companion**: Use 0.7-0.9 for more human-like variation

## Structured Output

### Purpose

Structured Output ensures that LLM responses strictly follow predefined schemas, particularly important for reliable function calling. When enabled, the model is constrained to output only valid function calls with all required parameters.

### Benefits

* **Increased Reliability**: Eliminates missing or malformed function arguments
* **Better Error Handling**: Prevents invalid function calls from being attempted
* **Consistent Data Format**: Ensures all outputs match expected schemas

### Trade-offs

* **Slower Auto-save**: Schema caching may delay agent configuration saves
* **Less Flexibility**: Model cannot deviate from defined structures
* **Initial Setup Time**: First load after changes may be slower

### When to Use

✅ **Enable for:**

* Production agents with critical function calls
* Agents handling financial or medical data
* Integration with strict API requirements

❌ **Consider disabling for:**

* Development and testing phases
* Agents with simple or flexible function needs
* When rapid iteration is more important than reliability

## Fast Tier (Premium Performance)

### What is Fast Tier?

Fast Tier routes your LLM calls through dedicated, high-priority infrastructure for superior performance and consistency. This premium option eliminates the variability you might experience with standard routing.

### Key Benefits

1. **Consistent Latency**: Predictable response times for every call
2. **Higher Availability**: Priority access to compute resources
3. **Reduced Variance**: Minimal fluctuation in processing speeds
4. **Better User Experience**: Smoother, more natural conversations

### Cost Consideration

<Warning>
  Fast Tier pricing is **1.5x the standard rate** for your selected model. Calculate the ROI based on your use case before enabling.
</Warning>

### When to Use Fast Tier

✅ **Ideal for:**

* High-value customer interactions
* Time-sensitive operations (emergency services, urgent support)
* Premium service tiers
* Demonstrations and sales calls

❌ **May not be necessary for:**

* Internal testing
* Low-volume or non-critical calls
* Cost-sensitive applications

<Frame>
  <img height="400" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/fast_tier.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=62b74c4d61be835df7a4ed82e4a33e60" alt="Performance comparison chart showing improved latency consistency with Fast Tier enabled" data-path="images/fast_tier.png" />
</Frame>

### Performance Impact

Based on our benchmarks:

* **50% reduction** in latency variance
* **25% improvement** in average response time
* **99.9% availability** vs 99.5% standard
