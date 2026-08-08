# Crawl a sitemap

Copy for LLM

This example downloads and crawls the URLs from a sitemap.

* Cheerio Crawler
* Puppeteer Crawler
* Playwright Crawler

Using `CheerioCrawler`:

[Run on](https://console.apify.com/actors/kk67IcZkKSSBTslXI?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IENoZWVyaW9DcmF3bGVyLCBkb3dubG9hZExpc3RPZlVybHMgfSBmcm9tICdjcmF3bGVlJztcXG5cXG5hd2FpdCBBY3Rvci5pbml0KCk7XFxuXFxuY29uc3QgY3Jhd2xlciA9IG5ldyBDaGVlcmlvQ3Jhd2xlcih7XFxuICAgIC8vIEZ1bmN0aW9uIGNhbGxlZCBmb3IgZWFjaCBVUkxcXG4gICAgYXN5bmMgcmVxdWVzdEhhbmRsZXIoeyByZXF1ZXN0IH0pIHtcXG4gICAgICAgIGNvbnNvbGUubG9nKHJlcXVlc3QudXJsKTtcXG4gICAgfSxcXG4gICAgbWF4UmVxdWVzdHNQZXJDcmF3bDogMTAsIC8vIExpbWl0YXRpb24gZm9yIG9ubHkgMTAgcmVxdWVzdHMgKGRvIG5vdCB1c2UgaWYgeW91IHdhbnQgdG8gY3Jhd2wgYSBzaXRlbWFwKVxcbn0pO1xcblxcbmNvbnN0IGxpc3RPZlVybHMgPSBhd2FpdCBkb3dubG9hZExpc3RPZlVybHMoe1xcbiAgICB1cmw6ICdodHRwczovL2FwaWZ5LmNvbS9zaXRlbWFwLnhtbCcsXFxufSk7XFxuXFxuLy8gUnVuIHRoZSBjcmF3bGVyXFxuYXdhaXQgY3Jhd2xlci5ydW4obGlzdE9mVXJscyk7XFxuXFxuYXdhaXQgQWN0b3IuZXhpdCgpO1xcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.XWC3QQWKIsRIB8TdL40CGjzvHiqadKnt7F-9rhoHEEo\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { CheerioCrawler, downloadListOfUrls } from 'crawlee';



await Actor.init();



const crawler = new CheerioCrawler({

    // Function called for each URL

    async requestHandler({ request }) {

        console.log(request.url);

    },

    maxRequestsPerCrawl: 10, // Limitation for only 10 requests (do not use if you want to crawl a sitemap)

});



const listOfUrls = await downloadListOfUrls({

    url: 'https://apify.com/sitemap.xml',

});



// Run the crawler

await crawler.run(listOfUrls);



await Actor.exit();
```

Using `PuppeteerCrawler`:

tip

To run this example on the Apify Platform, select the `apify/actor-node-puppeteer-chrome` image for your Dockerfile.

[Run on](https://console.apify.com/actors/7tWSD8hrYzuc9Lte7?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IGRvd25sb2FkTGlzdE9mVXJscywgUHVwcGV0ZWVyQ3Jhd2xlciB9IGZyb20gJ2NyYXdsZWUnO1xcblxcbmF3YWl0IEFjdG9yLmluaXQoKTtcXG5cXG5jb25zdCBjcmF3bGVyID0gbmV3IFB1cHBldGVlckNyYXdsZXIoe1xcbiAgICAvLyBGdW5jdGlvbiBjYWxsZWQgZm9yIGVhY2ggVVJMXFxuICAgIGFzeW5jIHJlcXVlc3RIYW5kbGVyKHsgcmVxdWVzdCB9KSB7XFxuICAgICAgICBjb25zb2xlLmxvZyhyZXF1ZXN0LnVybCk7XFxuICAgIH0sXFxuICAgIG1heFJlcXVlc3RzUGVyQ3Jhd2w6IDEwLCAvLyBMaW1pdGF0aW9uIGZvciBvbmx5IDEwIHJlcXVlc3RzIChkbyBub3QgdXNlIGlmIHlvdSB3YW50IHRvIGNyYXdsIGEgc2l0ZW1hcClcXG59KTtcXG5cXG5jb25zdCBsaXN0T2ZVcmxzID0gYXdhaXQgZG93bmxvYWRMaXN0T2ZVcmxzKHtcXG4gICAgdXJsOiAnaHR0cHM6Ly9hcGlmeS5jb20vc2l0ZW1hcC54bWwnLFxcbn0pO1xcblxcbi8vIFJ1biB0aGUgY3Jhd2xlclxcbmF3YWl0IGNyYXdsZXIucnVuKGxpc3RPZlVybHMpO1xcblxcbmF3YWl0IEFjdG9yLmV4aXQoKTtcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5Ijo0MDk2LCJ0aW1lb3V0IjoxODB9fQ._LOESjvhSiJamXz1EhpRWvA_afgRmfQABfI1Wgts8c8\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { downloadListOfUrls, PuppeteerCrawler } from 'crawlee';



await Actor.init();



const crawler = new PuppeteerCrawler({

    // Function called for each URL

    async requestHandler({ request }) {

        console.log(request.url);

    },

    maxRequestsPerCrawl: 10, // Limitation for only 10 requests (do not use if you want to crawl a sitemap)

});



const listOfUrls = await downloadListOfUrls({

    url: 'https://apify.com/sitemap.xml',

});



// Run the crawler

await crawler.run(listOfUrls);



await Actor.exit();
```

Using `PlaywrightCrawler`:

tip

To run this example on the Apify Platform, select the `apify/actor-node-playwright-chrome` image for your Dockerfile.

[Run on](https://console.apify.com/actors/6i5QsHBMtm3hKph70?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IGRvd25sb2FkTGlzdE9mVXJscywgUGxheXdyaWdodENyYXdsZXIgfSBmcm9tICdjcmF3bGVlJztcXG5cXG5hd2FpdCBBY3Rvci5pbml0KCk7XFxuXFxuY29uc3QgY3Jhd2xlciA9IG5ldyBQbGF5d3JpZ2h0Q3Jhd2xlcih7XFxuICAgIC8vIEZ1bmN0aW9uIGNhbGxlZCBmb3IgZWFjaCBVUkxcXG4gICAgYXN5bmMgcmVxdWVzdEhhbmRsZXIoeyByZXF1ZXN0IH0pIHtcXG4gICAgICAgIGNvbnNvbGUubG9nKHJlcXVlc3QudXJsKTtcXG4gICAgfSxcXG4gICAgbWF4UmVxdWVzdHNQZXJDcmF3bDogMTAsIC8vIExpbWl0YXRpb24gZm9yIG9ubHkgMTAgcmVxdWVzdHMgKGRvIG5vdCB1c2UgaWYgeW91IHdhbnQgdG8gY3Jhd2wgYSBzaXRlbWFwKVxcbn0pO1xcblxcbmNvbnN0IGxpc3RPZlVybHMgPSBhd2FpdCBkb3dubG9hZExpc3RPZlVybHMoe1xcbiAgICB1cmw6ICdodHRwczovL2FwaWZ5LmNvbS9zaXRlbWFwLnhtbCcsXFxufSk7XFxuXFxuLy8gUnVuIHRoZSBjcmF3bGVyXFxuYXdhaXQgY3Jhd2xlci5ydW4obGlzdE9mVXJscyk7XFxuXFxuYXdhaXQgQWN0b3IuZXhpdCgpO1xcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjQwOTYsInRpbWVvdXQiOjE4MH19.qbl4ro1qZvqNhlkeysCWDSDwM0LV0A3CVXl89bDLbR4\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { downloadListOfUrls, PlaywrightCrawler } from 'crawlee';



await Actor.init();



const crawler = new PlaywrightCrawler({

    // Function called for each URL

    async requestHandler({ request }) {

        console.log(request.url);

    },

    maxRequestsPerCrawl: 10, // Limitation for only 10 requests (do not use if you want to crawl a sitemap)

});



const listOfUrls = await downloadListOfUrls({

    url: 'https://apify.com/sitemap.xml',

});



// Run the crawler

await crawler.run(listOfUrls);



await Actor.exit();
```
