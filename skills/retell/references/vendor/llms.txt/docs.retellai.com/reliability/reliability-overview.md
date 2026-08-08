> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Reliability Overview

> How Retell AI maintains 99.9% uptime with enterprise infrastructure, monitoring, fallback mechanisms, and platform resilience features.

At Retell, we've made reliability our **top priority**. Our platform is built on enterprise-grade infrastructure to ensure consistent, high-quality performance for all your voice AI needs.

We focus on three key areas to maintain exceptional service quality:

1. **Phone Call Performance**
   * Reliable handling of inbound and outbound calls
   * Consistent connection throughout conversations
   * High voice quality

2. **Agent Reliability**
   * Consistent low latency during interactions

3. **Agent Performance**
   * Accurate speech transcription
   * Strict adherence to prompt instructions

### Our Commitment to Reliability

At Retell, we guarantee **>99.9% uptime**. To achieve this, we've invested in the following areas:

1. **Enterprise-grade infrastructure**
2. **Fallbacks and Resilience Features**
3. **24/7 Monitoring and Alerting**
4. **24/7 Support**

### Detailed Overview

1. **Enterprise-grade infrastructure**: We conduct extensive load testing on high traffic and maintain dedicated auto-scaling and provisioning to handle varying loads. Our enterprise-grade compute, networking, and infrastructure ensure stable performance.
   * We guarantee **>99.9% uptime** - Subscribe to our [Status Page](https://status.retellai.com/) to get notified about any issues.
   <Frame>
     <img height="500" src="https://mintcdn.com/retellai/a1LftRqc_k-5TDA7/images/status-page.png?fit=max&auto=format&n=a1LftRqc_k-5TDA7&q=85&s=e95d954306e7ecb1a23656c12a9791c4" data-path="images/status-page.png" />
   </Frame>
   * Self-hosted models to reduce third-party dependencies.
   * **Stable server cluster** (enterprise only): Opt in to route both calls and API requests to our stable server cluster, which receives delayed feature rollouts for added production stability. When enabled, point your API requests to `https://stable.retellai.com/` instead of `https://api.retellai.com/`. A \$0.02/min surcharge applies on calls. Contact [support](/general/support) to enable.

2. **Proactive Monitoring**: We maintain 24/7 latency monitoring and alerting systems to catch and address issues before they impact your operations.
   * Including ASR, TTS, LLM, Knowledge base, time to first token, and network latency distribution, p75, p90, p95, p99 latencies.
   * Failed calls count, ASR, LLM, TTS timeout, and error rate.
   * Server CPU, GPU, memory, and network usage. Database, API response time.

3. **Resilience Features**: We've implemented fallbacks, retries, and other features to improve reliability:
   * [Branded Call/Verified Phone Number Features](/build/telephony/call_efficiency_overview) to improve call pickup rate and allowlist carrier calls
   * [TTS fallback and retries](/build/tts-fallback) are automatically built in, and can be manually configured.
   * LLM fallback and retries are automatically built in.

4. [Testing Features](/test/test-overview)

5. **Support System**: Our dedicated support team is ready to assist if any issues arise.
   * 7 days/week on-call schedule
   * 24h SLA, with active support between 9 AM and 9 PM PST (lower for enterprise customers)
     Read more about support [here](/general/support).
