---
title: "II - Opening & controlling a page"
url: https://docs.apify.com/academy/puppeteer-playwright/page.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
children:
  - [Interacting with a page](https://docs.apify.com/academy/puppeteer-playwright/page/interacting-with-a-page.md)
  - [Waiting for elements and events](https://docs.apify.com/academy/puppeteer-playwright/page/waiting.md)
  - [Page methods](https://docs.apify.com/academy/puppeteer-playwright/page/page-methods.md)
previous: [I - Launching a browser](https://docs.apify.com/academy/puppeteer-playwright/browser.md)
next: [Interacting with a page](https://docs.apify.com/academy/puppeteer-playwright/page/interacting-with-a-page.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# II - Opening & controlling a page

**Learn how to create and open a Page with a Browser, and how to use it to visit and programmatically interact with a website.**

***

When you open up your regular browser and visit a website, you open up a new page (or tab) before entering the URL in the search bar and hitting the **Enter** key. In Playwright and Puppeteer, you also have to open up a new page before visiting a URL. This can be done with the `browser.newPage()` function, which will return a **Page** object ([Puppeteer](https://pptr.dev/#?product=Puppeteer&version=v13.7.0&show=api-class-page), [Playwright](https://playwright.dev/docs/api/class-page)).

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });



// Open a new page

const page = await browser.newPage();



await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });



// Open a new page

const page = await browser.newPage();



await browser.close();
```


Then, we can visit a website with the `page.goto()` method. Let's go to [Google](https://google.com) for now. We'll also use the `page.waitForTimeout()` function, which will force the program to wait for a number of seconds before quitting (otherwise, everything will flash before our eyes and we won't really be able to tell what's going on):

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });



// Open a new page

const page = await browser.newPage();



// Visit Google

await page.goto('https://google.com');



// wait for 10 seconds before shutting down

await page.waitForTimeout(10000);



await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });



// Open a new page

const page = await browser.newPage();



// Visit Google

await page.goto('https://google.com');



// wait for 10 seconds before shutting down

await page.waitForTimeout(10000);



await browser.close();
```


> If you haven't already, go ahead and run this code to see what happens.

## Next up

Now that we know how to open up a page, [let's learn](https://docs.apify.com/academy/puppeteer-playwright/page/interacting-with-a-page.md) how to automate page interaction, such as clicking, typing, and pressing keys.
