---
title: I - Launching a browser
url: https://docs.apify.com/academy/puppeteer-playwright/browser.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
previous: [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
next: [II - Opening & controlling a page](https://docs.apify.com/academy/puppeteer-playwright/page.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# I - Launching a browser

**Understand what the Browser object is in Puppeteer/Playwright, how to create one, and a bit about how to interact with one.**

***

In order to automate a browser in Playwright or Puppeteer, we need to open one up programmatically. Playwright supports Chromium, Firefox, and Webkit (Safari), while Puppeteer only supports Chromium based browsers. For ease of understanding, we've chosen to use Chromium in the Playwright examples to keep things working on the same plane.

Let's start by using the `launch()` function in the **index.js** file we created in the intro to this course:

**Playwright**


```js
import { chromium } from 'playwright';



await chromium.launch();



console.log('launched!');
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



await puppeteer.launch();



console.log('launched!');
```


When we run this code with the command `node index.js`, a browser will open up; however, we won't actually see anything. This is because the default mode of a browser after `launch()`ing it is **headless**, meaning that it has no visible UI.

> If you run this code right now, it will hang. Use **control^** + **C** to force quit the program.

## Launch options

In order to see what's actually happening, we can pass an **options** object ([Puppeteer](https://pptr.dev/#?product=Puppeteer&version=v13.7.0&show=api-puppeteerlaunchoptions), [Playwright](https://playwright.dev/docs/api/class-browsertype#browser-type-launch)) with **headless** set to **false**.

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });

await browser.newPage();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });

await browser.newPage();
```


Now we'll actually see a browser open up.

![Chromium browser opened by Puppeteer/Playwright](/assets/images/chromium-844298b27f771e8c1bb0441bf5572180.jpg)

You can pass a whole lot more options to the `launch()` function. We'll be getting into those a little bit later on.

## Browser methods

The `launch()` function also returns a **Browser** object ([Puppeteer](https://pptr.dev/#?product=Puppeteer&version=v13.7.0&show=api-class-browser), [Playwright](https://playwright.dev/docs/api/class-browser)), which is a representation of the browser. This object has many methods, which allow us to interact with the browser from our code. One of them is `close()`. Until now, we've been using **control^** + **C** to force quit the process, but with this function, we'll no longer have to do that.

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });

await browser.newPage();



// code will be here in the future



await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });

await browser.newPage();



// code will be here in the future



await browser.close();
```


## Next up

Now that we can open a browser, let's move onto the [next lesson](https://docs.apify.com/academy/puppeteer-playwright/page.md) where we will learn how to create pages and visit websites programmatically.
