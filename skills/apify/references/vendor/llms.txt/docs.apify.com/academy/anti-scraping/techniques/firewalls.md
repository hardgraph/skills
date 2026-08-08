---
title: Firewalls
url: https://docs.apify.com/academy/anti-scraping/techniques/firewalls.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Anti-scraping protections](https://docs.apify.com/academy/anti-scraping.md)
  - [Anti-scraping techniques](https://docs.apify.com/academy/anti-scraping/techniques.md)
previous: [Geolocation](https://docs.apify.com/academy/anti-scraping/techniques/geolocation.md)
next: [Browser challenges](https://docs.apify.com/academy/anti-scraping/techniques/browser-challenges.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Firewalls

**Understand what a web-application firewall is, how they work, and the various common techniques for avoiding them altogether.**

***

A web-application firewall (or **WAF**) is a tool for website admins which allows them to set various access rules for their visitors. The rules can vary on each website and are usually hard to detect; therefore, on sites using a WAF, you need to run a set of tests to test the rules and find out their limits.

One of the most common WAFs one can come across is the one from [Cloudflare](https://www.cloudflare.com). It allows setting a waiting screen that runs a few tests against the visitor to detect a genuine visitor or a bot. However, not all WAFs are that easy to detect.

![Cloudflare waiting screen](/assets/images/cloudflare-bd22fffac9bd5e98e327247500da14cb.png)

## How it works

WAFs work on a similar premise as regular firewalls. Web admins define the rules, and the firewall executes them. As an example of how the WAF can work, we will take a look at Cloudflare's solution:

1. The visitor sends a request to the webpage.
2. The request is intercepted by the firewall.
3. The firewall decides if presenting a challenge (captcha) is necessary. If the user already solved a captcha in the past or nothing is suspicious, it will immediately forward the request to the application's server.
4. A captcha is presented which must be solved. Once it is solved, a cookie is stored in the visitor's browser.
5. The request is forwarded to the application's server.

![Cloudflare WAP workflow](/assets/images/cloudflare-graphic-8f4223bc691752af247662e7778589ff.jpg)

Since there are multiple providers, it is essential to say that the challenges are not always graphical and can be entirely server-side (without any JavaScript evaluation in the visitor browser).

## Bypassing web-application firewalls

* Using [proxies](https://docs.apify.com/academy/anti-scraping/mitigation/proxies.md).
* Mocking headers.
* Overriding the browser's [fingerprint](https://docs.apify.com/academy/anti-scraping/techniques/fingerprinting.md) (most effective).
* Farming the cookies from a website with a headless browser, then using the farmed cookies to do HTTP based scraping (most performant).

As you likely already know, there is no solution that fits all. If you are struggling to get past a WAF provider, you can try using Firefox with Playwright.

## Next up

In the [next lesson](https://docs.apify.com/academy/anti-scraping/techniques/browser-challenges.md), we'll be covering **browser challenges** and specifically the Cloudflare browser challenge which is part of the Cloudflare WAF mentioned in this lesson.
