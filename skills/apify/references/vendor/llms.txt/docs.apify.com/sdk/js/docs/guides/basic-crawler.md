# Basic crawler

Copy for LLM

This is the most bare-bones example of the Apify SDK, which demonstrates some of its building blocks such as the [`BasicCrawler`](https://crawlee.dev/api/basic-crawler/class/BasicCrawler). You probably don't need to go this deep though, and it would be better to start with one of the full-featured crawlers like [`CheerioCrawler`](https://crawlee.dev/api/cheerio-crawler/class/CheerioCrawler) or [`PlaywrightCrawler`](https://crawlee.dev/api/playwright-crawler/class/PlaywrightCrawler).

The script simply downloads several web pages with plain HTTP requests using the [`got-scraping`](https://github.com/apify/got-scraping) npm package and stores their raw HTML and URL in the default dataset. In local configuration, the data will be stored as JSON files in `./storage/datasets/default`.

[Run on](https://console.apify.com/actors/kk67IcZkKSSBTslXI?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IEJhc2ljQ3Jhd2xlciB9IGZyb20gJ2NyYXdsZWUnO1xcbmltcG9ydCB7IGdvdFNjcmFwaW5nIH0gZnJvbSAnZ290LXNjcmFwaW5nJztcXG5cXG5hd2FpdCBBY3Rvci5pbml0KCk7XFxuXFxuLy8gQ3JlYXRlIGEgZGF0YXNldCB3aGVyZSB3ZSB3aWxsIHN0b3JlIHRoZSByZXN1bHRzLlxcbi8vIENyZWF0ZSBhIEJhc2ljQ3Jhd2xlciAtIHRoZSBzaW1wbGVzdCBjcmF3bGVyIHRoYXQgZW5hYmxlc1xcbi8vIHVzZXJzIHRvIGltcGxlbWVudCB0aGUgY3Jhd2xpbmcgbG9naWMgdGhlbXNlbHZlcy5cXG5jb25zdCBjcmF3bGVyID0gbmV3IEJhc2ljQ3Jhd2xlcih7XFxuICAgIC8vIFRoaXMgZnVuY3Rpb24gd2lsbCBiZSBjYWxsZWQgZm9yIGVhY2ggVVJMIHRvIGNyYXdsLlxcbiAgICBhc3luYyByZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QgfSkge1xcbiAgICAgICAgY29uc3QgeyB1cmwgfSA9IHJlcXVlc3Q7XFxuICAgICAgICBjb25zb2xlLmxvZyhgUHJvY2Vzc2luZyAke3VybH0uLi5gKTtcXG5cXG4gICAgICAgIC8vIEZldGNoIHRoZSBwYWdlIEhUTUwgdmlhIEFwaWZ5IHV0aWxzIGdvdFNjcmFwaW5nXFxuICAgICAgICBjb25zdCB7IGJvZHkgfSA9IGF3YWl0IGdvdFNjcmFwaW5nKHsgdXJsIH0pO1xcblxcbiAgICAgICAgLy8gU3RvcmUgdGhlIEhUTUwgYW5kIFVSTCB0byB0aGUgZGVmYXVsdCBkYXRhc2V0LlxcbiAgICAgICAgYXdhaXQgQWN0b3IucHVzaERhdGEoe1xcbiAgICAgICAgICAgIHVybDogcmVxdWVzdC51cmwsXFxuICAgICAgICAgICAgaHRtbDogYm9keSxcXG4gICAgICAgIH0pO1xcbiAgICB9LFxcbn0pO1xcblxcbi8vIFRoZSBpbml0aWFsIGxpc3Qgb2YgVVJMcyB0byBjcmF3bC4gSGVyZSB3ZSB1c2UganVzdCBhIGZldyBoYXJkLWNvZGVkIFVSTHMuXFxuYXdhaXQgY3Jhd2xlci5ydW4oW1xcbiAgICB7IHVybDogJ2h0dHA6Ly93d3cuZ29vZ2xlLmNvbS8nIH0sXFxuICAgIHsgdXJsOiAnaHR0cDovL3d3dy5leGFtcGxlLmNvbS8nIH0sXFxuICAgIHsgdXJsOiAnaHR0cDovL3d3dy5iaW5nLmNvbS8nIH0sXFxuICAgIHsgdXJsOiAnaHR0cDovL3d3dy53aWtpcGVkaWEuY29tLycgfSxcXG5dKTtcXG5cXG5jb25zb2xlLmxvZygnQ3Jhd2xlciBmaW5pc2hlZC4nKTtcXG5cXG5hd2FpdCBBY3Rvci5leGl0KCk7XFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.-TdH8qt-fjSHPGoP8mJHr2LqYkhq6aWUhY9IdesMFrM\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { BasicCrawler } from 'crawlee';

import { gotScraping } from 'got-scraping';



await Actor.init();



// Create a dataset where we will store the results.

// Create a BasicCrawler - the simplest crawler that enables

// users to implement the crawling logic themselves.

const crawler = new BasicCrawler({

    // This function will be called for each URL to crawl.

    async requestHandler({ request }) {

        const { url } = request;

        console.log(`Processing ${url}...`);



        // Fetch the page HTML via Apify utils gotScraping

        const { body } = await gotScraping({ url });



        // Store the HTML and URL to the default dataset.

        await Actor.pushData({

            url: request.url,

            html: body,

        });

    },

});



// The initial list of URLs to crawl. Here we use just a few hard-coded URLs.

await crawler.run([

    { url: 'http://www.google.com/' },

    { url: 'http://www.example.com/' },

    { url: 'http://www.bing.com/' },

    { url: 'http://www.wikipedia.com/' },

]);



console.log('Crawler finished.');



await Actor.exit();
```
