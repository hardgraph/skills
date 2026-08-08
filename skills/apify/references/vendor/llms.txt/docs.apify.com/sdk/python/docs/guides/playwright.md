# Browser automation with Playwright

Copy for LLM

In this guide, you'll learn how to use [Playwright](https://playwright.dev) for browser automation and web scraping in your Apify Actors.

## Introduction[](#introduction)

[Playwright](https://playwright.dev) is a tool for web automation and testing that can also be used for web scraping. It allows you to control a web browser programmatically and interact with web pages just as a human would.

Some of the key features of Playwright for web scraping include:

* **Cross-browser support** - Playwright supports Chromium, Firefox, and WebKit with a single API, ensuring consistent behavior across all browsers.
* **Auto-waiting** - Playwright automatically waits for elements to be ready before performing actions, reducing flaky scripts and eliminating the need for manual sleep calls.
* **Headless and headful modes** - Playwright can run with or without a visible browser window, making it suitable for both local development and containerized environments.
* **Powerful selectors** - Playwright provides CSS selectors, XPath, text matching, and its own resilient locator API for targeting elements on a page.
* **Network interception** - Playwright can intercept and modify network requests, allowing you to block unnecessary resources or mock API responses during scraping.

To create Actors which use Playwright, start from the [Playwright & Python](https://apify.com/templates/categories/python) Actor template.

On the Apify platform, the Actor will already have Playwright and the necessary browsers preinstalled in its Docker image, including the tools and setup necessary to run browsers in headful mode.

When running the Actor locally, you'll need to finish the Playwright setup yourself before you can run the Actor.

* Linux / macOS
* Windows

```
source .venv/bin/activate

playwright install --with-deps
```

```
.venv\Scripts\activate

playwright install --with-deps
```

## Example Actor[](#example-actor)

This is a simple Actor that recursively scrapes data from linked pages on the same site, up to a maximum depth, starting from URLs in the Actor input.

It uses Playwright to open the pages in an automated Chrome browser, and to extract the title, headings, and links after the pages load.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSB0eXBpbmcgaW1wb3J0IEFueVxcbmZyb20gdXJsbGliLnBhcnNlIGltcG9ydCB1cmxqb2luLCB1cmxzcGxpdFxcblxcbmZyb20gcGxheXdyaWdodC5hc3luY19hcGkgaW1wb3J0IEJyb3dzZXJDb250ZXh0LCBhc3luY19wbGF5d3JpZ2h0XFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFJlcXVlc3RcXG5mcm9tIGFwaWZ5LnN0b3JhZ2VzIGltcG9ydCBSZXF1ZXN0UXVldWVcXG5cXG4jIFRvIHJ1biBsb2NhbGx5LCBpbnN0YWxsIHRoZSBicm93c2VycyBmaXJzdDogYHBsYXl3cmlnaHQgaW5zdGFsbCAtLXdpdGgtZGVwc2AuXFxuIyBPbiB0aGUgQXBpZnkgcGxhdGZvcm0sIGJyb3dzZXJzIGFyZSBhbHJlYWR5IGluIHRoZSBBY3RvcidzIERvY2tlciBpbWFnZS5cXG5cXG5cXG5kZWYgdG9fcGxheXdyaWdodF9wcm94eShwcm94eV91cmw6IHN0cikgLT4gZGljdFtzdHIsIHN0cl06XFxuICAgIFxcXCJcXFwiXFxcIlNwbGl0IGFuIEFwaWZ5IFByb3h5IFVSTCBpbnRvIFBsYXl3cmlnaHQncyBzZXJ2ZXIvdXNlcm5hbWUvcGFzc3dvcmQuXFxcIlxcXCJcXFwiXFxuICAgIHBhcnRzID0gdXJsc3BsaXQocHJveHlfdXJsKVxcbiAgICByZXR1cm4ge1xcbiAgICAgICAgJ3NlcnZlcic6IGYne3BhcnRzLnNjaGVtZX06Ly97cGFydHMuaG9zdG5hbWV9OntwYXJ0cy5wb3J0fScsXFxuICAgICAgICAndXNlcm5hbWUnOiBwYXJ0cy51c2VybmFtZSBvciAnJyxcXG4gICAgICAgICdwYXNzd29yZCc6IHBhcnRzLnBhc3N3b3JkIG9yICcnLFxcbiAgICB9XFxuXFxuXFxuYXN5bmMgZGVmIHNjcmFwZV9wYWdlKFxcbiAgICBjb250ZXh0OiBCcm93c2VyQ29udGV4dCwgdXJsOiBzdHJcXG4pIC0-IHR1cGxlW2RpY3Rbc3RyLCBBbnldLCBsaXN0W3N0cl1dOlxcbiAgICBcXFwiXFxcIlxcXCJPcGVuIHRoZSBVUkwgaW4gYSBuZXcgcGFnZSBhbmQgcmV0dXJuIGl0cyBkYXRhIGFuZCBzYW1lLXNpdGUgbGlua3MuXFxcIlxcXCJcXFwiXFxuICAgIHBhZ2UgPSBhd2FpdCBjb250ZXh0Lm5ld19wYWdlKClcXG4gICAgdHJ5OlxcbiAgICAgICAgYXdhaXQgcGFnZS5nb3RvKHVybClcXG5cXG4gICAgICAgIGRhdGEgPSB7XFxuICAgICAgICAgICAgJ3VybCc6IHVybCxcXG4gICAgICAgICAgICAndGl0bGUnOiBhd2FpdCBwYWdlLnRpdGxlKCksXFxuICAgICAgICAgICAgJ2gxcyc6IFthd2FpdCBoMS50ZXh0X2NvbnRlbnQoKSBmb3IgaDEgaW4gYXdhaXQgcGFnZS5sb2NhdG9yKCdoMScpLmFsbCgpXSxcXG4gICAgICAgICAgICAnaDJzJzogW2F3YWl0IGgyLnRleHRfY29udGVudCgpIGZvciBoMiBpbiBhd2FpdCBwYWdlLmxvY2F0b3IoJ2gyJykuYWxsKCldLFxcbiAgICAgICAgICAgICdoM3MnOiBbYXdhaXQgaDMudGV4dF9jb250ZW50KCkgZm9yIGgzIGluIGF3YWl0IHBhZ2UubG9jYXRvcignaDMnKS5hbGwoKV0sXFxuICAgICAgICB9XFxuXFxuICAgICAgICAjIEtlZXAgb25seSBhYnNvbHV0ZSBsaW5rcyBvbiB0aGUgc2FtZSBob3N0LlxcbiAgICAgICAgbGlua3M6IGxpc3Rbc3RyXSA9IFtdXFxuICAgICAgICBob3N0ID0gdXJsc3BsaXQodXJsKS5uZXRsb2NcXG4gICAgICAgIGZvciBsaW5rIGluIGF3YWl0IHBhZ2UubG9jYXRvcignYScpLmFsbCgpOlxcbiAgICAgICAgICAgIGxpbmtfaHJlZiA9IGF3YWl0IGxpbmsuZ2V0X2F0dHJpYnV0ZSgnaHJlZicpXFxuICAgICAgICAgICAgbGlua191cmwgPSB1cmxqb2luKHVybCwgbGlua19ocmVmKVxcbiAgICAgICAgICAgIGlmIG5vdCBsaW5rX3VybC5zdGFydHN3aXRoKCgnaHR0cDovLycsICdodHRwczovLycpKTpcXG4gICAgICAgICAgICAgICAgY29udGludWVcXG4gICAgICAgICAgICBpZiB1cmxzcGxpdChsaW5rX3VybCkubmV0bG9jID09IGhvc3Q6XFxuICAgICAgICAgICAgICAgIGxpbmtzLmFwcGVuZChsaW5rX3VybClcXG5cXG4gICAgICAgIHJldHVybiBkYXRhLCBsaW5rc1xcblxcbiAgICBmaW5hbGx5OlxcbiAgICAgICAgYXdhaXQgcGFnZS5jbG9zZSgpXFxuXFxuXFxuYXN5bmMgZGVmIGVucXVldWVfbGlua3MoXFxuICAgIHJlcXVlc3RfcXVldWU6IFJlcXVlc3RRdWV1ZSxcXG4gICAgbGlua3M6IGxpc3Rbc3RyXSxcXG4gICAgKixcXG4gICAgZGVwdGg6IGludCxcXG4gICAgbWF4X2RlcHRoOiBpbnQsXFxuKSAtPiBOb25lOlxcbiAgICBcXFwiXFxcIlxcXCJFbnF1ZXVlIHRoZSBsaW5rcyBvbmUgbGV2ZWwgZGVlcGVyLCB1bmxlc3MgbWF4X2RlcHRoIHdhcyByZWFjaGVkLlxcXCJcXFwiXFxcIlxcbiAgICBpZiBkZXB0aCA-PSBtYXhfZGVwdGg6XFxuICAgICAgICByZXR1cm5cXG5cXG4gICAgZm9yIGxpbmtfdXJsIGluIGxpbmtzOlxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidFbnF1ZXVpbmcge2xpbmtfdXJsfSAuLi4nKVxcbiAgICAgICAgcmVxdWVzdCA9IFJlcXVlc3QuZnJvbV91cmwobGlua191cmwpXFxuICAgICAgICByZXF1ZXN0LmNyYXdsX2RlcHRoID0gZGVwdGggKyAxXFxuICAgICAgICBhd2FpdCByZXF1ZXN0X3F1ZXVlLmFkZF9yZXF1ZXN0KHJlcXVlc3QpXFxuXFxuXFxuYXN5bmMgZGVmIG1haW4oKSAtPiBOb25lOlxcbiAgICBhc3luYyB3aXRoIEFjdG9yOlxcbiAgICAgICAgIyBSZWFkIHRoZSBBY3RvciBpbnB1dC5cXG4gICAgICAgIGFjdG9yX2lucHV0ID0gYXdhaXQgQWN0b3IuZ2V0X2lucHV0KCkgb3Ige31cXG4gICAgICAgIHN0YXJ0X3VybHMgPSBhY3Rvcl9pbnB1dC5nZXQoJ3N0YXJ0VXJscycsIFt7J3VybCc6ICdodHRwczovL2NyYXdsZWUuZGV2J31dKVxcbiAgICAgICAgbWF4X2RlcHRoID0gYWN0b3JfaW5wdXQuZ2V0KCdtYXhEZXB0aCcsIDEpXFxuXFxuICAgICAgICBpZiBub3Qgc3RhcnRfdXJsczpcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbygnTm8gc3RhcnQgVVJMcyBzcGVjaWZpZWQgaW4gQWN0b3IgaW5wdXQsIGV4aXRpbmcuLi4nKVxcbiAgICAgICAgICAgIGF3YWl0IEFjdG9yLmV4aXQoKVxcblxcbiAgICAgICAgIyBTZXQgdXAgdGhlIHByb3h5IGNvbmZpZ3VyYXRpb24uIEEgZnJlc2ggcHJveHkgVVJMIGlzIGZldGNoZWQgcGVyIHJlcXVlc3QgYmVsb3cuXFxuICAgICAgICBwcm94eV9jb25maWd1cmF0aW9uID0gYXdhaXQgQWN0b3IuY3JlYXRlX3Byb3h5X2NvbmZpZ3VyYXRpb24oKVxcblxcbiAgICAgICAgIyBPcGVuIHRoZSByZXF1ZXN0IHF1ZXVlIGFuZCBlbnF1ZXVlIHRoZSBzdGFydCBVUkxzIChjcmF3bCBkZXB0aCAwKS5cXG4gICAgICAgIHJlcXVlc3RfcXVldWUgPSBhd2FpdCBBY3Rvci5vcGVuX3JlcXVlc3RfcXVldWUoKVxcbiAgICAgICAgZm9yIHN0YXJ0X3VybCBpbiBzdGFydF91cmxzOlxcbiAgICAgICAgICAgIHVybCA9IHN0YXJ0X3VybC5nZXQoJ3VybCcpXFxuICAgICAgICAgICAgQWN0b3IubG9nLmluZm8oZidFbnF1ZXVpbmcgc3RhcnQgVVJMOiB7dXJsfScpXFxuICAgICAgICAgICAgYXdhaXQgcmVxdWVzdF9xdWV1ZS5hZGRfcmVxdWVzdChSZXF1ZXN0LmZyb21fdXJsKHVybCkpXFxuXFxuICAgICAgICAjIENhcCB0aGUgY3Jhd2wuIFJhaXNlIG9yIHJlbW92ZSB0aGUgbGltaXQgdG8gZm9sbG93IG1vcmUgcGFnZXMuXFxuICAgICAgICBtYXhfcmVxdWVzdHMgPSAxMFxcbiAgICAgICAgaGFuZGxlZF9yZXF1ZXN0cyA9IDBcXG5cXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKCdMYXVuY2hpbmcgUGxheXdyaWdodC4uLicpXFxuXFxuICAgICAgICBhc3luYyB3aXRoIGFzeW5jX3BsYXl3cmlnaHQoKSBhcyBwbGF5d3JpZ2h0OlxcbiAgICAgICAgICAgIGJyb3dzZXIgPSBhd2FpdCBwbGF5d3JpZ2h0LmNocm9taXVtLmxhdW5jaChcXG4gICAgICAgICAgICAgICAgaGVhZGxlc3M9QWN0b3IuY29uZmlndXJhdGlvbi5oZWFkbGVzcyxcXG4gICAgICAgICAgICAgICAgYXJncz1bJy0tbm8tc2FuZGJveCcsICctLWRpc2FibGUtZGV2LXNobS11c2FnZScsICctLWRpc2FibGUtZ3B1J10sXFxuICAgICAgICAgICAgKVxcblxcbiAgICAgICAgICAgIHdoaWxlIGhhbmRsZWRfcmVxdWVzdHMgPCBtYXhfcmVxdWVzdHMgYW5kIChcXG4gICAgICAgICAgICAgICAgcmVxdWVzdCA6PSBhd2FpdCByZXF1ZXN0X3F1ZXVlLmZldGNoX25leHRfcmVxdWVzdCgpXFxuICAgICAgICAgICAgKTpcXG4gICAgICAgICAgICAgICAgaGFuZGxlZF9yZXF1ZXN0cyArPSAxXFxuICAgICAgICAgICAgICAgIHVybCA9IHJlcXVlc3QudXJsXFxuICAgICAgICAgICAgICAgIGRlcHRoID0gcmVxdWVzdC5jcmF3bF9kZXB0aFxcbiAgICAgICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1NjcmFwaW5nIHt1cmx9IChkZXB0aD17ZGVwdGh9KSAuLi4nKVxcblxcbiAgICAgICAgICAgICAgICAjIEEgbmV3IGNvbnRleHQgd2l0aCBhIGZyZXNoIHByb3h5IFVSTCBwZXIgcmVxdWVzdCByb3RhdGVzIHRoZSBwcm94eSBJUC5cXG4gICAgICAgICAgICAgICAgcHJveHlfdXJsID0gKFxcbiAgICAgICAgICAgICAgICAgICAgYXdhaXQgcHJveHlfY29uZmlndXJhdGlvbi5uZXdfdXJsKCkgaWYgcHJveHlfY29uZmlndXJhdGlvbiBlbHNlIE5vbmVcXG4gICAgICAgICAgICAgICAgKVxcbiAgICAgICAgICAgICAgICBjb250ZXh0ID0gYXdhaXQgYnJvd3Nlci5uZXdfY29udGV4dChcXG4gICAgICAgICAgICAgICAgICAgIHByb3h5PXRvX3BsYXl3cmlnaHRfcHJveHkocHJveHlfdXJsKSBpZiBwcm94eV91cmwgZWxzZSBOb25lLFxcbiAgICAgICAgICAgICAgICApXFxuXFxuICAgICAgICAgICAgICAgIHRyeTpcXG4gICAgICAgICAgICAgICAgICAgIGRhdGEsIGxpbmtzID0gYXdhaXQgc2NyYXBlX3BhZ2UoY29udGV4dCwgdXJsKVxcbiAgICAgICAgICAgICAgICAgICAgYXdhaXQgQWN0b3IucHVzaF9kYXRhKGRhdGEpXFxuICAgICAgICAgICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhcXG4gICAgICAgICAgICAgICAgICAgICAgICBmJ1N0b3JlZCBkYXRhIGZyb20ge3VybH0gJ1xcbiAgICAgICAgICAgICAgICAgICAgICAgIGYnKHRpdGxlPXtkYXRhW1xcXCJ0aXRsZVxcXCJdIXJ9LCB7bGVuKGxpbmtzKX0gbGlua3MgZm91bmQpLidcXG4gICAgICAgICAgICAgICAgICAgIClcXG4gICAgICAgICAgICAgICAgICAgIGF3YWl0IGVucXVldWVfbGlua3MoXFxuICAgICAgICAgICAgICAgICAgICAgICAgcmVxdWVzdF9xdWV1ZSwgbGlua3MsIGRlcHRoPWRlcHRoLCBtYXhfZGVwdGg9bWF4X2RlcHRoXFxuICAgICAgICAgICAgICAgICAgICApXFxuXFxuICAgICAgICAgICAgICAgIGV4Y2VwdCBFeGNlcHRpb246XFxuICAgICAgICAgICAgICAgICAgICBBY3Rvci5sb2cuZXhjZXB0aW9uKGYnQ2Fubm90IGV4dHJhY3QgZGF0YSBmcm9tIHt1cmx9LicpXFxuXFxuICAgICAgICAgICAgICAgIGZpbmFsbHk6XFxuICAgICAgICAgICAgICAgICAgICBhd2FpdCBjb250ZXh0LmNsb3NlKClcXG4gICAgICAgICAgICAgICAgICAgIGF3YWl0IHJlcXVlc3RfcXVldWUubWFya19yZXF1ZXN0X2FzX2hhbmRsZWQocmVxdWVzdClcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6NDA5NiwidGltZW91dCI6MTgwfX0.LO4PwZvywa1HYlN0QU-w9n6opdzK3AtsDTNcbZk8m0o\&asrc=run_on_apify)

```
import asyncio

from typing import Any

from urllib.parse import urljoin, urlsplit



from playwright.async_api import BrowserContext, async_playwright



from apify import Actor, Request

from apify.storages import RequestQueue



# To run locally, install the browsers first: `playwright install --with-deps`.

# On the Apify platform, browsers are already in the Actor's Docker image.





def to_playwright_proxy(proxy_url: str) -> dict[str, str]:

    """Split an Apify Proxy URL into Playwright's server/username/password."""

    parts = urlsplit(proxy_url)

    return {

        'server': f'{parts.scheme}://{parts.hostname}:{parts.port}',

        'username': parts.username or '',

        'password': parts.password or '',

    }





async def scrape_page(

    context: BrowserContext, url: str

) -> tuple[dict[str, Any], list[str]]:

    """Open the URL in a new page and return its data and same-site links."""

    page = await context.new_page()

    try:

        await page.goto(url)



        data = {

            'url': url,

            'title': await page.title(),

            'h1s': [await h1.text_content() for h1 in await page.locator('h1').all()],

            'h2s': [await h2.text_content() for h2 in await page.locator('h2').all()],

            'h3s': [await h3.text_content() for h3 in await page.locator('h3').all()],

        }



        # Keep only absolute links on the same host.

        links: list[str] = []

        host = urlsplit(url).netloc

        for link in await page.locator('a').all():

            link_href = await link.get_attribute('href')

            link_url = urljoin(url, link_href)

            if not link_url.startswith(('http://', 'https://')):

                continue

            if urlsplit(link_url).netloc == host:

                links.append(link_url)



        return data, links



    finally:

        await page.close()





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



        # Set up the proxy configuration. A fresh proxy URL is fetched per request below.

        proxy_configuration = await Actor.create_proxy_configuration()



        # Open the request queue and enqueue the start URLs (crawl depth 0).

        request_queue = await Actor.open_request_queue()

        for start_url in start_urls:

            url = start_url.get('url')

            Actor.log.info(f'Enqueuing start URL: {url}')

            await request_queue.add_request(Request.from_url(url))



        # Cap the crawl. Raise or remove the limit to follow more pages.

        max_requests = 10

        handled_requests = 0



        Actor.log.info('Launching Playwright...')



        async with async_playwright() as playwright:

            browser = await playwright.chromium.launch(

                headless=Actor.configuration.headless,

                args=['--no-sandbox', '--disable-dev-shm-usage', '--disable-gpu'],

            )



            while handled_requests < max_requests and (

                request := await request_queue.fetch_next_request()

            ):

                handled_requests += 1

                url = request.url

                depth = request.crawl_depth

                Actor.log.info(f'Scraping {url} (depth={depth}) ...')



                # A new context with a fresh proxy URL per request rotates the proxy IP.

                proxy_url = (

                    await proxy_configuration.new_url() if proxy_configuration else None

                )

                context = await browser.new_context(

                    proxy=to_playwright_proxy(proxy_url) if proxy_url else None,

                )



                try:

                    data, links = await scrape_page(context, url)

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

                    await context.close()

                    await request_queue.mark_request_as_handled(request)





if __name__ == '__main__':

    asyncio.run(main())
```

## Using Apify Proxy[](#using-apify-proxy)

Running on the Apify platform gives your scraper access to [Apify Proxy](https://docs.apify.com/platform/proxy), which rotates IP addresses to avoid rate limiting and blocking. The example creates a proxy configuration with `Actor.create_proxy_configuration` and fetches a fresh proxy URL for every request. Playwright applies a proxy per browser context. Each request runs in its own new context to rotate the IP. The `to_playwright_proxy` helper splits that URL into the `server`, `username`, and `password` fields Playwright expects. To select specific proxy groups or a country, pass the relevant arguments to `Actor.create_proxy_configuration`. For details, see [Proxy management](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/proxy-management.md).

## Conclusion[](#conclusion)

In this guide you learned how to create Actors that use Playwright to scrape websites. Playwright is a powerful tool that can be used to manage browser instances and scrape websites that require JavaScript execution. See the [Actor templates](https://apify.com/templates/categories/python) to get started with your own scraping tasks. If you have questions or need assistance, feel free to reach out on our [GitHub](https://github.com/apify/apify-sdk-python) or join our [Discord community](https://discord.com/invite/jyEM2PRvMU). Happy scraping!

## Additional resources[](#additional-resources)

* [Apify templates: Playwright + Chrome](https://apify.com/templates/python-playwright)
* [Apify templates: Crawlee + Playwright + Chrome](https://apify.com/templates/python-crawlee-playwright)
* [Playwright: Official documentation](https://playwright.dev/python/)
