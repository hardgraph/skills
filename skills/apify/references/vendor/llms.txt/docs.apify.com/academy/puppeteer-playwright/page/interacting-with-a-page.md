---
title: Interacting with a page
url: https://docs.apify.com/academy/puppeteer-playwright/page/interacting-with-a-page.md
parents:
  - [Apify documentation](https://docs.apify.com/llms.txt)
  - [Apify Academy](https://docs.apify.com/academy.md)
  - [Puppeteer and Playwright course](https://docs.apify.com/academy/puppeteer-playwright.md)
  - [II - Opening & controlling a page](https://docs.apify.com/academy/puppeteer-playwright/page.md)
previous: [II - Opening & controlling a page](https://docs.apify.com/academy/puppeteer-playwright/page.md)
next: [Waiting for elements and events](https://docs.apify.com/academy/puppeteer-playwright/page/waiting.md)
---

> ## Documentation index
> Fetch the complete documentation index at: https://docs.apify.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Interacting with a page

**Learn how to programmatically do actions on a page such as clicking, typing, and pressing keys. Also, discover a common roadblock that comes up when automating.**

***

The **Page** object has a whole boat-load of functions which can be used to interact with the loaded page. We're not going to go over every single one of them right now, but we *will* use a few of the most common ones to add some functionality to our current project.

Let's say that we want to automate searching for **hello world** on Google, then click on the first result and log the title of the page to the console, then take a screenshot and write it to the filesystem. In order to understand how we're going to automate this, let's break down how we would do it manually:

1. Click on the button which accepts Google's cookies policy (To see how it looks, open Google in an anonymous window.)
2. Type **hello world** into the search bar
3. Press **Enter**
4. Wait for the results page to load
5. Click on the first result
6. Read the title of the clicked result's loaded page
7. Screenshot the page

Though it seems complex, the wonderful **Page** API can help us with all the steps.

## Clicking & pressing keys

Let's first focus on the first 3 steps listed above. By using `page.click()` and the CSS selector of the element to click, we can click an element:

**Playwright**


```js
// Click the "Accept all" button

await page.click('button:has-text("Accept all")');
```


**Puppeteer**


```js
// Click the "Accept all" button

await page.click('button + button');
```


With `page.click()`, Puppeteer and Playwright actually drag the mouse and click, allowing the bot to act more human-like. This is different from programmatically clicking with `Element.click()` in vanilla client-side JavaScript.

Notice that in the Playwright example, we are using a different selector than in the Puppeteer example. This is because Playwright supports [many custom CSS selectors](https://playwright.dev/docs/other-locators#css-elements-matching-one-of-the-conditions), such as the **has-text** pseudo class. As a rule of thumb, using text selectors is much more preferable to using regular selectors, as they are much less likely to break. If Google makes the sibling above the **Accept all** button a `<div>` element instead of a `<button>` element, our `button + button` selector will break. However, the button will always have the text **Accept all**; therefore, `button:has-text("Accept all")` is more reliable.

> If you're not already familiar with CSS selectors and how to find them, we recommend referring to the [lesson about locating HTML elements with browser DevTools](https://docs.apify.com/academy/scraping-basics-javascript/devtools-locating-elements.md) from the **Web scraping basics for JavaScript devs** course.

Then, we can type some text into an input field `<textarea>` with `page.type()`; passing a CSS selector as the first, and the string to input as the second parameter:


```js
// Type the query into the search box

await page.type('textarea[title]', 'hello world');
```


Finally, we can press a single key by accessing the `keyboard` property of `page` and calling the `press()` function on it:


```js
// Press enter

await page.keyboard.press('Enter');
```


This is what we've got so far:

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });



const page = await browser.newPage();



await page.goto('https://www.google.com/');



// Click the "Accept all" button

await page.click('button:has-text("Accept all")');



// Type the query into the search box

await page.type('textarea[title]', 'hello world');



// Press enter

await page.keyboard.press('Enter');



await page.waitForTimeout(10000);

await browser.close();
```


**Puppeteer**


```js
import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });



const page = await browser.newPage();



await page.goto('https://www.google.com/');



// Click the "Accept all" button

await page.click('button + button');



// Type the query into the search box

await page.type('textarea[title]', 'hello world');



// Press enter

await page.keyboard.press('Enter');



await page.waitForTimeout(10000);

await browser.close();
```


When we run it, we leave off on the results page:

![Google results page reached by headless browser](/assets/images/google-results-7c52a69dcd7170b0a8d1a8b93b321811.png)

Great! Now all we have to do is click the first result which matches the CSS selector `.g a`:

**Playwright**


```js
import { chromium } from 'playwright';



const browser = await chromium.launch({ headless: false });



const page = await browser.newPage();



await page.goto('https://www.google.com/');



await page.click('button:has-text("Accept all")');



await page.type('textarea[title]', 'hello world');



await page.keyboard.press('Enter');



// Click the first result

await page.click('.g a');



await page.waitForTimeout(10000);

await browser.close();
```


**Puppeteer**


```js
// This code will throw an error!

import puppeteer from 'puppeteer';



const browser = await puppeteer.launch({ headless: false });



const page = await browser.newPage();



await page.goto('https://www.google.com/');



await page.click('button + button');



await page.type('textarea[title]', 'hello world');



await page.keyboard.press('Enter');



// Click the first result

await page.click('.g a');



await page.waitForTimeout(10000);

await browser.close();
```


But wait, when we try to run the Puppeteer code, we run into this nasty error:

> The following error won't be present if you're following the Playwright examples. You'll learn why in the next lesson.


```text
/Users/me/Desktop/playwright-puppeteer/node_modules/puppeteer/lib/cjs/puppeteer/common/assert.js:26

        throw new Error(message);

              ^



Error: No node found for selector: .g a

    at assert (/Users/me/Desktop/playwright-puppeteer/node_modules/puppeteer/lib/cjs/puppeteer/common/assert.js:26:15)

...
```


We hit this error because we attempted to click an element that wasn't yet present on the page. The results page hadn't even loaded yet!

## Next up

In the [next lesson](https://docs.apify.com/academy/puppeteer-playwright/page/waiting.md), we'll be taking a look at how to **wait for** navigation, events, and content before resuming interactions.
