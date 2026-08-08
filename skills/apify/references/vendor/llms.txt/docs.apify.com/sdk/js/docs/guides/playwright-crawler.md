# Playwright crawler

Copy for LLM

This example demonstrates how to use [`PlaywrightCrawler`](https://crawlee.dev/api/playwright-crawler/class/PlaywrightCrawler) in combination with [`RequestQueue`](https://docs.apify.com/sdk/js/sdk/js/reference/class/RequestQueue.md) to recursively scrape the [Hacker News website](https://news.ycombinator.com) using headless Chrome / Playwright.

The crawler starts with a single URL, finds links to next pages, enqueues them and continues until no more desired links are available. The results are stored to the default dataset. In local configuration, the results are stored as JSON files in `./storage/datasets/default`

tip

To run this example on the Apify Platform, select the `apify/actor-node-playwright-chrome` image for your Dockerfile.

[Run on](https://console.apify.com/actors/6i5QsHBMtm3hKph70?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IEFjdG9yIH0gZnJvbSAnYXBpZnknO1xcbmltcG9ydCB7IFBsYXl3cmlnaHRDcmF3bGVyIH0gZnJvbSAnY3Jhd2xlZSc7XFxuXFxuYXdhaXQgQWN0b3IuaW5pdCgpO1xcblxcbi8vIENyZWF0ZSBhbiBpbnN0YW5jZSBvZiB0aGUgUGxheXdyaWdodENyYXdsZXIgY2xhc3MgLSBhIGNyYXdsZXJcXG4vLyB0aGF0IGF1dG9tYXRpY2FsbHkgbG9hZHMgdGhlIFVSTHMgaW4gaGVhZGxlc3MgQ2hyb21lIC8gUGxheXdyaWdodC5cXG5jb25zdCBjcmF3bGVyID0gbmV3IFBsYXl3cmlnaHRDcmF3bGVyKHtcXG4gICAgbGF1bmNoQ29udGV4dDoge1xcbiAgICAgICAgLy8gSGVyZSB5b3UgY2FuIHNldCBvcHRpb25zIHRoYXQgYXJlIHBhc3NlZCB0byB0aGUgcGxheXdyaWdodCAubGF1bmNoKCkgZnVuY3Rpb24uXFxuICAgICAgICBsYXVuY2hPcHRpb25zOiB7XFxuICAgICAgICAgICAgaGVhZGxlc3M6IHRydWUsXFxuICAgICAgICB9LFxcbiAgICB9LFxcblxcbiAgICAvLyBTdG9wIGNyYXdsaW5nIGFmdGVyIHNldmVyYWwgcGFnZXNcXG4gICAgbWF4UmVxdWVzdHNQZXJDcmF3bDogNTAsXFxuXFxuICAgIC8vIFRoaXMgZnVuY3Rpb24gd2lsbCBiZSBjYWxsZWQgZm9yIGVhY2ggVVJMIHRvIGNyYXdsLlxcbiAgICAvLyBIZXJlIHlvdSBjYW4gd3JpdGUgdGhlIFBsYXl3cmlnaHQgc2NyaXB0cyB5b3UgYXJlIGZhbWlsaWFyIHdpdGgsXFxuICAgIC8vIHdpdGggdGhlIGV4Y2VwdGlvbiB0aGF0IGJyb3dzZXJzIGFuZCBwYWdlcyBhcmUgYXV0b21hdGljYWxseSBtYW5hZ2VkIGJ5IHRoZSBBcGlmeSBTREsuXFxuICAgIC8vIFRoZSBmdW5jdGlvbiBhY2NlcHRzIGEgc2luZ2xlIHBhcmFtZXRlciwgd2hpY2ggaXMgYW4gb2JqZWN0IHdpdGggYSBsb3Qgb2YgcHJvcGVydGllcyxcXG4gICAgLy8gdGhlIG1vc3QgaW1wb3J0YW50IGJlaW5nOlxcbiAgICAvLyAtIHJlcXVlc3Q6IGFuIGluc3RhbmNlIG9mIHRoZSBSZXF1ZXN0IGNsYXNzIHdpdGggaW5mb3JtYXRpb24gc3VjaCBhcyBVUkwgYW5kIEhUVFAgbWV0aG9kXFxuICAgIC8vIC0gcGFnZTogUGxheXdyaWdodCdzIFBhZ2Ugb2JqZWN0IChzZWUgaHR0cHM6Ly9wbGF5d3JpZ2h0LmRldi9kb2NzL2FwaS9jbGFzcy1wYWdlKVxcbiAgICBhc3luYyByZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QsIHBhZ2UsIGVucXVldWVMaW5rcyB9KSB7XFxuICAgICAgICBjb25zb2xlLmxvZyhgUHJvY2Vzc2luZyAke3JlcXVlc3QudXJsfS4uLmApO1xcblxcbiAgICAgICAgLy8gQSBmdW5jdGlvbiB0byBiZSBldmFsdWF0ZWQgYnkgUGxheXdyaWdodCB3aXRoaW4gdGhlIGJyb3dzZXIgY29udGV4dC5cXG4gICAgICAgIGNvbnN0IGRhdGEgPSBhd2FpdCBwYWdlLiQkZXZhbCgnLmF0aGluZycsICgkcG9zdHMpID0-IHtcXG4gICAgICAgICAgICBjb25zdCBzY3JhcGVkRGF0YSA9IFtdO1xcblxcbiAgICAgICAgICAgIC8vIFdlJ3JlIGdldHRpbmcgdGhlIHRpdGxlLCByYW5rIGFuZCBVUkwgb2YgZWFjaCBwb3N0IG9uIEhhY2tlciBOZXdzLlxcbiAgICAgICAgICAgICRwb3N0cy5mb3JFYWNoKCgkcG9zdCkgPT4ge1xcbiAgICAgICAgICAgICAgICBzY3JhcGVkRGF0YS5wdXNoKHtcXG4gICAgICAgICAgICAgICAgICAgIHRpdGxlOiAkcG9zdC5xdWVyeVNlbGVjdG9yKCcudGl0bGUgYScpLmlubmVyVGV4dCxcXG4gICAgICAgICAgICAgICAgICAgIHJhbms6ICRwb3N0LnF1ZXJ5U2VsZWN0b3IoJy5yYW5rJykuaW5uZXJUZXh0LFxcbiAgICAgICAgICAgICAgICAgICAgaHJlZjogJHBvc3QucXVlcnlTZWxlY3RvcignLnRpdGxlIGEnKS5ocmVmLFxcbiAgICAgICAgICAgICAgICB9KTtcXG4gICAgICAgICAgICB9KTtcXG5cXG4gICAgICAgICAgICByZXR1cm4gc2NyYXBlZERhdGE7XFxuICAgICAgICB9KTtcXG5cXG4gICAgICAgIC8vIFN0b3JlIHRoZSByZXN1bHRzIHRvIHRoZSBkZWZhdWx0IGRhdGFzZXQuXFxuICAgICAgICBhd2FpdCBBY3Rvci5wdXNoRGF0YShkYXRhKTtcXG5cXG4gICAgICAgIC8vIEZpbmQgYSBsaW5rIHRvIHRoZSBuZXh0IHBhZ2UgYW5kIGVucXVldWUgaXQgaWYgaXQgZXhpc3RzLlxcbiAgICAgICAgY29uc3QgaW5mb3MgPSBhd2FpdCBlbnF1ZXVlTGlua3Moe1xcbiAgICAgICAgICAgIHNlbGVjdG9yOiAnLm1vcmVsaW5rJyxcXG4gICAgICAgIH0pO1xcblxcbiAgICAgICAgaWYgKGluZm9zLnByb2Nlc3NlZFJlcXVlc3RzLmxlbmd0aCA9PT0gMClcXG4gICAgICAgICAgICBjb25zb2xlLmxvZyhgJHtyZXF1ZXN0LnVybH0gaXMgdGhlIGxhc3QgcGFnZSFgKTtcXG4gICAgfSxcXG5cXG4gICAgLy8gVGhpcyBmdW5jdGlvbiBpcyBjYWxsZWQgaWYgdGhlIHBhZ2UgcHJvY2Vzc2luZyBmYWlsZWQgbW9yZSB0aGFuIG1heFJlcXVlc3RSZXRyaWVzKzEgdGltZXMuXFxuICAgIGZhaWxlZFJlcXVlc3RIYW5kbGVyKHsgcmVxdWVzdCB9KSB7XFxuICAgICAgICBjb25zb2xlLmxvZyhgUmVxdWVzdCAke3JlcXVlc3QudXJsfSBmYWlsZWQgdG9vIG1hbnkgdGltZXMuYCk7XFxuICAgIH0sXFxufSk7XFxuXFxuLy8gUnVuIHRoZSBjcmF3bGVyIGFuZCB3YWl0IGZvciBpdCB0byBmaW5pc2guXFxuYXdhaXQgY3Jhd2xlci5ydW4oWydodHRwczovL25ld3MueWNvbWJpbmF0b3IuY29tLyddKTtcXG5cXG5jb25zb2xlLmxvZygnQ3Jhd2xlciBmaW5pc2hlZC4nKTtcXG5cXG5hd2FpdCBBY3Rvci5leGl0KCk7XFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6NDA5NiwidGltZW91dCI6MTgwfX0.pYKENUrfvL61rPML7uc96hLxWD7O0UxTc_ZALKmFpyA\&asrc=run_on_apify)

```
import { Actor } from 'apify';

import { PlaywrightCrawler } from 'crawlee';



await Actor.init();



// Create an instance of the PlaywrightCrawler class - a crawler

// that automatically loads the URLs in headless Chrome / Playwright.

const crawler = new PlaywrightCrawler({

    launchContext: {

        // Here you can set options that are passed to the playwright .launch() function.

        launchOptions: {

            headless: true,

        },

    },



    // Stop crawling after several pages

    maxRequestsPerCrawl: 50,



    // This function will be called for each URL to crawl.

    // Here you can write the Playwright scripts you are familiar with,

    // with the exception that browsers and pages are automatically managed by the Apify SDK.

    // The function accepts a single parameter, which is an object with a lot of properties,

    // the most important being:

    // - request: an instance of the Request class with information such as URL and HTTP method

    // - page: Playwright's Page object (see https://playwright.dev/docs/api/class-page)

    async requestHandler({ request, page, enqueueLinks }) {

        console.log(`Processing ${request.url}...`);



        // A function to be evaluated by Playwright within the browser context.

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



        if (infos.processedRequests.length === 0)

            console.log(`${request.url} is the last page!`);

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
