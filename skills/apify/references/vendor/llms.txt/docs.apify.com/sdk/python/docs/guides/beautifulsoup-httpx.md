# Scraping with BeautifulSoup and HTTPX

Copy for LLM

In this guide, you'll learn how to scrape web pages with the [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) and [HTTPX](https://www.python-httpx.org/) libraries in your Apify Actors.

## Introduction[](#introduction)

[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) is a Python library for extracting data from HTML and XML files. It provides simple methods and Pythonic idioms for navigating, searching, and modifying a website's element tree, enabling efficient data extraction.

[HTTPX](https://www.python-httpx.org/) is a modern, high-level HTTP client library for Python. It provides a simple interface for making HTTP requests and supports both synchronous and asynchronous requests.

To create an Actor which uses those libraries, start from the [BeautifulSoup & Python](https://apify.com/templates/categories/python) Actor template. This template includes the [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) and [HTTPX](https://www.python-httpx.org/) libraries preinstalled, allowing you to begin development immediately.

## Example Actor[](#example-actor)

The following example is a simple Actor that recursively scrapes data from linked pages on the same site, up to a specified maximum depth, starting from URLs provided in the Actor input. It uses [HTTPX](https://www.python-httpx.org/) for fetching pages through [Apify Proxy](https://docs.apify.com/platform/proxy) and [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) for parsing their content to extract the title, headings, and links to other pages.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSB0eXBpbmcgaW1wb3J0IEFueVxcbmZyb20gdXJsbGliLnBhcnNlIGltcG9ydCB1cmxqb2luLCB1cmxzcGxpdFxcblxcbmltcG9ydCBodHRweFxcbmZyb20gYnM0IGltcG9ydCBCZWF1dGlmdWxTb3VwXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFJlcXVlc3RcXG5mcm9tIGFwaWZ5LnN0b3JhZ2VzIGltcG9ydCBSZXF1ZXN0UXVldWVcXG5cXG5cXG5hc3luYyBkZWYgc2NyYXBlX3BhZ2UoXFxuICAgIHVybDogc3RyLFxcbiAgICAqLFxcbiAgICBwcm94eV91cmw6IHN0ciB8IE5vbmUgPSBOb25lLFxcbikgLT4gdHVwbGVbZGljdFtzdHIsIEFueV0sIGxpc3Rbc3RyXV06XFxuICAgIFxcXCJcXFwiXFxcIkZldGNoIGEgcGFnZSB3aXRoIEhUVFBYIGFuZCByZXR1cm4gaXRzIGRhdGEgYW5kIHNhbWUtc2l0ZSBsaW5rcy5cXFwiXFxcIlxcXCJcXG4gICAgIyBBIGZyZXNoIGNsaWVudCBwZXIgY2FsbCBsZXRzIGVhY2ggcmVxdWVzdCB1c2UgYSBuZXcgcHJveHkgVVJMLlxcbiAgICBhc3luYyB3aXRoIGh0dHB4LkFzeW5jQ2xpZW50KHByb3h5PXByb3h5X3VybCkgYXMgY2xpZW50OlxcbiAgICAgICAgcmVzcG9uc2UgPSBhd2FpdCBjbGllbnQuZ2V0KHVybCwgZm9sbG93X3JlZGlyZWN0cz1UcnVlKVxcblxcbiAgICBzb3VwID0gQmVhdXRpZnVsU291cChyZXNwb25zZS5jb250ZW50LCAnaHRtbC5wYXJzZXInKVxcblxcbiAgICBkYXRhID0ge1xcbiAgICAgICAgJ3VybCc6IHVybCxcXG4gICAgICAgICd0aXRsZSc6IHNvdXAudGl0bGUuc3RyaW5nIGlmIHNvdXAudGl0bGUgZWxzZSBOb25lLFxcbiAgICAgICAgJ2gxcyc6IFtoMS50ZXh0IGZvciBoMSBpbiBzb3VwLmZpbmRfYWxsKCdoMScpXSxcXG4gICAgICAgICdoMnMnOiBbaDIudGV4dCBmb3IgaDIgaW4gc291cC5maW5kX2FsbCgnaDInKV0sXFxuICAgICAgICAnaDNzJzogW2gzLnRleHQgZm9yIGgzIGluIHNvdXAuZmluZF9hbGwoJ2gzJyldLFxcbiAgICB9XFxuXFxuICAgICMgS2VlcCBvbmx5IGFic29sdXRlIGxpbmtzIG9uIHRoZSBzYW1lIGhvc3QuXFxuICAgIGxpbmtzOiBsaXN0W3N0cl0gPSBbXVxcbiAgICBob3N0ID0gdXJsc3BsaXQodXJsKS5uZXRsb2NcXG4gICAgZm9yIGxpbmsgaW4gc291cC5maW5kX2FsbCgnYScpOlxcbiAgICAgICAgbGlua191cmwgPSB1cmxqb2luKHVybCwgbGluay5nZXQoJ2hyZWYnKSlcXG4gICAgICAgIGlmIG5vdCBsaW5rX3VybC5zdGFydHN3aXRoKCgnaHR0cDovLycsICdodHRwczovLycpKTpcXG4gICAgICAgICAgICBjb250aW51ZVxcbiAgICAgICAgaWYgdXJsc3BsaXQobGlua191cmwpLm5ldGxvYyA9PSBob3N0OlxcbiAgICAgICAgICAgIGxpbmtzLmFwcGVuZChsaW5rX3VybClcXG5cXG4gICAgcmV0dXJuIGRhdGEsIGxpbmtzXFxuXFxuXFxuYXN5bmMgZGVmIGVucXVldWVfbGlua3MoXFxuICAgIHJlcXVlc3RfcXVldWU6IFJlcXVlc3RRdWV1ZSxcXG4gICAgbGlua3M6IGxpc3Rbc3RyXSxcXG4gICAgKixcXG4gICAgZGVwdGg6IGludCxcXG4gICAgbWF4X2RlcHRoOiBpbnQsXFxuKSAtPiBOb25lOlxcbiAgICBcXFwiXFxcIlxcXCJFbnF1ZXVlIHRoZSBsaW5rcyBvbmUgbGV2ZWwgZGVlcGVyLCB1bmxlc3MgbWF4X2RlcHRoIHdhcyByZWFjaGVkLlxcXCJcXFwiXFxcIlxcbiAgICBpZiBkZXB0aCA-PSBtYXhfZGVwdGg6XFxuICAgICAgICByZXR1cm5cXG5cXG4gICAgZm9yIGxpbmtfdXJsIGluIGxpbmtzOlxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidFbnF1ZXVpbmcge2xpbmtfdXJsfSAuLi4nKVxcbiAgICAgICAgcmVxdWVzdCA9IFJlcXVlc3QuZnJvbV91cmwobGlua191cmwpXFxuICAgICAgICByZXF1ZXN0LmNyYXdsX2RlcHRoID0gZGVwdGggKyAxXFxuICAgICAgICBhd2FpdCByZXF1ZXN0X3F1ZXVlLmFkZF9yZXF1ZXN0KHJlcXVlc3QpXFxuXFxuXFxuYXN5bmMgZGVmIG1haW4oKSAtPiBOb25lOlxcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgIyBSZWFkIHRoZSBBY3RvciBpbnB1dC5cXG4gICAgICAgIGFjdG9yX2lucHV0ID0gYXdhaXQgQWN0b3IuZ2V0X2lucHV0KCkgb3Ige31cXG4gICAgICAgIHN0YXJ0X3VybHMgPSBhY3Rvcl9pbnB1dC5nZXQoJ3N0YXJ0VXJscycsIFt7J3VybCc6ICdodHRwczovL2NyYXdsZWUuZGV2J31dKVxcbiAgICAgICAgbWF4X2RlcHRoID0gYWN0b3JfaW5wdXQuZ2V0KCdtYXhEZXB0aCcsIDEpXFxuXFxuICAgICAgICBpZiBub3Qgc3RhcnRfdXJsczpcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbygnTm8gc3RhcnQgVVJMcyBzcGVjaWZpZWQgaW4gQWN0b3IgaW5wdXQsIGV4aXRpbmcuLi4nKVxcbiAgICAgICAgICAgIGF3YWl0IEFjdG9yLmV4aXQoKVxcblxcbiAgICAgICAgIyBTZXQgdXAgQXBpZnkgUHJveHkgYW5kIHRoZSByZXF1ZXN0IHF1ZXVlLlxcbiAgICAgICAgcHJveHlfY29uZmlndXJhdGlvbiA9IGF3YWl0IEFjdG9yLmNyZWF0ZV9wcm94eV9jb25maWd1cmF0aW9uKClcXG4gICAgICAgIHJlcXVlc3RfcXVldWUgPSBhd2FpdCBBY3Rvci5vcGVuX3JlcXVlc3RfcXVldWUoKVxcblxcbiAgICAgICAgIyBFbnF1ZXVlIHRoZSBzdGFydCBVUkxzIChjcmF3bCBkZXB0aCBkZWZhdWx0cyB0byAwKS5cXG4gICAgICAgIGZvciBzdGFydF91cmwgaW4gc3RhcnRfdXJsczpcXG4gICAgICAgICAgICB1cmwgPSBzdGFydF91cmwuZ2V0KCd1cmwnKVxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnRW5xdWV1aW5nIHN0YXJ0IFVSTDoge3VybH0nKVxcbiAgICAgICAgICAgIGF3YWl0IHJlcXVlc3RfcXVldWUuYWRkX3JlcXVlc3QoUmVxdWVzdC5mcm9tX3VybCh1cmwpKVxcblxcbiAgICAgICAgIyBDYXAgdGhlIGNyYXdsLiBSYWlzZSBvciByZW1vdmUgdGhlIGxpbWl0IHRvIGZvbGxvdyBtb3JlIHBhZ2VzLlxcbiAgICAgICAgbWF4X3JlcXVlc3RzID0gMTBcXG4gICAgICAgIGhhbmRsZWRfcmVxdWVzdHMgPSAwXFxuXFxuICAgICAgICB3aGlsZSBoYW5kbGVkX3JlcXVlc3RzIDwgbWF4X3JlcXVlc3RzIGFuZCAoXFxuICAgICAgICAgICAgcmVxdWVzdCA6PSBhd2FpdCByZXF1ZXN0X3F1ZXVlLmZldGNoX25leHRfcmVxdWVzdCgpXFxuICAgICAgICApOlxcbiAgICAgICAgICAgIGhhbmRsZWRfcmVxdWVzdHMgKz0gMVxcbiAgICAgICAgICAgIHVybCA9IHJlcXVlc3QudXJsXFxuICAgICAgICAgICAgZGVwdGggPSByZXF1ZXN0LmNyYXdsX2RlcHRoXFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oZidTY3JhcGluZyB7dXJsfSAoZGVwdGg9e2RlcHRofSkgLi4uJylcXG5cXG4gICAgICAgICAgICB0cnk6XFxuICAgICAgICAgICAgICAgICMgRnJlc2ggcHJveHkgVVJMIHBlciByZXF1ZXN0IChOb25lIGlmIG5vIHByb3h5KS5cXG4gICAgICAgICAgICAgICAgcHJveHlfdXJsID0gTm9uZVxcbiAgICAgICAgICAgICAgICBpZiBwcm94eV9jb25maWd1cmF0aW9uOlxcbiAgICAgICAgICAgICAgICAgICAgcHJveHlfdXJsID0gYXdhaXQgcHJveHlfY29uZmlndXJhdGlvbi5uZXdfdXJsKClcXG5cXG4gICAgICAgICAgICAgICAgZGF0YSwgbGlua3MgPSBhd2FpdCBzY3JhcGVfcGFnZSh1cmwsIHByb3h5X3VybD1wcm94eV91cmwpXFxuICAgICAgICAgICAgICAgIGF3YWl0IEFjdG9yLnB1c2hfZGF0YShkYXRhKVxcbiAgICAgICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhcXG4gICAgICAgICAgICAgICAgICAgIGYnU3RvcmVkIGRhdGEgZnJvbSB7dXJsfSAnXFxuICAgICAgICAgICAgICAgICAgICBmJyh0aXRsZT17ZGF0YVtcXFwidGl0bGVcXFwiXSFyfSwge2xlbihsaW5rcyl9IGxpbmtzIGZvdW5kKS4nXFxuICAgICAgICAgICAgICAgIClcXG4gICAgICAgICAgICAgICAgYXdhaXQgZW5xdWV1ZV9saW5rcyhcXG4gICAgICAgICAgICAgICAgICAgIHJlcXVlc3RfcXVldWUsIGxpbmtzLCBkZXB0aD1kZXB0aCwgbWF4X2RlcHRoPW1heF9kZXB0aFxcbiAgICAgICAgICAgICAgICApXFxuXFxuICAgICAgICAgICAgZXhjZXB0IEV4Y2VwdGlvbjpcXG4gICAgICAgICAgICAgICAgQWN0b3IubG9nLmV4Y2VwdGlvbihmJ0Nhbm5vdCBleHRyYWN0IGRhdGEgZnJvbSB7dXJsfS4nKVxcblxcbiAgICAgICAgICAgIGZpbmFsbHk6XFxuICAgICAgICAgICAgICAgIGF3YWl0IHJlcXVlc3RfcXVldWUubWFya19yZXF1ZXN0X2FzX2hhbmRsZWQocmVxdWVzdClcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.tFbT7uVrOyMi1ae0N4DM1ttvEQcCkYEeGgfEA2N3JjU\&asrc=run_on_apify)

```
import asyncio

from typing import Any

from urllib.parse import urljoin, urlsplit



import httpx

from bs4 import BeautifulSoup



from apify import Actor, Request

from apify.storages import RequestQueue





async def scrape_page(

    url: str,

    *,

    proxy_url: str | None = None,

) -> tuple[dict[str, Any], list[str]]:

    """Fetch a page with HTTPX and return its data and same-site links."""

    # A fresh client per call lets each request use a new proxy URL.

    async with httpx.AsyncClient(proxy=proxy_url) as client:

        response = await client.get(url, follow_redirects=True)



    soup = BeautifulSoup(response.content, 'html.parser')



    data = {

        'url': url,

        'title': soup.title.string if soup.title else None,

        'h1s': [h1.text for h1 in soup.find_all('h1')],

        'h2s': [h2.text for h2 in soup.find_all('h2')],

        'h3s': [h3.text for h3 in soup.find_all('h3')],

    }



    # Keep only absolute links on the same host.

    links: list[str] = []

    host = urlsplit(url).netloc

    for link in soup.find_all('a'):

        link_url = urljoin(url, link.get('href'))

        if not link_url.startswith(('http://', 'https://')):

            continue

        if urlsplit(link_url).netloc == host:

            links.append(link_url)



    return data, links





async def enqueue_links(

    request_queue: RequestQueue,

    links: list[str],

    *,

    depth: int,

    max_depth: int,

) -> None:

    """Enqueue the links one level deeper, unless max_depth was reached."""

    if depth >= max_depth:

        return



    for link_url in links:

        Actor.log.info(f'Enqueuing {link_url} ...')

        request = Request.from_url(link_url)

        request.crawl_depth = depth + 1

        await request_queue.add_request(request)





async def main() -> None:

    async with Actor:

        # Read the Actor input.

        actor_input = await Actor.get_input() or {}

        start_urls = actor_input.get('startUrls', [{'url': 'https://crawlee.dev'}])

        max_depth = actor_input.get('maxDepth', 1)



        if not start_urls:

            Actor.log.info('No start URLs specified in Actor input, exiting...')

            await Actor.exit()



        # Set up Apify Proxy and the request queue.

        proxy_configuration = await Actor.create_proxy_configuration()

        request_queue = await Actor.open_request_queue()



        # Enqueue the start URLs (crawl depth defaults to 0).

        for start_url in start_urls:

            url = start_url.get('url')

            Actor.log.info(f'Enqueuing start URL: {url}')

            await request_queue.add_request(Request.from_url(url))



        # Cap the crawl. Raise or remove the limit to follow more pages.

        max_requests = 10

        handled_requests = 0



        while handled_requests < max_requests and (

            request := await request_queue.fetch_next_request()

        ):

            handled_requests += 1

            url = request.url

            depth = request.crawl_depth

            Actor.log.info(f'Scraping {url} (depth={depth}) ...')



            try:

                # Fresh proxy URL per request (None if no proxy).

                proxy_url = None

                if proxy_configuration:

                    proxy_url = await proxy_configuration.new_url()



                data, links = await scrape_page(url, proxy_url=proxy_url)

                await Actor.push_data(data)

                Actor.log.info(

                    f'Stored data from {url} '

                    f'(title={data["title"]!r}, {len(links)} links found).'

                )

                await enqueue_links(

                    request_queue, links, depth=depth, max_depth=max_depth

                )



            except Exception:

                Actor.log.exception(f'Cannot extract data from {url}.')



            finally:

                await request_queue.mark_request_as_handled(request)





if __name__ == '__main__':

    asyncio.run(main())
```

## Using Apify Proxy[](#using-apify-proxy)

Running on the Apify platform gives your scraper access to [Apify Proxy](https://docs.apify.com/platform/proxy), which rotates IP addresses to avoid rate limiting and blocking. The example creates a proxy configuration with `Actor.create_proxy_configuration` and fetches a fresh proxy URL for every request. Each page then goes through a different IP. A new HTTPX client is created per request to apply that URL. To select specific proxy groups or a country, pass the relevant arguments to `Actor.create_proxy_configuration`. For details, see [Proxy management](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/proxy-management.md).

## Conclusion[](#conclusion)

In this guide, you learned how to use the [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/) with the [HTTPX](https://www.python-httpx.org/) in your Apify Actors. By combining these libraries, you can efficiently extract data from HTML or XML files, making it easy to build web scraping tasks in Python. See the [Actor templates](https://apify.com/templates/categories/python) to get started with your own scraping tasks. If you have questions or need assistance, feel free to reach out on our [GitHub](https://github.com/apify/apify-sdk-python) or join our [Discord community](https://discord.com/invite/jyEM2PRvMU). Happy scraping!

## Additional resources[](#additional-resources)

* [Apify templates: BeautifulSoup](https://apify.com/templates/python-beautifulsoup)
* [BeautifulSoup: Official documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
* [HTTPX: Official documentation](https://www.python-httpx.org/)
