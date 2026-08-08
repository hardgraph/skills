---
title: III - Executing scripts
url: https://docs.apify.com/academy/puppeteer-playwright/executing-scripts.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
children:
  - [Injecting code](https://docs.apify.com/academy/puppeteer-playwright/executing-scripts/injecting-code.md)
  - [Extracting data](https://docs.apify.com/academy/puppeteer-playwright/executing-scripts/collecting-data.md)
previous: [Page methods](https://docs.apify.com/academy/puppeteer-playwright/page/page-methods.md)
next: [Injecting code](https://docs.apify.com/academy/puppeteer-playwright/executing-scripts/injecting-code.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# III - Executing scripts

**Understand the two different contexts which your code can be run in, and how to run custom scripts in the context of the browser.**

***

An important concept to understand when dealing with headless browsers is the **context** in which your code is being run. For example, if you try to use the native `fs` Node.js module (used in the previous lesson) while running code in the context of the browser, errors will be thrown saying that it is undefined. Similarly, if you are trying to use `document.querySelector()` or other browser-specific functions in the server-side Node.js context, errors will also be thrown.

![Diagram explaining the two different contexts your code can be run in](/assets/images/context-diagram-f4475f84c3ebf68da73881f283fbc174.jpg)

Here is an example of a common mistake made by beginners to Puppeteer/Playwright:


```js
// This code is incorrect!

import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });

const page = await browser.newPage();



// visit google

await page.goto('https://www.google.com/');



// change background to green

document.body.style.background = 'green';



await page.waitForTimeout(10000);



await browser.close();
```


When we try and run this, we get this error:


```text
ReferenceError: document is not defined
```


The reason this is happening is because we're trying to run browser-side code on the server-side where it is not supported. [document](https://developer.mozilla.org/en-US/docs/Web/API/Document) is a property of the browser [Window](https://developer.mozilla.org/en-US/docs/Web/API/Window) instance that holds the rendered website; therefore, this API is not available in Node.js. How are we supposed to run code within the context of the browser?

## Running code in the context of the browser

We will use `page.evaluate()` to run our code in the browser. This method takes a callback as its first parameter, which will be executed within the browser.

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });

const page = await browser.newPage();



await page.goto('https://www.google.com/');



await page.evaluate(() => {

    document.body.style.background = 'green';

});



await page.waitForTimeout(10000);



await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });

const page = await browser.newPage();



await page.goto('https://www.google.com/');



await page.evaluate(() => {

    document.body.style.background = 'green';

});



await page.waitForTimeout(10000);



await browser.close();
```


Here's what we see in the automated browser when we run this code:

![Google with the background color changed to green](/assets/images/green-google-c009bd62b8a1b2ec669f6e5ccef214fc.png)

## Using variables in `page.evaluate()`

Within our code, we generate a `randomString` in the Node.js context:


```js
const randomString = Math.random().toString(36).slice(2);
```


Now, let's say we want to change the title of the document to be this random string. To have the random string available in the callback of our `page.evaluate()`, we'll pass it in a second parameter. It's best practice to have this second parameter as an object, because in real world situations you often need to pass more than one value.

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });

const page = await browser.newPage();



await page.goto('https://www.google.com/');



const params = { randomString: Math.random().toString(36).slice(2) };



await page.evaluate(({ randomString }) => {

    document.querySelector('title').textContent = randomString;

}, params);



await page.waitForTimeout(10000);



await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });

const page = await browser.newPage();



await page.goto('https://www.google.com/');



const params = { randomString: Math.random().toString(36).slice(2) };



await page.evaluate(({ randomString }) => {

    document.querySelector('title').textContent = randomString;

}, params);



await page.waitForTimeout(10000);



await browser.close();
```


Now, when we run this code, we can see the title change on the page's tab:

![Google with the background color changed to green](data:image/png;base64,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)

## Next up

The [next lesson](https://docs.apify.com/academy/puppeteer-playwright/executing-scripts/injecting-code.md) will be a short one discussing two different ways of executing scripts on a page.
