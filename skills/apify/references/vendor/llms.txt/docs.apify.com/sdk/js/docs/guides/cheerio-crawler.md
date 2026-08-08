# Cheerio crawler

Copy for LLM

This example demonstrates how to use [`CheerioCrawler`](https://crawlee.dev/api/cheerio-crawler/class/CheerioCrawler) to crawl a list of URLs from an external file, load each URL using a plain HTTP request, parse the HTML using the [Cheerio library](https://www.npmjs.com/package/cheerio) and extract some data from it: the page title and all `h1` tags.

[Run on](https://console.apify.com/actors/kk67IcZkKSSBTslXI?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCB7IENoZWVyaW9DcmF3bGVyLCBsb2csIExvZ0xldmVsIH0gZnJvbSAnY3Jhd2xlZSc7XFxuXFxuLy8gQ3Jhd2xlcnMgY29tZSB3aXRoIHZhcmlvdXMgdXRpbGl0aWVzLCBlLmcuIGZvciBsb2dnaW5nLlxcbi8vIEhlcmUgd2UgdXNlIGRlYnVnIGxldmVsIG9mIGxvZ2dpbmcgdG8gaW1wcm92ZSB0aGUgZGVidWdnaW5nIGV4cGVyaWVuY2UuXFxuLy8gVGhpcyBmdW5jdGlvbmFsaXR5IGlzIG9wdGlvbmFsIVxcbmxvZy5zZXRMZXZlbChMb2dMZXZlbC5ERUJVRyk7XFxuXFxuLy8gQ3JlYXRlIGFuIGluc3RhbmNlIG9mIHRoZSBDaGVlcmlvQ3Jhd2xlciBjbGFzcyAtIGEgY3Jhd2xlclxcbi8vIHRoYXQgYXV0b21hdGljYWxseSBsb2FkcyB0aGUgVVJMcyBhbmQgcGFyc2VzIHRoZWlyIEhUTUwgdXNpbmcgdGhlIGNoZWVyaW8gbGlicmFyeS5cXG5jb25zdCBjcmF3bGVyID0gbmV3IENoZWVyaW9DcmF3bGVyKHtcXG4gICAgLy8gVGhlIGNyYXdsZXIgZG93bmxvYWRzIGFuZCBwcm9jZXNzZXMgdGhlIHdlYiBwYWdlcyBpbiBwYXJhbGxlbCwgd2l0aCBhIGNvbmN1cnJlbmN5XFxuICAgIC8vIGF1dG9tYXRpY2FsbHkgbWFuYWdlZCBiYXNlZCBvbiB0aGUgYXZhaWxhYmxlIHN5c3RlbSBtZW1vcnkgYW5kIENQVSAoc2VlIEF1dG9zY2FsZWRQb29sIGNsYXNzKS5cXG4gICAgLy8gSGVyZSB3ZSBkZWZpbmUgc29tZSBoYXJkIGxpbWl0cyBmb3IgdGhlIGNvbmN1cnJlbmN5LlxcbiAgICBtaW5Db25jdXJyZW5jeTogMTAsXFxuICAgIG1heENvbmN1cnJlbmN5OiA1MCxcXG5cXG4gICAgLy8gT24gZXJyb3IsIHJldHJ5IGVhY2ggcGFnZSBhdCBtb3N0IG9uY2UuXFxuICAgIG1heFJlcXVlc3RSZXRyaWVzOiAxLFxcblxcbiAgICAvLyBJbmNyZWFzZSB0aGUgdGltZW91dCBmb3IgcHJvY2Vzc2luZyBvZiBlYWNoIHBhZ2UuXFxuICAgIHJlcXVlc3RIYW5kbGVyVGltZW91dFNlY3M6IDMwLFxcblxcbiAgICAvLyBMaW1pdCB0byAxMCByZXF1ZXN0cyBwZXIgb25lIGNyYXdsXFxuICAgIG1heFJlcXVlc3RzUGVyQ3Jhd2w6IDEwLFxcblxcbiAgICAvLyBUaGlzIGZ1bmN0aW9uIHdpbGwgYmUgY2FsbGVkIGZvciBlYWNoIFVSTCB0byBjcmF3bC5cXG4gICAgLy8gSXQgYWNjZXB0cyBhIHNpbmdsZSBwYXJhbWV0ZXIsIHdoaWNoIGlzIGFuIG9iamVjdCB3aXRoIG9wdGlvbnMgYXM6XFxuICAgIC8vIGh0dHBzOi8vc2RrLmFwaWZ5LmNvbS9kb2NzL3R5cGVkZWZzL2NoZWVyaW8tY3Jhd2xlci1vcHRpb25zI2hhbmRsZXBhZ2VmdW5jdGlvblxcbiAgICAvLyBXZSB1c2UgZm9yIGRlbW9uc3RyYXRpb24gb25seSAyIG9mIHRoZW06XFxuICAgIC8vIC0gcmVxdWVzdDogYW4gaW5zdGFuY2Ugb2YgdGhlIFJlcXVlc3QgY2xhc3Mgd2l0aCBpbmZvcm1hdGlvbiBzdWNoIGFzIFVSTCBhbmQgSFRUUCBtZXRob2RcXG4gICAgLy8gLSAkOiB0aGUgY2hlZXJpbyBvYmplY3QgY29udGFpbmluZyBwYXJzZWQgSFRNTFxcbiAgICBhc3luYyByZXF1ZXN0SGFuZGxlcih7IHJlcXVlc3QsICQgfSkge1xcbiAgICAgICAgbG9nLmRlYnVnKGBQcm9jZXNzaW5nICR7cmVxdWVzdC51cmx9Li4uYCk7XFxuXFxuICAgICAgICAvLyBFeHRyYWN0IGRhdGEgZnJvbSB0aGUgcGFnZSB1c2luZyBjaGVlcmlvLlxcbiAgICAgICAgY29uc3QgdGl0bGUgPSAkKCd0aXRsZScpLnRleHQoKTtcXG4gICAgICAgIGNvbnN0IGgxdGV4dHMgPSBbXTtcXG4gICAgICAgICQoJ2gxJykuZWFjaCgoaW5kZXgsIGVsKSA9PiB7XFxuICAgICAgICAgICAgaDF0ZXh0cy5wdXNoKHtcXG4gICAgICAgICAgICAgICAgdGV4dDogJChlbCkudGV4dCgpLFxcbiAgICAgICAgICAgIH0pO1xcbiAgICAgICAgfSk7XFxuXFxuICAgICAgICAvLyBTdG9yZSB0aGUgcmVzdWx0cyB0byB0aGUgZGF0YXNldC4gSW4gbG9jYWwgY29uZmlndXJhdGlvbixcXG4gICAgICAgIC8vIHRoZSBkYXRhIHdpbGwgYmUgc3RvcmVkIGFzIEpTT04gZmlsZXMgaW4gLi9zdG9yYWdlL2RhdGFzZXRzL2RlZmF1bHRcXG4gICAgICAgIGF3YWl0IEFjdG9yLnB1c2hEYXRhKHtcXG4gICAgICAgICAgICB1cmw6IHJlcXVlc3QudXJsLFxcbiAgICAgICAgICAgIHRpdGxlLFxcbiAgICAgICAgICAgIGgxdGV4dHMsXFxuICAgICAgICB9KTtcXG4gICAgfSxcXG5cXG4gICAgLy8gVGhpcyBmdW5jdGlvbiBpcyBjYWxsZWQgaWYgdGhlIHBhZ2UgcHJvY2Vzc2luZyBmYWlsZWQgbW9yZSB0aGFuIG1heFJlcXVlc3RSZXRyaWVzKzEgdGltZXMuXFxuICAgIGZhaWxlZFJlcXVlc3RIYW5kbGVyKHsgcmVxdWVzdCB9KSB7XFxuICAgICAgICBsb2cuZGVidWcoYFJlcXVlc3QgJHtyZXF1ZXN0LnVybH0gZmFpbGVkIHR3aWNlLmApO1xcbiAgICB9LFxcbn0pO1xcblxcbi8vIFJ1biB0aGUgY3Jhd2xlciBhbmQgd2FpdCBmb3IgaXQgdG8gZmluaXNoLlxcbmF3YWl0IGNyYXdsZXIucnVuKCk7XFxuXFxubG9nLmRlYnVnKCdDcmF3bGVyIGZpbmlzaGVkLicpO1xcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.dOZILM56nUwSSVMoLgQB0brbbjQm2W2FDao35eLD72s\&asrc=run_on_apify)

```
import { CheerioCrawler, log, LogLevel } from 'crawlee';



// Crawlers come with various utilities, e.g. for logging.

// Here we use debug level of logging to improve the debugging experience.

// This functionality is optional!

log.setLevel(LogLevel.DEBUG);



// Create an instance of the CheerioCrawler class - a crawler

// that automatically loads the URLs and parses their HTML using the cheerio library.

const crawler = new CheerioCrawler({

    // The crawler downloads and processes the web pages in parallel, with a concurrency

    // automatically managed based on the available system memory and CPU (see AutoscaledPool class).

    // Here we define some hard limits for the concurrency.

    minConcurrency: 10,

    maxConcurrency: 50,



    // On error, retry each page at most once.

    maxRequestRetries: 1,



    // Increase the timeout for processing of each page.

    requestHandlerTimeoutSecs: 30,



    // Limit to 10 requests per one crawl

    maxRequestsPerCrawl: 10,



    // This function will be called for each URL to crawl.

    // It accepts a single parameter, which is an object with options as:

    // https://sdk.apify.com/docs/typedefs/cheerio-crawler-options#handlepagefunction

    // We use for demonstration only 2 of them:

    // - request: an instance of the Request class with information such as URL and HTTP method

    // - $: the cheerio object containing parsed HTML

    async requestHandler({ request, $ }) {

        log.debug(`Processing ${request.url}...`);



        // Extract data from the page using cheerio.

        const title = $('title').text();

        const h1texts = [];

        $('h1').each((index, el) => {

            h1texts.push({

                text: $(el).text(),

            });

        });



        // Store the results to the dataset. In local configuration,

        // the data will be stored as JSON files in ./storage/datasets/default

        await Actor.pushData({

            url: request.url,

            title,

            h1texts,

        });

    },



    // This function is called if the page processing failed more than maxRequestRetries+1 times.

    failedRequestHandler({ request }) {

        log.debug(`Request ${request.url} failed twice.`);

    },

});



// Run the crawler and wait for it to finish.

await crawler.run();



log.debug('Crawler finished.');
```
