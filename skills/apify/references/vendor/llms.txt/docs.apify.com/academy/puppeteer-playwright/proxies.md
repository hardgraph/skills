---
title: V - Using proxies
url: https://docs.apify.com/academy/puppeteer-playwright/proxies.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
previous: [IV - Reading & intercepting requests](https://docs.apify.com/academy/puppeteer-playwright/reading-intercepting-requests.md)
next: [VI - Creating multiple browser contexts](https://docs.apify.com/academy/puppeteer-playwright/browser-contexts.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# V - Using proxies

**Understand how to use proxies in your Puppeteer and Playwright requests, as well as a couple of the most common use cases for proxies.**

***

[Proxies](https://docs.apify.com/academy/anti-scraping/mitigation/proxies.md) are a great way of appearing as if you are making requests from a different location. A common use case for proxies is to avoid [geolocation](https://docs.apify.com/academy/anti-scraping/techniques/geolocation.md) restrictions. For example your favorite TV show might not be available on Netflix in your country, but it might be available for Vietnamese Netflix watchers.

In this lesson, we'll be learning how to use proxies with Playwright and Puppeteer. This will be demonstrated with a Vietnamese proxy that we got by running [this](https://apify.com/mstephen190/proxy-scraper) proxy-scraping Actor on the Apify platform.

## Adding a proxy

First, let's add our familiar boilerplate code for visiting Google and also create a variable called `proxy` which will point to our proxy server:

> Note that this proxy may no longer be working at the time of reading. If you don't have a proxy to use during this lesson, we recommend using Proxy Scraper for a list of free ones, or checking out [Apify proxy](https://apify.com/proxy)

**Playwright**


```js
import { chromium } from 'playwright';



// our proxy server

const proxy = '103.214.9.13:3128';



const browser = await chromium.launch({ headless: false });

const page = await browser.newPage();

await page.goto('https://google.com');



await page.waitForTimeout(10000);

await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



// our proxy server

const proxy = '103.214.9.13:3128';



const browser = await puppeteer.launch({ headless: false });

const page = await browser.newPage();

await page.goto('https://google.com');



await page.waitForTimeout(10000);

await browser.close();
```


For both Puppeteer and Playwright, the proxy server's URL should be passed into the options of the `launch()` function; however, it's done a bit differently depending on which library you're using.

In Puppeteer, the server must be passed within the **--proxy-server** [Chromium command line argument](https://peter.sh/experiments/chromium-command-line-switches/), while in Playwright, it can be passed into the **proxy** option.

**Playwright**


```js
import { chromium } from 'playwright';



const proxy = '103.214.9.13:3128';



const browser = await chromium.launch({

    headless: false,

    // Using the "proxy" option

    proxy: {

        // Pass in the server URL

        server: proxy,



    },

});

const page = await browser.newPage();

await page.goto('https://google.com');



await page.waitForTimeout(10000);

await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const proxy = '103.214.9.13:3128';



// Using the "args" option, which is an array of Chromium command

// line switches, we pass the server URL in with "--proxy-server"

const browser = await puppeteer.launch({

    headless: false,

    args: [`--proxy-server=${proxy}`],

});

const page = await browser.newPage();

await page.goto('https://google.com');



await page.waitForTimeout(10000);

await browser.close();
```


And that's it! Now, when we visit Google, it's in Vietnamese. Depending on the country of your proxy, the language will vary.

![Vietnamese Google](/assets/images/vietnamese-google-a742c6f89651d9c47a6d3701140a11cd.png)

> Note that in order to rotate through multiple proxies, you must retire a browser instance then create a new one to continue automating with a new proxy.

## Authenticating a proxy

The proxy in the last activity didn't require a username and password, but let's say that this one does:


```text
proxy.example.com:3001
```


One might automatically assume that this would be the solution:

**Playwright**


```js
// This code is wrong!

import { chromium } from 'playwright';



const proxy = 'proxy.example.com:3001';

const username = 'someUsername';

const password = 'password123';



const browser = await chromium.launch({

    headless: false,

    proxy: {

        server: `http://${username}:${password}@${proxy}`,



    },

});
```


**Puppeteer**


```js
// This code is wrong!

import puppeteer from 'puppeteer';



const proxy = 'proxy.example.com:3001';

const username = 'someUsername';

const password = 'password123';



const browser = await puppeteer.launch({

    headless: false,

    args: [`--proxy-server=http://${username}:${password}@${proxy}`],

});
```


However, authentication parameters need to be passed in separately in order to work. In Puppeteer, the username and password need to be passed to the `page.authenticate()` prior to any navigations being made, while in Playwright they can be passed to the **proxy** option object.

**Playwright**


```js
import { chromium } from 'playwright';



const proxy = 'proxy.example.com:3001';

const username = 'someUsername';

const password = 'password123';



const browser = await chromium.launch({

    headless: false,

    proxy: {

        server: proxy,

        username,

        password,

    },

});

// Proxy will now be authenticated
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const proxy = 'proxy.example.com:3001';

const username = 'someUsername';

const password = 'password123';



const browser = await puppeteer.launch({

    headless: false,

    args: [`--proxy-server=${proxy}`],

});



const page = await browser.newPage();



await page.authenticate({ username, password });

// Proxy will now be authenticated
```


## Next up

You already know how to launch a browser with various configurations, which means you're ready to [learn about browser contexts](https://docs.apify.com/academy/puppeteer-playwright/browser-contexts.md). Browser contexts can be used to automate multiple sessions at once with completely different configurations. You'll also learn how to emulate different devices, such as iPhones, iPads, and Androids.
