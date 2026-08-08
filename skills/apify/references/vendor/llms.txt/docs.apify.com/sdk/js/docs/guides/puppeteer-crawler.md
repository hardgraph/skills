# Puppeteer crawler

Copy for LLM

This example demonstrates how to use [`PuppeteerCrawler`](https://crawlee.dev/api/puppeteer-crawler/class/PuppeteerCrawler) in combination with [`RequestQueue`](https://docs.apify.com/sdk/js/sdk/js/reference/class/RequestQueue.md) to recursively scrape the [Hacker News website](https://news.ycombinator.com) using headless Chrome / Puppeteer.

The crawler starts with a single URL, finds links to next pages, enqueues them and continues until no more desired links are available. The results are stored to the default dataset. In local configuration, the results are stored as JSON files in `./storage/datasets/default`

tip

To run this example on the Apify Platform, select the `apify/actor-node-puppeteer-chrome` image for your Dockerfile.

[Run on](https://console.apify.com/actors/7tWSD8hrYzuc9Lte7?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IFB1cHBldGVlckNyYXdsZXIgfSBmcm9tICdjcmF3bGVlJztcXG5cXG5hd2FpdCBBY3Rvci5pbml0KCk7XFxuXFxuLy8gQ3JlYXRlIGFuIGluc3RhbmNlIG9mIHRoZSBQdXBwZXRlZXJDcmF3bGVyIGNsYXNzIC0gYSBjcmF3bGVyXFxuLy8gdGhhdCBhdXRvbWF0aWNhbGx5IGxvYWRzIHRoZSBVUkxzIGluIGhlYWRsZXNzIENocm9tZSAvIFB1cHBldGVlci5cXG5jb25zdCBjcmF3bGVyID0gbmV3IFB1cHBldGVlckNyYXdsZXIoe1xcbiAgICAvLyBIZXJlIHlvdSBjYW4gc2V0IG9wdGlvbnMgdGhhdCBhcmUgcGFzc2VkIHRvIHRoZSBsYXVuY2hQdXBwZXRlZXIoKSBmdW5jdGlvbi5cXG4gICAgbGF1bmNoQ29udGV4dDoge1xcbiAgICAgICAgbGF1bmNoT3B0aW9uczoge1xcbiAgICAgICAgICAgIGhlYWRsZXNzOiB0cnVlLFxcbiAgICAgICAgICAgIC8vIE90aGVyIFB1cHBldGVlciBvcHRpb25zXFxuICAgICAgICB9LFxcbiAgICB9LFxcblxcbiAgICAvLyBTdG9wIGNyYXdsaW5nIGFmdGVyIHNldmVyYWwgcGFnZXNcXG4gICAgbWF4UmVxdWVzdHNQZXJDcmF3bDogNTAsXFxuXFxuICAgIC8vIFRoaXMgZnVuY3Rpb24gd2lsbCBiZSBjYWxsZWQgZm9yIGVhY2ggVVJMIHRvIGNyYXdsLlxcbiAgICAvLyBIZXJlIHlvdSBjYW4gd3JpdGUgdGhlIFB1cHBldGVlciBzY3JpcHRzIHlvdSBhcmUgZmFtaWxpYXIgd2l0aCxcXG4gICAgLy8gd2l0aCB0aGUgZXhjZXB0aW9uIHRoYXQgYnJvd3NlcnMgYW5kIHBhZ2VzIGFyZSBhdXRvbWF0aWNhbGx5IG1hbmFnZWQgYnkgdGhlIEFwaWZ5IFNESy5cXG4gICAgLy8gVGhlIGZ1bmN0aW9uIGFjY2VwdHMgYSBzaW5nbGUgcGFyYW1ldGVyLCB3aGljaCBpcyBhbiBvYmplY3Qgd2l0aCB0aGUgZm9sbG93aW5nIGZpZWxkczpcXG4gICAgLy8gLSByZXF1ZXN0OiBhbiBpbnN0YW5jZSBvZiB0aGUgUmVxdWVzdCBjbGFzcyB3aXRoIGluZm9ybWF0aW9uIHN1Y2ggYXMgVVJMIGFuZCBIVFRQIG1ldGhvZFxcbiAgICAvLyAtIHBhZ2U6IFB1cHBldGVlcidzIFBhZ2Ugb2JqZWN0IChzZWUgaHR0cHM6Ly9wcHRyLmRldi8jc2hvdz1hcGktY2xhc3MtcGFnZSlcXG4gICAgYXN5bmMgcmVxdWVzdEhhbmRsZXIoeyByZXF1ZXN0LCBwYWdlLCBlbnF1ZXVlTGlua3MgfSkge1xcbiAgICAgICAgY29uc29sZS5sb2coYFByb2Nlc3NpbmcgJHtyZXF1ZXN0LnVybH0uLi5gKTtcXG5cXG4gICAgICAgIC8vIEEgZnVuY3Rpb24gdG8gYmUgZXZhbHVhdGVkIGJ5IFB1cHBldGVlciB3aXRoaW4gdGhlIGJyb3dzZXIgY29udGV4dC5cXG4gICAgICAgIGNvbnN0IGRhdGEgPSBhd2FpdCBwYWdlLiQkZXZhbCgnLmF0aGluZycsICgkcG9zdHMpID0-IHtcXG4gICAgICAgICAgICBjb25zdCBzY3JhcGVkRGF0YSA9IFtdO1xcblxcbiAgICAgICAgICAgIC8vIFdlJ3JlIGdldHRpbmcgdGhlIHRpdGxlLCByYW5rIGFuZCBVUkwgb2YgZWFjaCBwb3N0IG9uIEhhY2tlciBOZXdzLlxcbiAgICAgICAgICAgICRwb3N0cy5mb3JFYWNoKCgkcG9zdCkgPT4ge1xcbiAgICAgICAgICAgICAgICBzY3JhcGVkRGF0YS5wdXNoKHtcXG4gICAgICAgICAgICAgICAgICAgIHRpdGxlOiAkcG9zdC5xdWVyeVNlbGVjdG9yKCcudGl0bGUgYScpLmlubmVyVGV4dCxcXG4gICAgICAgICAgICAgICAgICAgIHJhbms6ICRwb3N0LnF1ZXJ5U2VsZWN0b3IoJy5yYW5rJykuaW5uZXJUZXh0LFxcbiAgICAgICAgICAgICAgICAgICAgaHJlZjogJHBvc3QucXVlcnlTZWxlY3RvcignLnRpdGxlIGEnKS5ocmVmLFxcbiAgICAgICAgICAgICAgICB9KTtcXG4gICAgICAgICAgICB9KTtcXG5cXG4gICAgICAgICAgICByZXR1cm4gc2NyYXBlZERhdGE7XFxuICAgICAgICB9KTtcXG5cXG4gICAgICAgIC8vIFN0b3JlIHRoZSByZXN1bHRzIHRvIHRoZSBkZWZhdWx0IGRhdGFzZXQuXFxuICAgICAgICBhd2FpdCBBY3Rvci5wdXNoRGF0YShkYXRhKTtcXG5cXG4gICAgICAgIC8vIEZpbmQgYSBsaW5rIHRvIHRoZSBuZXh0IHBhZ2UgYW5kIGVucXVldWUgaXQgaWYgaXQgZXhpc3RzLlxcbiAgICAgICAgY29uc3QgaW5mb3MgPSBhd2FpdCBlbnF1ZXVlTGlua3Moe1xcbiAgICAgICAgICAgIHNlbGVjdG9yOiAnLm1vcmVsaW5rJyxcXG4gICAgICAgIH0pO1xcblxcbiAgICAgICAgaWYgKGluZm9zLmxlbmd0aCA9PT0gMCkgY29uc29sZS5sb2coYCR7cmVxdWVzdC51cmx9IGlzIHRoZSBsYXN0IHBhZ2UhYCk7XFxuICAgIH0sXFxuXFxuICAgIC8vIFRoaXMgZnVuY3Rpb24gaXMgY2FsbGVkIGlmIHRoZSBwYWdlIHByb2Nlc3NpbmcgZmFpbGVkIG1vcmUgdGhhbiBtYXhSZXF1ZXN0UmV0cmllcysxIHRpbWVzLlxcbiAgICBmYWlsZWRSZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QgfSkge1xcbiAgICAgICAgY29uc29sZS5sb2coYFJlcXVlc3QgJHtyZXF1ZXN0LnVybH0gZmFpbGVkIHRvbyBtYW55IHRpbWVzLmApO1xcbiAgICB9LFxcbn0pO1xcblxcbi8vIFJ1biB0aGUgY3Jhd2xlciBhbmQgd2FpdCBmb3IgaXQgdG8gZmluaXNoLlxcbmF3YWl0IGNyYXdsZXIucnVuKFsnaHR0cHM6Ly9uZXdzLnljb21iaW5hdG9yLmNvbS8nXSk7XFxuXFxuY29uc29sZS5sb2coJ0NyYXdsZXIgZmluaXNoZWQuJyk7XFxuXFxuYXdhaXQgQWN0b3IuZXhpdCgpO1xcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjQwOTYsInRpbWVvdXQiOjE4MH19.88cqtP3DJA1811DUd2fOqdjsLFRPvz91Pi_WHe8Yt5U\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { PuppeteerCrawler } from 'crawlee';



await Actor.init();



// Create an instance of the PuppeteerCrawler class - a crawler

// that automatically loads the URLs in headless Chrome / Puppeteer.

const crawler = new PuppeteerCrawler({

    // Here you can set options that are passed to the launchPuppeteer() function.

    launchContext: {

        launchOptions: {

            headless: true,

            // Other Puppeteer options

        },

    },



    // Stop crawling after several pages

    maxRequestsPerCrawl: 50,



    // This function will be called for each URL to crawl.

    // Here you can write the Puppeteer scripts you are familiar with,

    // with the exception that browsers and pages are automatically managed by the Apify SDK.

    // The function accepts a single parameter, which is an object with the following fields:

    // - request: an instance of the Request class with information such as URL and HTTP method

    // - page: Puppeteer's Page object (see https://pptr.dev/#show=api-class-page)

    async requestHandler({ request, page, enqueueLinks }) {

        console.log(`Processing ${request.url}...`);



        // A function to be evaluated by Puppeteer within the browser context.

        const data = await page.$$eval('.athing', ($posts) => {

            const scrapedData = [];



            // We're getting the title, rank and URL of each post on Hacker News.

            $posts.forEach(($post) => {

                scrapedData.push({

                    title: $post.querySelector('.title a').innerText,

                    rank: $post.querySelector('.rank').innerText,

                    href: $post.querySelector('.title a').href,

                });

            });



            return scrapedData;

        });



        // Store the results to the default dataset.

        await Actor.pushData(data);



        // Find a link to the next page and enqueue it if it exists.

        const infos = await enqueueLinks({

            selector: '.morelink',

        });



        if (infos.length === 0) console.log(`${request.url} is the last page!`);

    },



    // This function is called if the page processing failed more than maxRequestRetries+1 times.

    failedRequestHandler({ request }) {

        console.log(`Request ${request.url} failed too many times.`);

    },

});



// Run the crawler and wait for it to finish.

await crawler.run(['https://news.ycombinator.com/']);



console.log('Crawler finished.');



await Actor.exit();
```
