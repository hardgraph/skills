# Building crawlers with Crawlee

Copy for LLM

In this guide, you'll learn how to build web crawlers with the [Crawlee](https://crawlee.dev/python) library in your Apify Actors.

## Introduction[](#introduction)

[Crawlee](https://crawlee.dev/python) is a Python library for web scraping and browser automation that provides a robust and flexible framework for building web scraping tasks. It seamlessly integrates with the Apify platform and supports a variety of scraping techniques, from static HTML parsing to dynamic JavaScript-rendered content handling. Crawlee offers a range of crawlers, including HTTP-based crawlers like [`HttpCrawler`](https://crawlee.dev/python/api/class/HttpCrawler), [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler) and [`ParselCrawler`](https://crawlee.dev/python/api/class/ParselCrawler), and browser-based crawlers like [`PlaywrightCrawler`](https://crawlee.dev/python/api/class/PlaywrightCrawler), to suit different scraping needs.

In this guide, you'll learn how to use Crawlee with [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler), [`ParselCrawler`](https://crawlee.dev/python/api/class/ParselCrawler), and [`PlaywrightCrawler`](https://crawlee.dev/python/api/class/PlaywrightCrawler) to build Apify Actors for web scraping.

## Actor with BeautifulSoupCrawler[](#actor-with-beautifulsoupcrawler)

The [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler) is ideal for extracting data from static HTML pages. It uses [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) for parsing and [`ImpitHttpClient`](https://crawlee.dev/python/api/class/ImpitHttpClient) for HTTP communication, ensuring efficient and lightweight scraping. If you do not need to execute JavaScript on the page, [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler) is a great choice for your scraping tasks. The following example shows how to use it in an Apify Actor.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBjcmF3bGVlLmNyYXdsZXJzIGltcG9ydCBCZWF1dGlmdWxTb3VwQ3Jhd2xlciwgQmVhdXRpZnVsU291cENyYXdsaW5nQ29udGV4dFxcbmZyb20gY3Jhd2xlZS5yb3V0ZXIgaW1wb3J0IFJvdXRlclxcblxcbmZyb20gYXBpZnkgaW1wb3J0IEFjdG9yXFxuXFxuIyBEZWZpbmUgdGhlIHJvdXRlciB1cCBmcm9udC4gVGhlIGNyYXdsZXIgaXMgY3JlYXRlZCBsYXRlciBpbiBgbWFpbmAuXFxucm91dGVyID0gUm91dGVyW0JlYXV0aWZ1bFNvdXBDcmF3bGluZ0NvbnRleHRdKClcXG5cXG5cXG4jIEhhbmRsZXIgY2FsbGVkIGZvciBldmVyeSByZXF1ZXN0LlxcbkByb3V0ZXIuZGVmYXVsdF9oYW5kbGVyXFxuYXN5bmMgZGVmIHJlcXVlc3RfaGFuZGxlcihjb250ZXh0OiBCZWF1dGlmdWxTb3VwQ3Jhd2xpbmdDb250ZXh0KSAtPiBOb25lOlxcbiAgICBBY3Rvci5sb2cuaW5mbyhmJ1NjcmFwaW5nIHtjb250ZXh0LnJlcXVlc3QudXJsfSAuLi4nKVxcblxcbiAgICBkYXRhID0ge1xcbiAgICAgICAgJ3VybCc6IGNvbnRleHQucmVxdWVzdC51cmwsXFxuICAgICAgICAndGl0bGUnOiBjb250ZXh0LnNvdXAudGl0bGUuc3RyaW5nIGlmIGNvbnRleHQuc291cC50aXRsZSBlbHNlIE5vbmUsXFxuICAgICAgICAnaDFzJzogW2gxLnRleHQgZm9yIGgxIGluIGNvbnRleHQuc291cC5maW5kX2FsbCgnaDEnKV0sXFxuICAgICAgICAnaDJzJzogW2gyLnRleHQgZm9yIGgyIGluIGNvbnRleHQuc291cC5maW5kX2FsbCgnaDInKV0sXFxuICAgICAgICAnaDNzJzogW2gzLnRleHQgZm9yIGgzIGluIGNvbnRleHQuc291cC5maW5kX2FsbCgnaDMnKV0sXFxuICAgIH1cXG5cXG4gICAgYXdhaXQgY29udGV4dC5wdXNoX2RhdGEoZGF0YSlcXG4gICAgQWN0b3IubG9nLmluZm8oZidTdG9yZWQgZGF0YSBmcm9tIHtjb250ZXh0LnJlcXVlc3QudXJsfSAodGl0bGU9e2RhdGFbXFxcInRpdGxlXFxcIl0hcn0pLicpXFxuXFxuICAgICMgRW5xdWV1ZSBsaW5rcyBmb3VuZCBvbiB0aGUgcGFnZS5cXG4gICAgYXdhaXQgY29udGV4dC5lbnF1ZXVlX2xpbmtzKHN0cmF0ZWd5PSdzYW1lLWRvbWFpbicpXFxuXFxuXFxuYXN5bmMgZGVmIG1haW4oKSAtPiBOb25lOlxcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgIyBSZWFkIHRoZSBBY3RvciBpbnB1dC5cXG4gICAgICAgIGFjdG9yX2lucHV0ID0gYXdhaXQgQWN0b3IuZ2V0X2lucHV0KCkgb3Ige31cXG4gICAgICAgIHN0YXJ0X3VybHMgPSBbXFxuICAgICAgICAgICAgdXJsLmdldCgndXJsJylcXG4gICAgICAgICAgICBmb3IgdXJsIGluIGFjdG9yX2lucHV0LmdldCgnc3RhcnRVcmxzJywgW3sndXJsJzogJ2h0dHBzOi8vY3Jhd2xlZS5kZXYnfV0pXFxuICAgICAgICBdXFxuXFxuICAgICAgICBpZiBub3Qgc3RhcnRfdXJsczpcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbygnTm8gc3RhcnQgVVJMcyBzcGVjaWZpZWQgaW4gQWN0b3IgaW5wdXQsIGV4aXRpbmcuLi4nKVxcbiAgICAgICAgICAgIGF3YWl0IEFjdG9yLmV4aXQoKVxcblxcbiAgICAgICAgIyBDcmF3bGVlIHJvdGF0ZXMgdGhlIHByb3h5IFVSTCBwZXIgcmVxdWVzdCBvbiBpdHMgb3duLlxcbiAgICAgICAgcHJveHlfY29uZmlndXJhdGlvbiA9IGF3YWl0IEFjdG9yLmNyZWF0ZV9wcm94eV9jb25maWd1cmF0aW9uKClcXG4gICAgICAgIGlmIHByb3h5X2NvbmZpZ3VyYXRpb24gaXMgTm9uZTpcXG4gICAgICAgICAgICByYWlzZSBSdW50aW1lRXJyb3IoJ0ZhaWxlZCB0byBjcmVhdGUgdGhlIHByb3h5IGNvbmZpZ3VyYXRpb24uJylcXG5cXG4gICAgICAgIGNyYXdsZXIgPSBCZWF1dGlmdWxTb3VwQ3Jhd2xlcihcXG4gICAgICAgICAgICBwcm94eV9jb25maWd1cmF0aW9uPXByb3h5X2NvbmZpZ3VyYXRpb24sXFxuICAgICAgICAgICAgcmVxdWVzdF9oYW5kbGVyPXJvdXRlcixcXG4gICAgICAgICAgICAjIENhcCB0aGUgY3Jhd2wuIFJlbW92ZSBvciBpbmNyZWFzZSB0aGUgbGltaXQgdG8gZm9sbG93IGFsbCBsaW5rcy5cXG4gICAgICAgICAgICBtYXhfcmVxdWVzdHNfcGVyX2NyYXdsPTEwLFxcbiAgICAgICAgKVxcblxcbiAgICAgICAgYXdhaXQgY3Jhd2xlci5ydW4oc3RhcnRfdXJscylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.5FEWeUDG2Y_-7foS7zloSUMz3bGA6Zy3TFDQ0bB_kwc\&asrc=run_on_apify)

```
import asyncio



from crawlee.crawlers import BeautifulSoupCrawler, BeautifulSoupCrawlingContext

from crawlee.router import Router



from apify import Actor



# Define the router up front. The crawler is created later in `main`.

router = Router[BeautifulSoupCrawlingContext]()





# Handler called for every request.

@router.default_handler

async def request_handler(context: BeautifulSoupCrawlingContext) -> None:

    Actor.log.info(f'Scraping {context.request.url} ...')



    data = {

        'url': context.request.url,

        'title': context.soup.title.string if context.soup.title else None,

        'h1s': [h1.text for h1 in context.soup.find_all('h1')],

        'h2s': [h2.text for h2 in context.soup.find_all('h2')],

        'h3s': [h3.text for h3 in context.soup.find_all('h3')],

    }



    await context.push_data(data)

    Actor.log.info(f'Stored data from {context.request.url} (title={data["title"]!r}).')



    # Enqueue links found on the page.

    await context.enqueue_links(strategy='same-domain')





async def main() -> None:

    async with Actor:

        # Read the Actor input.

        actor_input = await Actor.get_input() or {}

        start_urls = [

            url.get('url')

            for url in actor_input.get('startUrls', [{'url': 'https://crawlee.dev'}])

        ]



        if not start_urls:

            Actor.log.info('No start URLs specified in Actor input, exiting...')

            await Actor.exit()



        # Crawlee rotates the proxy URL per request on its own.

        proxy_configuration = await Actor.create_proxy_configuration()

        if proxy_configuration is None:

            raise RuntimeError('Failed to create the proxy configuration.')



        crawler = BeautifulSoupCrawler(

            proxy_configuration=proxy_configuration,

            request_handler=router,

            # Cap the crawl. Remove or increase the limit to follow all links.

            max_requests_per_crawl=10,

        )



        await crawler.run(start_urls)





if __name__ == '__main__':

    asyncio.run(main())
```

## Actor with ParselCrawler[](#actor-with-parselcrawler)

The [`ParselCrawler`](https://crawlee.dev/python/api/class/ParselCrawler) works in the same way as [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler), but it uses the [Parsel](https://parsel.readthedocs.io/en/latest/) library for HTML parsing. This allows for more powerful and flexible data extraction using [XPath](https://en.wikipedia.org/wiki/XPath) selectors. It should be faster than [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler). The following example shows how to use [`ParselCrawler`](https://crawlee.dev/python/api/class/ParselCrawler) in an Apify Actor.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBjcmF3bGVlLmNyYXdsZXJzIGltcG9ydCBQYXJzZWxDcmF3bGVyLCBQYXJzZWxDcmF3bGluZ0NvbnRleHRcXG5mcm9tIGNyYXdsZWUucm91dGVyIGltcG9ydCBSb3V0ZXJcXG5cXG5mcm9tIGFwaWZ5IGltcG9ydCBBY3RvclxcblxcbiMgRGVmaW5lIHRoZSByb3V0ZXIgdXAgZnJvbnQuIFRoZSBjcmF3bGVyIGlzIGNyZWF0ZWQgbGF0ZXIgaW4gYG1haW5gLlxcbnJvdXRlciA9IFJvdXRlcltQYXJzZWxDcmF3bGluZ0NvbnRleHRdKClcXG5cXG5cXG4jIEhhbmRsZXIgY2FsbGVkIGZvciBldmVyeSByZXF1ZXN0LlxcbkByb3V0ZXIuZGVmYXVsdF9oYW5kbGVyXFxuYXN5bmMgZGVmIHJlcXVlc3RfaGFuZGxlcihjb250ZXh0OiBQYXJzZWxDcmF3bGluZ0NvbnRleHQpIC0-IE5vbmU6XFxuICAgIEFjdG9yLmxvZy5pbmZvKGYnU2NyYXBpbmcge2NvbnRleHQucmVxdWVzdC51cmx9IC4uLicpXFxuXFxuICAgIGRhdGEgPSB7XFxuICAgICAgICAndXJsJzogY29udGV4dC5yZXF1ZXN0LnVybCxcXG4gICAgICAgICd0aXRsZSc6IGNvbnRleHQuc2VsZWN0b3IueHBhdGgoJy8vdGl0bGUvdGV4dCgpJykuZ2V0KCksXFxuICAgICAgICAnaDFzJzogY29udGV4dC5zZWxlY3Rvci54cGF0aCgnLy9oMS90ZXh0KCknKS5nZXRhbGwoKSxcXG4gICAgICAgICdoMnMnOiBjb250ZXh0LnNlbGVjdG9yLnhwYXRoKCcvL2gyL3RleHQoKScpLmdldGFsbCgpLFxcbiAgICAgICAgJ2gzcyc6IGNvbnRleHQuc2VsZWN0b3IueHBhdGgoJy8vaDMvdGV4dCgpJykuZ2V0YWxsKCksXFxuICAgIH1cXG5cXG4gICAgYXdhaXQgY29udGV4dC5wdXNoX2RhdGEoZGF0YSlcXG4gICAgQWN0b3IubG9nLmluZm8oZidTdG9yZWQgZGF0YSBmcm9tIHtjb250ZXh0LnJlcXVlc3QudXJsfSAodGl0bGU9e2RhdGFbXFxcInRpdGxlXFxcIl0hcn0pLicpXFxuXFxuICAgICMgRW5xdWV1ZSBsaW5rcyBmb3VuZCBvbiB0aGUgcGFnZS5cXG4gICAgYXdhaXQgY29udGV4dC5lbnF1ZXVlX2xpbmtzKHN0cmF0ZWd5PSdzYW1lLWRvbWFpbicpXFxuXFxuXFxuYXN5bmMgZGVmIG1haW4oKSAtPiBOb25lOlxcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgIyBSZWFkIHRoZSBBY3RvciBpbnB1dC5cXG4gICAgICAgIGFjdG9yX2lucHV0ID0gYXdhaXQgQWN0b3IuZ2V0X2lucHV0KCkgb3Ige31cXG4gICAgICAgIHN0YXJ0X3VybHMgPSBbXFxuICAgICAgICAgICAgdXJsLmdldCgndXJsJylcXG4gICAgICAgICAgICBmb3IgdXJsIGluIGFjdG9yX2lucHV0LmdldCgnc3RhcnRVcmxzJywgW3sndXJsJzogJ2h0dHBzOi8vY3Jhd2xlZS5kZXYnfV0pXFxuICAgICAgICBdXFxuXFxuICAgICAgICBpZiBub3Qgc3RhcnRfdXJsczpcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbygnTm8gc3RhcnQgVVJMcyBzcGVjaWZpZWQgaW4gQWN0b3IgaW5wdXQsIGV4aXRpbmcuLi4nKVxcbiAgICAgICAgICAgIGF3YWl0IEFjdG9yLmV4aXQoKVxcblxcbiAgICAgICAgIyBDcmF3bGVlIHJvdGF0ZXMgdGhlIHByb3h5IFVSTCBwZXIgcmVxdWVzdCBvbiBpdHMgb3duLlxcbiAgICAgICAgcHJveHlfY29uZmlndXJhdGlvbiA9IGF3YWl0IEFjdG9yLmNyZWF0ZV9wcm94eV9jb25maWd1cmF0aW9uKClcXG4gICAgICAgIGlmIHByb3h5X2NvbmZpZ3VyYXRpb24gaXMgTm9uZTpcXG4gICAgICAgICAgICByYWlzZSBSdW50aW1lRXJyb3IoJ0ZhaWxlZCB0byBjcmVhdGUgdGhlIHByb3h5IGNvbmZpZ3VyYXRpb24uJylcXG5cXG4gICAgICAgIGNyYXdsZXIgPSBQYXJzZWxDcmF3bGVyKFxcbiAgICAgICAgICAgIHByb3h5X2NvbmZpZ3VyYXRpb249cHJveHlfY29uZmlndXJhdGlvbixcXG4gICAgICAgICAgICByZXF1ZXN0X2hhbmRsZXI9cm91dGVyLFxcbiAgICAgICAgICAgICMgQ2FwIHRoZSBjcmF3bC4gUmVtb3ZlIG9yIGluY3JlYXNlIHRoZSBsaW1pdCB0byBmb2xsb3cgYWxsIGxpbmtzLlxcbiAgICAgICAgICAgIG1heF9yZXF1ZXN0c19wZXJfY3Jhd2w9MTAsXFxuICAgICAgICApXFxuXFxuICAgICAgICBhd2FpdCBjcmF3bGVyLnJ1bihzdGFydF91cmxzKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.5u1v9v-WcqcIhTBuKGI1jfb9PfI6ZdwrKih0WUB5dyc\&asrc=run_on_apify)

```
import asyncio



from crawlee.crawlers import ParselCrawler, ParselCrawlingContext

from crawlee.router import Router



from apify import Actor



# Define the router up front. The crawler is created later in `main`.

router = Router[ParselCrawlingContext]()





# Handler called for every request.

@router.default_handler

async def request_handler(context: ParselCrawlingContext) -> None:

    Actor.log.info(f'Scraping {context.request.url} ...')



    data = {

        'url': context.request.url,

        'title': context.selector.xpath('//title/text()').get(),

        'h1s': context.selector.xpath('//h1/text()').getall(),

        'h2s': context.selector.xpath('//h2/text()').getall(),

        'h3s': context.selector.xpath('//h3/text()').getall(),

    }



    await context.push_data(data)

    Actor.log.info(f'Stored data from {context.request.url} (title={data["title"]!r}).')



    # Enqueue links found on the page.

    await context.enqueue_links(strategy='same-domain')





async def main() -> None:

    async with Actor:

        # Read the Actor input.

        actor_input = await Actor.get_input() or {}

        start_urls = [

            url.get('url')

            for url in actor_input.get('startUrls', [{'url': 'https://crawlee.dev'}])

        ]



        if not start_urls:

            Actor.log.info('No start URLs specified in Actor input, exiting...')

            await Actor.exit()



        # Crawlee rotates the proxy URL per request on its own.

        proxy_configuration = await Actor.create_proxy_configuration()

        if proxy_configuration is None:

            raise RuntimeError('Failed to create the proxy configuration.')



        crawler = ParselCrawler(

            proxy_configuration=proxy_configuration,

            request_handler=router,

            # Cap the crawl. Remove or increase the limit to follow all links.

            max_requests_per_crawl=10,

        )



        await crawler.run(start_urls)





if __name__ == '__main__':

    asyncio.run(main())
```

## Actor with PlaywrightCrawler[](#actor-with-playwrightcrawler)

The [`PlaywrightCrawler`](https://crawlee.dev/python/api/class/PlaywrightCrawler) is built for handling dynamic web pages that rely on JavaScript for content rendering. Using the [Playwright](https://playwright.dev/) library, it provides a browser-based automation environment to interact with complex websites. The following example shows how to use [`PlaywrightCrawler`](https://crawlee.dev/python/api/class/PlaywrightCrawler) in an Apify Actor.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBjcmF3bGVlLmNyYXdsZXJzIGltcG9ydCBQbGF5d3JpZ2h0Q3Jhd2xlciwgUGxheXdyaWdodENyYXdsaW5nQ29udGV4dFxcbmZyb20gY3Jhd2xlZS5yb3V0ZXIgaW1wb3J0IFJvdXRlclxcblxcbmZyb20gYXBpZnkgaW1wb3J0IEFjdG9yXFxuXFxuIyBEZWZpbmUgdGhlIHJvdXRlciB1cCBmcm9udC4gVGhlIGNyYXdsZXIgaXMgY3JlYXRlZCBsYXRlciBpbiBgbWFpbmAuXFxucm91dGVyID0gUm91dGVyW1BsYXl3cmlnaHRDcmF3bGluZ0NvbnRleHRdKClcXG5cXG5cXG4jIEhhbmRsZXIgY2FsbGVkIGZvciBldmVyeSByZXF1ZXN0LlxcbkByb3V0ZXIuZGVmYXVsdF9oYW5kbGVyXFxuYXN5bmMgZGVmIHJlcXVlc3RfaGFuZGxlcihjb250ZXh0OiBQbGF5d3JpZ2h0Q3Jhd2xpbmdDb250ZXh0KSAtPiBOb25lOlxcbiAgICBBY3Rvci5sb2cuaW5mbyhmJ1NjcmFwaW5nIHtjb250ZXh0LnJlcXVlc3QudXJsfSAuLi4nKVxcblxcbiAgICBkYXRhID0ge1xcbiAgICAgICAgJ3VybCc6IGNvbnRleHQucmVxdWVzdC51cmwsXFxuICAgICAgICAndGl0bGUnOiBhd2FpdCBjb250ZXh0LnBhZ2UudGl0bGUoKSxcXG4gICAgICAgICdoMXMnOiBbYXdhaXQgaDEudGV4dF9jb250ZW50KCkgZm9yIGgxIGluIGF3YWl0IGNvbnRleHQucGFnZS5sb2NhdG9yKCdoMScpLmFsbCgpXSxcXG4gICAgICAgICdoMnMnOiBbYXdhaXQgaDIudGV4dF9jb250ZW50KCkgZm9yIGgyIGluIGF3YWl0IGNvbnRleHQucGFnZS5sb2NhdG9yKCdoMicpLmFsbCgpXSxcXG4gICAgICAgICdoM3MnOiBbYXdhaXQgaDMudGV4dF9jb250ZW50KCkgZm9yIGgzIGluIGF3YWl0IGNvbnRleHQucGFnZS5sb2NhdG9yKCdoMycpLmFsbCgpXSxcXG4gICAgfVxcblxcbiAgICBhd2FpdCBjb250ZXh0LnB1c2hfZGF0YShkYXRhKVxcbiAgICBBY3Rvci5sb2cuaW5mbyhmJ1N0b3JlZCBkYXRhIGZyb20ge2NvbnRleHQucmVxdWVzdC51cmx9ICh0aXRsZT17ZGF0YVtcXFwidGl0bGVcXFwiXSFyfSkuJylcXG5cXG4gICAgIyBFbnF1ZXVlIGxpbmtzIGZvdW5kIG9uIHRoZSBwYWdlLlxcbiAgICBhd2FpdCBjb250ZXh0LmVucXVldWVfbGlua3Moc3RyYXRlZ3k9J3NhbWUtZG9tYWluJylcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFJlYWQgdGhlIEFjdG9yIGlucHV0LlxcbiAgICAgICAgYWN0b3JfaW5wdXQgPSBhd2FpdCBBY3Rvci5nZXRfaW5wdXQoKSBvciB7fVxcbiAgICAgICAgc3RhcnRfdXJscyA9IFtcXG4gICAgICAgICAgICB1cmwuZ2V0KCd1cmwnKVxcbiAgICAgICAgICAgIGZvciB1cmwgaW4gYWN0b3JfaW5wdXQuZ2V0KCdzdGFydFVybHMnLCBbeyd1cmwnOiAnaHR0cHM6Ly9jcmF3bGVlLmRldid9XSlcXG4gICAgICAgIF1cXG5cXG4gICAgICAgIGlmIG5vdCBzdGFydF91cmxzOlxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKCdObyBzdGFydCBVUkxzIHNwZWNpZmllZCBpbiBBY3RvciBpbnB1dCwgZXhpdGluZy4uLicpXFxuICAgICAgICAgICAgYXdhaXQgQWN0b3IuZXhpdCgpXFxuXFxuICAgICAgICAjIENyYXdsZWUgcm90YXRlcyB0aGUgcHJveHkgVVJMIHBlciByZXF1ZXN0IG9uIGl0cyBvd24uXFxuICAgICAgICBwcm94eV9jb25maWd1cmF0aW9uID0gYXdhaXQgQWN0b3IuY3JlYXRlX3Byb3h5X2NvbmZpZ3VyYXRpb24oKVxcbiAgICAgICAgaWYgcHJveHlfY29uZmlndXJhdGlvbiBpcyBOb25lOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignRmFpbGVkIHRvIGNyZWF0ZSB0aGUgcHJveHkgY29uZmlndXJhdGlvbi4nKVxcblxcbiAgICAgICAgIyBDb21tb24gQ2hyb21lIGZsYWdzIGZvciBydW5uaW5nIHRoZSBicm93c2VyIGluIGEgY29udGFpbmVyLlxcbiAgICAgICAgYnJvd3Nlcl9hcmdzID0gWyctLW5vLXNhbmRib3gnLCAnLS1kaXNhYmxlLWRldi1zaG0tdXNhZ2UnLCAnLS1kaXNhYmxlLWdwdSddXFxuXFxuICAgICAgICBjcmF3bGVyID0gUGxheXdyaWdodENyYXdsZXIoXFxuICAgICAgICAgICAgcHJveHlfY29uZmlndXJhdGlvbj1wcm94eV9jb25maWd1cmF0aW9uLFxcbiAgICAgICAgICAgIHJlcXVlc3RfaGFuZGxlcj1yb3V0ZXIsXFxuICAgICAgICAgICAgIyBDYXAgdGhlIGNyYXdsLiBSZW1vdmUgb3IgaW5jcmVhc2UgdGhlIGxpbWl0IHRvIGZvbGxvdyBhbGwgbGlua3MuXFxuICAgICAgICAgICAgbWF4X3JlcXVlc3RzX3Blcl9jcmF3bD0xMCxcXG4gICAgICAgICAgICBoZWFkbGVzcz1UcnVlLFxcbiAgICAgICAgICAgIGJyb3dzZXJfbGF1bmNoX29wdGlvbnM9eydhcmdzJzogYnJvd3Nlcl9hcmdzfSxcXG4gICAgICAgIClcXG5cXG4gICAgICAgIGF3YWl0IGNyYXdsZXIucnVuKHN0YXJ0X3VybHMpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjQwOTYsInRpbWVvdXQiOjE4MH19.VOKgTeIPcmsIwWbG4-ih_r4LlT69R7ezib3IyG0U5Ww\&asrc=run_on_apify)

```
import asyncio



from crawlee.crawlers import PlaywrightCrawler, PlaywrightCrawlingContext

from crawlee.router import Router



from apify import Actor



# Define the router up front. The crawler is created later in `main`.

router = Router[PlaywrightCrawlingContext]()





# Handler called for every request.

@router.default_handler

async def request_handler(context: PlaywrightCrawlingContext) -> None:

    Actor.log.info(f'Scraping {context.request.url} ...')



    data = {

        'url': context.request.url,

        'title': await context.page.title(),

        'h1s': [await h1.text_content() for h1 in await context.page.locator('h1').all()],

        'h2s': [await h2.text_content() for h2 in await context.page.locator('h2').all()],

        'h3s': [await h3.text_content() for h3 in await context.page.locator('h3').all()],

    }



    await context.push_data(data)

    Actor.log.info(f'Stored data from {context.request.url} (title={data["title"]!r}).')



    # Enqueue links found on the page.

    await context.enqueue_links(strategy='same-domain')





async def main() -> None:

    async with Actor:

        # Read the Actor input.

        actor_input = await Actor.get_input() or {}

        start_urls = [

            url.get('url')

            for url in actor_input.get('startUrls', [{'url': 'https://crawlee.dev'}])

        ]



        if not start_urls:

            Actor.log.info('No start URLs specified in Actor input, exiting...')

            await Actor.exit()



        # Crawlee rotates the proxy URL per request on its own.

        proxy_configuration = await Actor.create_proxy_configuration()

        if proxy_configuration is None:

            raise RuntimeError('Failed to create the proxy configuration.')



        # Common Chrome flags for running the browser in a container.

        browser_args = ['--no-sandbox', '--disable-dev-shm-usage', '--disable-gpu']



        crawler = PlaywrightCrawler(

            proxy_configuration=proxy_configuration,

            request_handler=router,

            # Cap the crawl. Remove or increase the limit to follow all links.

            max_requests_per_crawl=10,

            headless=True,

            browser_launch_options={'args': browser_args},

        )



        await crawler.run(start_urls)





if __name__ == '__main__':

    asyncio.run(main())
```

## Using Apify Proxy[](#using-apify-proxy)

All three crawlers above route their requests through [Apify Proxy](https://docs.apify.com/platform/proxy), which rotates IP addresses to avoid rate limiting and blocking. `Actor.create_proxy_configuration` returns a Crawlee-compatible proxy configuration, which is passed to the crawler as `proxy_configuration`. Crawlee then rotates the proxy IP for every request on its own. Because the configuration is only available inside the running Actor, the crawler is created in `main` and the request handler is registered on a standalone [`Router`](https://crawlee.dev/python/api/class/Router) up front. To select specific proxy groups or a country, pass the relevant arguments to `Actor.create_proxy_configuration`. For details, see [Proxy management](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/proxy-management.md).

## Conclusion[](#conclusion)

In this guide, you learned how to use the [Crawlee](https://crawlee.dev/python) library in your Apify Actors. By using the [`BeautifulSoupCrawler`](https://crawlee.dev/python/api/class/BeautifulSoupCrawler), [`ParselCrawler`](https://crawlee.dev/python/api/class/ParselCrawler), and [`PlaywrightCrawler`](https://crawlee.dev/python/api/class/PlaywrightCrawler) crawlers, you can efficiently scrape static or dynamic web pages, making it easy to build web scraping tasks in Python. See the [Actor templates](https://apify.com/templates/categories/python) to get started with your own scraping tasks. If you have questions or need assistance, feel free to reach out on our [GitHub](https://github.com/apify/apify-sdk-python) or join our [Discord community](https://discord.com/invite/jyEM2PRvMU). Happy scraping!

## Additional resources[](#additional-resources)

* [Apify templates: Crawlee + BeautifulSoup](https://apify.com/templates/python-crawlee-beautifulsoup)
* [Apify templates: Crawlee + Parsel](https://apify.com/templates/python-crawlee-parsel)
* [Apify templates: Crawlee + Playwright + Chrome](https://apify.com/templates/python-crawlee-playwright)
* [Crawlee: Official website](https://crawlee.dev/python)
* [Crawlee: Documentation](https://crawlee.dev/python/docs)
* [Crawlee: GitHub repository](https://github.com/apify/crawlee-python)
