---
title: Page methods
url: https://docs.apify.com/academy/puppeteer-playwright/page/page-methods.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
  - [II - Opening & controlling a page](https://docs.apify.com/academy/puppeteer-playwright/page.md)
previous: [Waiting for elements and events](https://docs.apify.com/academy/puppeteer-playwright/page/waiting.md)
next: [III - Executing scripts](https://docs.apify.com/academy/puppeteer-playwright/executing-scripts.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Page methods

**Understand that the Page object has many different methods to offer, and learn how to use two of them to capture a page's title and take a screenshot.**

***

Other than having methods for interacting with a page and waiting for events and elements, the **Page** object also supports various methods for doing other things, such as [reloading](https://pptr.dev/api/puppeteer.page.reload), [screenshotting](https://playwright.dev/docs/api/class-page#page-screenshot), [changing headers](https://playwright.dev/docs/api/class-page#page-set-extra-http-headers), and extracting the [page's content](https://pptr.dev/api/puppeteer.page.content).

Last lesson, we left off at a point where we were waiting for the page to navigate so that we can extract the page's title and take a screenshot of it. In this lesson, we'll be learning about the two methods we can use to achieve both of those things.

## Grabbing the title

Two main page functions exist that will return general data:

1. `page.content()` will return the entire HTML content of the page.
2. `page.title()` will return the title of the current page found in the `<title>` tag.

For our case, we'll utilize the `page.title()` function to grab the title and log it to the console:


```js
// Grab the title and set it to a variable

const title = await page.title();



// Log the title to the console

console.log(title);
```


## Screenshotting

The `page.screenshot()` function will return a buffer which can be written to the filesystem as an image:


```js
// Take the screenshot and write it to the filesystem

await page.screenshot({ path: 'screenshot.png' });
```


> The image will by default be **.png**. To change the image to **.jpeg** type, set the (optional) `type` option to **jpeg**.

## Final code

Here's our final code which extracts the page's title, takes a screenshot and saves it to our project's folder as `screenshot.png`:

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });



// Create a page and visit Google

const page = await browser.newPage();

await page.goto('https://google.com');



// Agree to the cookies policy

await page.click('button:has-text("Accept all")');



// Type the query and visit the results page

await page.type('textarea[title]', 'hello world');

await page.keyboard.press('Enter');



// Click on the first result

await page.click('.g a');

await page.waitForLoadState('load');



// Grab the page's title and log it to the console

const title = await page.title();

console.log(title);



// Take a screenshot and write it to the filesystem

await page.screenshot({ path: 'screenshot.png' });



await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });



// Create a page and visit Google

const page = await browser.newPage();

await page.goto('https://google.com');



// Agree to the cookies policy

await page.click('button + button');



// Type the query and visit the results page

await page.type('textarea[title]', 'hello world');

await page.keyboard.press('Enter');



// Wait for the first result to appear on the page,

// then click on it

await page.waitForSelector('.g a');

await Promise.all([page.waitForNavigation(), page.click('.g a')]);



// Grab the page's title and log it to the console

const title = await page.title();

console.log(title);



// Take a screenshot and write it to the filesystem

await page.screenshot({ path: 'screenshot.png' });



await browser.close();
```


When you run this code, you should see this logged to the console:


```text
"Hello, World!" program - Wikipedia
```


Additionally, you should see a new image named **screenshot.png** in your project's folder that looks something like this:

![Screenshot from Playwright/Puppeteer](/assets/images/wikipedia-screenshot-e418e43eabee246c354755dd29f091c5.png)

## Next up

In the [next exciting lesson](https://docs.apify.com/academy/puppeteer-playwright/executing-scripts.md), we'll gain a solid understanding of the two different contexts we can run our code in when using Puppeteer and Playwright, as well as how to run code in the context of the browser.
