---
title: "Reliability"
---

*All Systems Operational*

# Paywalls load. Purchases go through. Every time.

You can’t afford downtime. Get 99.99% uptime with a five-layer defense that keeps revenue flowing.


## A five-layer defense, running 24/7

It's 2am. A cloud provider has a service disruption. Your users are opening your app, viewing your paywall, and trying to subscribe. Here's what happens.

1. **The SDK**
   The user opens the app, but the App Store is unreachable. Instead of an error or blank screen, RevenueCat loads a cached paywall from the SDK.
   - **Local caching of paywall specs and product data**
   - **Built-in branded fallback paywall with purchase CTA**
   - **Durable queue + temporary entitlements**
   - **Telemetry for every fallback path**

2. **The Edge & CDN**
   The request leaves the device. RevenueCat uses multiple CDNs and global points of presence to route traffic to reachable API servers, even when a region, route, or provider is degraded.
   - **Global CDN with worldwide points-of-presence**
   - **Low-latency API routing**
   - **Dedicated, monitored network paths**
   - **Alternate CDN coverage for constrained networks**

3. **Static fallback CDNs**
   If the SDK has no local cache, it still needs enough data to show products. Static fallback CDNs can serve offerings, product data, and paywall assets from a separate fallback path.
   - **Static offerings and product data**
   - **Separate infrastructure path**
   - **Built for simplicity**
   - **Recovery for fresh installs and empty caches**

4. **Fortress**
   RevenueCat’s primary backend is unavailable. Fortress accepts the purchase event, grants a temporary entitlement, and stores the transaction for reconciliation. The user gets access, and the purchase is preserved.
   - **Backup backend separate from primary services**
   - **Accepts purchases during primary backend failure**
   - **Grants temporary entitlements**
   - **Sends fallback entitlement webhooks**

5. **The Core Backend**
   The outage ends. RevenueCat validates receipts with the app stores, reconciles temporary grants from the SDK and Fortress, deduplicates replayed events, and restores the source of truth.
   - **Multi-region, autoscaled APIs as the source of truth**
   - **Receipt validation against app stores**
   - **Reconciliation of temporary grants and replayed events**
   - **Deduplication of replayed purchase events**


## Reliability is never done

Every incident, edge case, and new feature teaches us something. RevenueCat keeps investing in the people, systems, and fallback paths that protect purchases before, during, and after an outage.

### Reliability from product inception
New features are designed with failure modes, fallback behavior, and recovery paths in mind before they ship.

### Dedicated engineering and SRE expertise
A highly skilled team owns the systems, playbooks, monitoring, and escalation paths that keep critical purchase flows available.

### Continuous hardening after every incident
Incidents, alerts, and edge cases turn into follow-up work, so the platform gets more resilient over time.

### Reliability across the full purchase path
We invest across the SDK, CDNs, backend services, app store integrations, and reconciliation systems.


## When the system needs a human, one is ready

Fortress handles failures automatically. But when an incident needs human judgment, our SRE team is already on it, with a defined playbook.

### 24/7 on-call SRE coverage
A dedicated on-call engineer is always available. Every alert is triaged immediately.

### Defined SEV triggers
Incidents are classified by severity the moment they're detected. SEV1 triggers immediate escalation with a dedicated incident commander and customer communication within minutes.

### Post-incident improvements
Every incident ends with a post-mortem. Findings are tracked and shipped as hardening improvements.

### Synthetic monitoring
Automated checks run continuously across multiple regions, simulating real user flows. Issues are detected before your users notice them.

### Encryption everywhere
All data in transit and at rest is protected using industry-standard security protocols.


## Driving results for the world’s most downloaded apps

> RevenueCat is at the center of our stack for subscriptions. It enables us to have one single source of truth for subscriptions and revenue data and then allows us to spread that reliable data across all of the great integrations RevenueCat has with the rest of our marketing and analytics stack.
> — Olivier Lemarié, Head of Growth and Marketing, Photoroom

**2-3x** — increase in trial sign-up rates in Japan
**50%** — increase in upsell screen conversions
[Read case study](/customers/photoroom)

> We needed a partner who could match our speed and scale — RevenueCat delivered on both.
> — Sara Conlon, Head of Financial Engineering, ChatGPT
[Read the case](/customers/revenuecat-openai)

> In just a year from shipping with RevenueCat, I was able to quit my day job to focus 100% on CardPointers. I’ve continued to grow and expand, all thanks to RevenueCat.
> — Emmanuel Crouvisier, Founder at CardPointers, CardPointers

**~27%** — saved in app store fees with Stripe integration
[Read case study](/customers/cardpointers)


## Frequently asked questions

### What happens if my paywall fails to load?

The RevenueCat SDK caches your paywall specs and product data locally at initialization. If the configured paywall can't render, because our servers are unreachable or the paywall config is missing, the SDK automatically shows a built-in, branded fallback paywall. Your users always see a purchase CTA. They never see a blank screen.

### Are purchases lost during an outage?

No. When the primary backend is unavailable, Fortress accepts and persists the transaction, and your users keep their entitlements unlocked. Once the core backend recovers, every transaction is reconciled with the app stores, no purchase is lost and no entitlement is granted incorrectly.

### How do you prevent fraud with offline entitlements?

Offline access is signed and time-bounded. The SDK validates locally cached entitlements against a tamper-resistant signature, and the core backend independently re-verifies every receipt with the app stores as soon as connectivity is restored. Mismatched, replayed, or revoked purchases are dropped during reconciliation.

### How do I test that my app handles outages correctly?

RevenueCat provides a sandbox environment and SDK helpers that simulate network failures, paywall fetch errors, and backend outages. You can verify the fallback paywall, cached entitlements, and reconciliation flow end-to-end before shipping without touching production.

### What's your SLA and where can I see uptime history?

RevenueCat targets 99.99% uptime, backed by a five-layer defense (SDK → Edge → Static CDN → Fortress → Core Backend). Real-time status and historical uptime are published on our public status page, with detailed post-incident reports for every SEV1 event.


## Ready to grow?

Our entire suite of features comes standard and it's free to get started.

[Start for free](https://app.revenuecat.com/signup)
[Talk to sales](/talk-to-sales)
