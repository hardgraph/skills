# Crawl all links on a website

Copy for LLM

This example uses the `enqueueLinks()` method to add new links to the `RequestQueue` as the crawler navigates from page to page. If only the required parameters are defined, all links will be crawled.

* Cheerio Crawler
* Puppeteer Crawler
* Playwright Crawler

Using `CheerioCrawler`:

[Run on](https://console.apify.com/actors/kk67IcZkKSSBTslXI?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IENoZWVyaW9DcmF3bGVyIH0gZnJvbSAnY3Jhd2xlZSc7XFxuXFxuYXdhaXQgQWN0b3IuaW5pdCgpO1xcblxcbmNvbnN0IGNyYXdsZXIgPSBuZXcgQ2hlZXJpb0NyYXdsZXIoe1xcbiAgICBhc3luYyByZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QsIGVucXVldWVMaW5rcyB9KSB7XFxuICAgICAgICBjb25zb2xlLmxvZyhyZXF1ZXN0LnVybCk7XFxuICAgICAgICAvLyBBZGQgYWxsIGxpbmtzIGZyb20gcGFnZSB0byBSZXF1ZXN0UXVldWVcXG4gICAgICAgIGF3YWl0IGVucXVldWVMaW5rcygpO1xcbiAgICB9LFxcbiAgICBtYXhSZXF1ZXN0c1BlckNyYXdsOiAxMCwgLy8gTGltaXRhdGlvbiBmb3Igb25seSAxMCByZXF1ZXN0cyAoZG8gbm90IHVzZSBpZiB5b3Ugd2FudCB0byBjcmF3bCBhbGwgbGlua3MpXFxufSk7XFxuXFxuLy8gUnVuIHRoZSBjcmF3bGVyXFxuYXdhaXQgY3Jhd2xlci5ydW4oWydodHRwczovL2FwaWZ5LmNvbS8nXSk7XFxuXFxuYXdhaXQgQWN0b3IuZXhpdCgpO1xcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.WZ0oMu6yd1pBKWHbkngs3qzaOVhpacPP6PKxjXnRLbc\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { CheerioCrawler } from 'crawlee';



await Actor.init();



const crawler = new CheerioCrawler({

    async requestHandler({ request, enqueueLinks }) {

        console.log(request.url);

        // Add all links from page to RequestQueue

        await enqueueLinks();

    },

    maxRequestsPerCrawl: 10, // Limitation for only 10 requests (do not use if you want to crawl all links)

});



// Run the crawler

await crawler.run(['https://apify.com/']);



await Actor.exit();
```

Using `PuppeteerCrawler`:

tip

To run this example on the Apify Platform, select the `apify/actor-node-puppeteer-chrome` image for your Dockerfile.

[Run on](https://console.apify.com/actors/7tWSD8hrYzuc9Lte7?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IFB1cHBldGVlckNyYXdsZXIgfSBmcm9tICdjcmF3bGVlJztcXG5cXG5hd2FpdCBBY3Rvci5pbml0KCk7XFxuXFxuY29uc3QgY3Jhd2xlciA9IG5ldyBQdXBwZXRlZXJDcmF3bGVyKHtcXG4gICAgYXN5bmMgcmVxdWVzdEhhbmRsZXIoeyByZXF1ZXN0LCBlbnF1ZXVlTGlua3MgfSkge1xcbiAgICAgICAgY29uc29sZS5sb2cocmVxdWVzdC51cmwpO1xcbiAgICAgICAgLy8gQWRkIGFsbCBsaW5rcyBmcm9tIHBhZ2UgdG8gUmVxdWVzdFF1ZXVlXFxuICAgICAgICBhd2FpdCBlbnF1ZXVlTGlua3MoKTtcXG4gICAgfSxcXG4gICAgbWF4UmVxdWVzdHNQZXJDcmF3bDogMTAsIC8vIExpbWl0YXRpb24gZm9yIG9ubHkgMTAgcmVxdWVzdHMgKGRvIG5vdCB1c2UgaWYgeW91IHdhbnQgdG8gY3Jhd2wgYWxsIGxpbmtzKVxcbn0pO1xcblxcbi8vIFJ1biB0aGUgY3Jhd2xlclxcbmF3YWl0IGNyYXdsZXIucnVuKFsnaHR0cHM6Ly9hcGlmeS5jb20vJ10pO1xcblxcbmF3YWl0IEFjdG9yLmV4aXQoKTtcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5Ijo0MDk2LCJ0aW1lb3V0IjoxODB9fQ.gNhqxwBfIYMReWTkgUMf9WC-YJ_1Vy7-cQOmxNZDobM\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { PuppeteerCrawler } from 'crawlee';



await Actor.init();



const crawler = new PuppeteerCrawler({

    async requestHandler({ request, enqueueLinks }) {

        console.log(request.url);

        // Add all links from page to RequestQueue

        await enqueueLinks();

    },

    maxRequestsPerCrawl: 10, // Limitation for only 10 requests (do not use if you want to crawl all links)

});



// Run the crawler

await crawler.run(['https://apify.com/']);



await Actor.exit();
```

Using `PlaywrightCrawler`:

tip

To run this example on the Apify Platform, select the `apify/actor-node-playwright-chrome` image for your Dockerfile.

[Run on](https://console.apify.com/actors/6i5QsHBMtm3hKph70?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IFBsYXl3cmlnaHRDcmF3bGVyIH0gZnJvbSAnY3Jhd2xlZSc7XFxuXFxuYXdhaXQgQWN0b3IuaW5pdCgpO1xcblxcbmNvbnN0IGNyYXdsZXIgPSBuZXcgUGxheXdyaWdodENyYXdsZXIoe1xcbiAgICBhc3luYyByZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QsIGVucXVldWVMaW5rcyB9KSB7XFxuICAgICAgICBjb25zb2xlLmxvZyhyZXF1ZXN0LnVybCk7XFxuICAgICAgICAvLyBBZGQgYWxsIGxpbmtzIGZyb20gcGFnZSB0byBSZXF1ZXN0UXVldWVcXG4gICAgICAgIGF3YWl0IGVucXVldWVMaW5rcygpO1xcbiAgICB9LFxcbiAgICBtYXhSZXF1ZXN0c1BlckNyYXdsOiAxMCwgLy8gTGltaXRhdGlvbiBmb3Igb25seSAxMCByZXF1ZXN0cyAoZG8gbm90IHVzZSBpZiB5b3Ugd2FudCB0byBjcmF3bCBhbGwgbGlua3MpXFxufSk7XFxuXFxuLy8gUnVuIHRoZSBjcmF3bGVyXFxuYXdhaXQgY3Jhd2xlci5ydW4oWydodHRwczovL2FwaWZ5LmNvbS8nXSk7XFxuXFxuYXdhaXQgQWN0b3IuZXhpdCgpO1xcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjQwOTYsInRpbWVvdXQiOjE4MH19.eVg0BTpLuA9jZtmijHGMjetPuME0zmTZX4oo8kxSAh8\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { PlaywrightCrawler } from 'crawlee';



await Actor.init();



const crawler = new PlaywrightCrawler({

    async requestHandler({ request, enqueueLinks }) {

        console.log(request.url);

        // Add all links from page to RequestQueue

        await enqueueLinks();

    },

    maxRequestsPerCrawl: 10, // Limitation for only 10 requests (do not use if you want to crawl all links)

});



// Run the crawler

await crawler.run(['https://apify.com/']);



await Actor.exit();
```
