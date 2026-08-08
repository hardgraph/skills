---
title: Anti-scraping techniques
url: https://docs.apify.com/academy/anti-scraping/techniques.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Anti-scraping protections](https://docs.apify.com/academy/anti-scraping.md)
children:
  - [Rate-limiting](https://docs.apify.com/academy/anti-scraping/techniques/rate-limiting.md)
  - [Fingerprinting](https://docs.apify.com/academy/anti-scraping/techniques/fingerprinting.md)
  - [Geolocation](https://docs.apify.com/academy/anti-scraping/techniques/geolocation.md)
  - [Firewalls](https://docs.apify.com/academy/anti-scraping/techniques/firewalls.md)
  - [Browser challenges](https://docs.apify.com/academy/anti-scraping/techniques/browser-challenges.md)
  - [Captchas](https://docs.apify.com/academy/anti-scraping/techniques/captchas.md)
previous: [Anti-scraping protections](https://docs.apify.com/academy/anti-scraping.md)
next: [Rate-limiting](https://docs.apify.com/academy/anti-scraping/techniques/rate-limiting.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Anti-scraping techniques

**Understand the various common (and obscure) anti-scraping techniques used by websites to prevent bots from accessing their content.**

***

In this section, we'll be discussing some of the most common (as well as some obscure) anti-scraping techniques used by websites to detect and block/limit bots from accessing their content.

When a scraper is detected, a website can respond in a variety of ways:

## "Access denied" page

This is a complete block which usually has a response status code of **403**. Usually, you'll hit an **Access denied** page if you have bad IP address or the website is restricted in the country of the IP address.

> For a better understanding of what all the HTTP status codes mean, we recommend checking out [HTTP Cat](https://http.cat/) which provides a highly professional description for each status code.

## Captcha page

Probably the most common blocking method. The website gives you a chance to prove that you are not a bot by presenting you with a captcha. We'll be covering CAPTCHAs within this course.

## Redirect

Another common method is redirecting to the home page of the site (or a different location).

## Request timeout/Socket hangup

This is the cheapest defense mechanism where the website won't even respond to the request. Dealing with timeouts in a scraper can be challenging, because you have to differentiate them from regular network problems.

## Custom status code or message

Similar to getting an **Access denied** page, but some sites send along specific status codes (eg. **503**) and messages explaining what was wrong with the request.

## Empty results

The website responds "normally," but pretends to not find any results. This requires manual testing to recognize the pattern.

## Fake results

The website responds with data, but the data is totally fake, which is very difficult to recognize and requires extensive manual testing. Luckily, this type of response is not all too common.

## Next up

In the [first lesson](https://docs.apify.com/academy/anti-scraping/techniques/rate-limiting.md) of this course, you'll be learning about **rate limiting**, which is a technique used to prevent a large amount of requests from being sent from one user.
