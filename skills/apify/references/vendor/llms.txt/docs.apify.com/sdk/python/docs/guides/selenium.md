# Browser automation with Selenium

Copy for LLM

In this guide, you'll learn how to use [Selenium](https://www.selenium.dev/) for browser automation and web scraping in your Apify Actors.

## Introduction[](#introduction)

[Selenium](https://www.selenium.dev/) is a tool for web automation and testing that can also be used for web scraping. It allows you to control a web browser programmatically and interact with web pages just as a human would.

Some of the key features of Selenium for web scraping include:

* **Broad ecosystem** - Selenium has a large community and extensive documentation, with support for multiple programming languages beyond Python.
* **WebDriver protocol** - Selenium uses the W3C WebDriver protocol, providing standardized browser automation that works with Chrome, Firefox, Edge, and Safari.
* **Headless and headful modes** - Selenium can run with or without a visible browser window, making it suitable for both local development and containerized environments.
* **Flexible element selection** - Selenium provides CSS selectors, XPath, ID, class name, and other strategies for locating elements on a page.
* **User interaction emulation** - Selenium allows you to emulate user actions like clicking, scrolling, filling out forms, and typing, which is useful for scraping dynamic websites.

To create Actors which use Selenium, start from the [Selenium & Python](https://apify.com/templates/categories/python) Actor template.

On the Apify platform, the Actor will already have Selenium and the necessary browsers preinstalled in its Docker image, including the tools and setup necessary to run browsers in headful mode.

When running the Actor locally, you'll need to install the Selenium browser drivers yourself. Refer to the [Selenium documentation](https://www.selenium.dev/documentation/webdriver/getting_started/install_drivers/) for installation instructions.

## Example Actor[](#example-actor)

This is a simple Actor that recursively scrapes data from linked pages on the same site, up to a maximum depth, starting from URLs in the Actor input.

It uses Selenium ChromeDriver to open the pages in an automated Chrome browser, and to extract the title, headings, and links after the pages load.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuZnJvbSB0eXBpbmcgaW1wb3J0IEFueVxcbmZyb20gdXJsbGliLnBhcnNlIGltcG9ydCB1cmxqb2luLCB1cmxzcGxpdFxcblxcbmZyb20gc2VsZW5pdW0gaW1wb3J0IHdlYmRyaXZlclxcbmZyb20gc2VsZW5pdW0ud2ViZHJpdmVyLmNocm9tZS5vcHRpb25zIGltcG9ydCBPcHRpb25zIGFzIENocm9tZU9wdGlvbnNcXG5mcm9tIHNlbGVuaXVtLndlYmRyaXZlci5jb21tb24uYnkgaW1wb3J0IEJ5XFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFJlcXVlc3RcXG5mcm9tIGFwaWZ5LnN0b3JhZ2VzIGltcG9ydCBSZXF1ZXN0UXVldWVcXG5cXG4jIFRvIHJ1biBsb2NhbGx5LCBpbnN0YWxsIHRoZSBTZWxlbml1bSBDaHJvbWVkcml2ZXI6XFxuIyBodHRwczovL3d3dy5zZWxlbml1bS5kZXYvZG9jdW1lbnRhdGlvbi93ZWJkcml2ZXIvZ2V0dGluZ19zdGFydGVkL2luc3RhbGxfZHJpdmVycy9cXG4jIE9uIHRoZSBBcGlmeSBwbGF0Zm9ybSwgaXQncyBhbHJlYWR5IGluIHRoZSBBY3RvcidzIERvY2tlciBpbWFnZS5cXG5cXG5cXG5kZWYgYnVpbGRfY2hyb21lX2RyaXZlcigpIC0-IHdlYmRyaXZlci5DaHJvbWU6XFxuICAgIFxcXCJcXFwiXFxcIkNyZWF0ZSBhIGhlYWRsZXNzIENocm9tZSBXZWJEcml2ZXIgc3VpdGFibGUgZm9yIGEgY29udGFpbmVyLlxcXCJcXFwiXFxcIlxcbiAgICBjaHJvbWVfb3B0aW9ucyA9IENocm9tZU9wdGlvbnMoKVxcblxcbiAgICBpZiBBY3Rvci5jb25maWd1cmF0aW9uLmhlYWRsZXNzOlxcbiAgICAgICAgY2hyb21lX29wdGlvbnMuYWRkX2FyZ3VtZW50KCctLWhlYWRsZXNzPW5ldycpXFxuXFxuICAgIGNocm9tZV9vcHRpb25zLmFkZF9hcmd1bWVudCgnLS1uby1zYW5kYm94JylcXG4gICAgY2hyb21lX29wdGlvbnMuYWRkX2FyZ3VtZW50KCctLWRpc2FibGUtZGV2LXNobS11c2FnZScpXFxuICAgIGNocm9tZV9vcHRpb25zLmFkZF9hcmd1bWVudCgnLS1kaXNhYmxlLWdwdScpXFxuXFxuICAgIHJldHVybiB3ZWJkcml2ZXIuQ2hyb21lKG9wdGlvbnM9Y2hyb21lX29wdGlvbnMpXFxuXFxuXFxuZGVmIHNjcmFwZV9wYWdlKGRyaXZlcjogd2ViZHJpdmVyLkNocm9tZSwgdXJsOiBzdHIpIC0-IHR1cGxlW2RpY3Rbc3RyLCBBbnldLCBsaXN0W3N0cl1dOlxcbiAgICBcXFwiXFxcIlxcXCJOYXZpZ2F0ZSB0byB0aGUgVVJMIHdpdGggU2VsZW5pdW0gYW5kIHJldHVybiBpdHMgZGF0YSBhbmQgc2FtZS1zaXRlIGxpbmtzLlxcXCJcXFwiXFxcIlxcbiAgICBkcml2ZXIuZ2V0KHVybClcXG5cXG4gICAgZGF0YSA9IHtcXG4gICAgICAgICd1cmwnOiB1cmwsXFxuICAgICAgICAndGl0bGUnOiBkcml2ZXIudGl0bGUsXFxuICAgICAgICAnaDFzJzogW2VsLnRleHQgZm9yIGVsIGluIGRyaXZlci5maW5kX2VsZW1lbnRzKEJ5LlRBR19OQU1FLCAnaDEnKV0sXFxuICAgICAgICAnaDJzJzogW2VsLnRleHQgZm9yIGVsIGluIGRyaXZlci5maW5kX2VsZW1lbnRzKEJ5LlRBR19OQU1FLCAnaDInKV0sXFxuICAgICAgICAnaDNzJzogW2VsLnRleHQgZm9yIGVsIGluIGRyaXZlci5maW5kX2VsZW1lbnRzKEJ5LlRBR19OQU1FLCAnaDMnKV0sXFxuICAgIH1cXG5cXG4gICAgIyBLZWVwIG9ubHkgYWJzb2x1dGUgbGlua3Mgb24gdGhlIHNhbWUgaG9zdC5cXG4gICAgbGlua3M6IGxpc3Rbc3RyXSA9IFtdXFxuICAgIGhvc3QgPSB1cmxzcGxpdCh1cmwpLm5ldGxvY1xcbiAgICBmb3IgbGluayBpbiBkcml2ZXIuZmluZF9lbGVtZW50cyhCeS5UQUdfTkFNRSwgJ2EnKTpcXG4gICAgICAgIGxpbmtfdXJsID0gdXJsam9pbih1cmwsIGxpbmsuZ2V0X2F0dHJpYnV0ZSgnaHJlZicpKVxcbiAgICAgICAgaWYgbm90IGxpbmtfdXJsLnN0YXJ0c3dpdGgoKCdodHRwOi8vJywgJ2h0dHBzOi8vJykpOlxcbiAgICAgICAgICAgIGNvbnRpbnVlXFxuICAgICAgICBpZiB1cmxzcGxpdChsaW5rX3VybCkubmV0bG9jID09IGhvc3Q6XFxuICAgICAgICAgICAgbGlua3MuYXBwZW5kKGxpbmtfdXJsKVxcblxcbiAgICByZXR1cm4gZGF0YSwgbGlua3NcXG5cXG5cXG5hc3luYyBkZWYgZW5xdWV1ZV9saW5rcyhcXG4gICAgcmVxdWVzdF9xdWV1ZTogUmVxdWVzdFF1ZXVlLFxcbiAgICBsaW5rczogbGlzdFtzdHJdLFxcbiAgICAqLFxcbiAgICBkZXB0aDogaW50LFxcbiAgICBtYXhfZGVwdGg6IGludCxcXG4pIC0-IE5vbmU6XFxuICAgIFxcXCJcXFwiXFxcIkVucXVldWUgdGhlIGxpbmtzIG9uZSBsZXZlbCBkZWVwZXIsIHVubGVzcyBtYXhfZGVwdGggd2FzIHJlYWNoZWQuXFxcIlxcXCJcXFwiXFxuICAgIGlmIGRlcHRoID49IG1heF9kZXB0aDpcXG4gICAgICAgIHJldHVyblxcblxcbiAgICBmb3IgbGlua191cmwgaW4gbGlua3M6XFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0VucXVldWluZyB7bGlua191cmx9IC4uLicpXFxuICAgICAgICByZXF1ZXN0ID0gUmVxdWVzdC5mcm9tX3VybChsaW5rX3VybClcXG4gICAgICAgIHJlcXVlc3QuY3Jhd2xfZGVwdGggPSBkZXB0aCArIDFcXG4gICAgICAgIGF3YWl0IHJlcXVlc3RfcXVldWUuYWRkX3JlcXVlc3QocmVxdWVzdClcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICAjIFJlYWQgdGhlIEFjdG9yIGlucHV0LlxcbiAgICAgICAgYWN0b3JfaW5wdXQgPSBhd2FpdCBBY3Rvci5nZXRfaW5wdXQoKSBvciB7fVxcbiAgICAgICAgc3RhcnRfdXJscyA9IGFjdG9yX2lucHV0LmdldCgnc3RhcnRVcmxzJywgW3sndXJsJzogJ2h0dHBzOi8vY3Jhd2xlZS5kZXYnfV0pXFxuICAgICAgICBtYXhfZGVwdGggPSBhY3Rvcl9pbnB1dC5nZXQoJ21heERlcHRoJywgMSlcXG5cXG4gICAgICAgIGlmIG5vdCBzdGFydF91cmxzOlxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKCdObyBzdGFydCBVUkxzIHNwZWNpZmllZCBpbiBBY3RvciBpbnB1dCwgZXhpdGluZy4uLicpXFxuICAgICAgICAgICAgYXdhaXQgQWN0b3IuZXhpdCgpXFxuXFxuICAgICAgICAjIE9wZW4gdGhlIHJlcXVlc3QgcXVldWUgYW5kIGVucXVldWUgdGhlIHN0YXJ0IFVSTHMgKGNyYXdsIGRlcHRoIDApLlxcbiAgICAgICAgcmVxdWVzdF9xdWV1ZSA9IGF3YWl0IEFjdG9yLm9wZW5fcmVxdWVzdF9xdWV1ZSgpXFxuICAgICAgICBmb3Igc3RhcnRfdXJsIGluIHN0YXJ0X3VybHM6XFxuICAgICAgICAgICAgdXJsID0gc3RhcnRfdXJsLmdldCgndXJsJylcXG4gICAgICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ0VucXVldWluZyBzdGFydCBVUkw6IHt1cmx9JylcXG4gICAgICAgICAgICBhd2FpdCByZXF1ZXN0X3F1ZXVlLmFkZF9yZXF1ZXN0KFJlcXVlc3QuZnJvbV91cmwodXJsKSlcXG5cXG4gICAgICAgICMgQ2FwIHRoZSBjcmF3bC4gUmFpc2Ugb3IgcmVtb3ZlIHRoZSBsaW1pdCB0byBmb2xsb3cgbW9yZSBwYWdlcy5cXG4gICAgICAgIG1heF9yZXF1ZXN0cyA9IDEwXFxuICAgICAgICBoYW5kbGVkX3JlcXVlc3RzID0gMFxcblxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oJ0xhdW5jaGluZyBDaHJvbWUgV2ViRHJpdmVyLi4uJylcXG4gICAgICAgIGRyaXZlciA9IGJ1aWxkX2Nocm9tZV9kcml2ZXIoKVxcblxcbiAgICAgICAgd2hpbGUgaGFuZGxlZF9yZXF1ZXN0cyA8IG1heF9yZXF1ZXN0cyBhbmQgKFxcbiAgICAgICAgICAgIHJlcXVlc3QgOj0gYXdhaXQgcmVxdWVzdF9xdWV1ZS5mZXRjaF9uZXh0X3JlcXVlc3QoKVxcbiAgICAgICAgKTpcXG4gICAgICAgICAgICBoYW5kbGVkX3JlcXVlc3RzICs9IDFcXG4gICAgICAgICAgICB1cmwgPSByZXF1ZXN0LnVybFxcbiAgICAgICAgICAgIGRlcHRoID0gcmVxdWVzdC5jcmF3bF9kZXB0aFxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnU2NyYXBpbmcge3VybH0gKGRlcHRoPXtkZXB0aH0pIC4uLicpXFxuXFxuICAgICAgICAgICAgdHJ5OlxcbiAgICAgICAgICAgICAgICAjIEJsb2NraW5nIFdlYkRyaXZlciBjYWxscyBydW4gaW4gYSB3b3JrZXIgdGhyZWFkLlxcbiAgICAgICAgICAgICAgICBkYXRhLCBsaW5rcyA9IGF3YWl0IGFzeW5jaW8udG9fdGhyZWFkKHNjcmFwZV9wYWdlLCBkcml2ZXIsIHVybClcXG4gICAgICAgICAgICAgICAgYXdhaXQgQWN0b3IucHVzaF9kYXRhKGRhdGEpXFxuICAgICAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKFxcbiAgICAgICAgICAgICAgICAgICAgZidTdG9yZWQgZGF0YSBmcm9tIHt1cmx9ICdcXG4gICAgICAgICAgICAgICAgICAgIGYnKHRpdGxlPXtkYXRhW1xcXCJ0aXRsZVxcXCJdIXJ9LCB7bGVuKGxpbmtzKX0gbGlua3MgZm91bmQpLidcXG4gICAgICAgICAgICAgICAgKVxcbiAgICAgICAgICAgICAgICBhd2FpdCBlbnF1ZXVlX2xpbmtzKFxcbiAgICAgICAgICAgICAgICAgICAgcmVxdWVzdF9xdWV1ZSwgbGlua3MsIGRlcHRoPWRlcHRoLCBtYXhfZGVwdGg9bWF4X2RlcHRoXFxuICAgICAgICAgICAgICAgIClcXG5cXG4gICAgICAgICAgICBleGNlcHQgRXhjZXB0aW9uOlxcbiAgICAgICAgICAgICAgICBBY3Rvci5sb2cuZXhjZXB0aW9uKGYnQ2Fubm90IGV4dHJhY3QgZGF0YSBmcm9tIHt1cmx9LicpXFxuXFxuICAgICAgICAgICAgZmluYWxseTpcXG4gICAgICAgICAgICAgICAgYXdhaXQgcmVxdWVzdF9xdWV1ZS5tYXJrX3JlcXVlc3RfYXNfaGFuZGxlZChyZXF1ZXN0KVxcblxcbiAgICAgICAgZHJpdmVyLnF1aXQoKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.ciN9AsfZ0a-HWUkL9xjd05GwMYpxvqCeSnQTGR1udZw\&asrc=run_on_apify)

```
import asyncio

from typing import Any

from urllib.parse import urljoin, urlsplit



from selenium import webdriver

from selenium.webdriver.chrome.options import Options as ChromeOptions

from selenium.webdriver.common.by import By



from apify import Actor, Request

from apify.storages import RequestQueue



# To run locally, install the Selenium Chromedriver:

# https://www.selenium.dev/documentation/webdriver/getting_started/install_drivers/

# On the Apify platform, it's already in the Actor's Docker image.





def build_chrome_driver() -> webdriver.Chrome:

    """Create a headless Chrome WebDriver suitable for a container."""

    chrome_options = ChromeOptions()



    if Actor.configuration.headless:

        chrome_options.add_argument('--headless=new')



    chrome_options.add_argument('--no-sandbox')

    chrome_options.add_argument('--disable-dev-shm-usage')

    chrome_options.add_argument('--disable-gpu')



    return webdriver.Chrome(options=chrome_options)





def scrape_page(driver: webdriver.Chrome, url: str) -> tuple[dict[str, Any], list[str]]:

    """Navigate to the URL with Selenium and return its data and same-site links."""

    driver.get(url)



    data = {

        'url': url,

        'title': driver.title,

        'h1s': [el.text for el in driver.find_elements(By.TAG_NAME, 'h1')],

        'h2s': [el.text for el in driver.find_elements(By.TAG_NAME, 'h2')],

        'h3s': [el.text for el in driver.find_elements(By.TAG_NAME, 'h3')],

    }



    # Keep only absolute links on the same host.

    links: list[str] = []

    host = urlsplit(url).netloc

    for link in driver.find_elements(By.TAG_NAME, 'a'):

        link_url = urljoin(url, link.get_attribute('href'))

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



        # Open the request queue and enqueue the start URLs (crawl depth 0).

        request_queue = await Actor.open_request_queue()

        for start_url in start_urls:

            url = start_url.get('url')

            Actor.log.info(f'Enqueuing start URL: {url}')

            await request_queue.add_request(Request.from_url(url))



        # Cap the crawl. Raise or remove the limit to follow more pages.

        max_requests = 10

        handled_requests = 0



        Actor.log.info('Launching Chrome WebDriver...')

        driver = build_chrome_driver()



        while handled_requests < max_requests and (

            request := await request_queue.fetch_next_request()

        ):

            handled_requests += 1

            url = request.url

            depth = request.crawl_depth

            Actor.log.info(f'Scraping {url} (depth={depth}) ...')



            try:

                # Blocking WebDriver calls run in a worker thread.

                data, links = await asyncio.to_thread(scrape_page, driver, url)

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



        driver.quit()





if __name__ == '__main__':

    asyncio.run(main())
```

## Using Apify Proxy[](#using-apify-proxy)

Running on the Apify platform gives your scraper access to [Apify Proxy](https://docs.apify.com/platform/proxy), which rotates IP addresses to avoid rate limiting and blocking. The runnable example Actor skips the proxy to stay simple. This section extends it to route the browser through Apify Proxy. The snippet below isn't a complete, runnable Actor on its own. It shows only the proxy-specific parts you add to the example Actor.

Chrome ignores the credentials passed in the `--proxy-server` flag. To use an authenticated proxy such as Apify Proxy, configure it from inside a small extension. The `proxy_auth_extension` helper builds one at runtime. Its service worker sets the proxy server and answers the browser's authentication challenge with the username and password. The proxy-aware `build_chrome_driver` below replaces the simple one from the example Actor and loads that extension. The new headless mode (`--headless=new`) is required for Chrome to load it.

```
import json

from pathlib import Path

from tempfile import mkdtemp

from urllib.parse import urlsplit

from zipfile import ZipFile



from selenium import webdriver

from selenium.webdriver.chrome.options import Options as ChromeOptions



from apify import Actor





def proxy_auth_extension(proxy_url: str) -> str:

    """Build a Chrome extension that routes Chrome through an authenticated proxy."""

    parts = urlsplit(proxy_url)



    manifest = {

        'name': 'Apify Proxy',

        'version': '1.0.0',

        'manifest_version': 3,

        'permissions': ['proxy', 'webRequest', 'webRequestAuthProvider'],

        'host_permissions': ['<all_urls>'],

        'background': {'service_worker': 'background.js'},

        'minimum_chrome_version': '108',

    }



    # The service worker sets the proxy and answers the auth challenge.

    proxy_config = json.dumps(

        {

            'mode': 'fixed_servers',

            'rules': {

                'singleProxy': {

                    'scheme': parts.scheme,

                    'host': parts.hostname,

                    'port': parts.port,

                },

            },

        }

    )

    credentials = json.dumps(

        {'username': parts.username or '', 'password': parts.password or ''}

    )

    background = (

        'chrome.proxy.settings.set('

        '{value: ' + proxy_config + ', scope: "regular"});\n'

        'chrome.webRequest.onAuthRequired.addListener(\n'

        '    () => ({authCredentials: ' + credentials + '}),\n'

        '    {urls: ["<all_urls>"]},\n'

        '    ["blocking"],\n'

        ');\n'

    )



    extension_path = Path(mkdtemp()) / 'apify_proxy.zip'

    with ZipFile(extension_path, 'w') as archive:

        archive.writestr('manifest.json', json.dumps(manifest))

        archive.writestr('background.js', background)

    return str(extension_path)





def build_chrome_driver(proxy_url: str) -> webdriver.Chrome:

    """Create a headless Chrome WebDriver routed through an authenticated proxy."""

    chrome_options = ChromeOptions()



    if Actor.configuration.headless:

        # The new headless mode is required to load the proxy extension.

        chrome_options.add_argument('--headless=new')



    chrome_options.add_argument('--no-sandbox')

    chrome_options.add_argument('--disable-dev-shm-usage')

    chrome_options.add_argument('--disable-gpu')



    # Load the proxy extension and keep it enabled in headless mode.

    chrome_options.add_extension(proxy_auth_extension(proxy_url))

    chrome_options.add_argument(

        '--disable-features=DisableLoadExtensionCommandLineSwitch'

    )



    return webdriver.Chrome(options=chrome_options)
```

To wire the proxy into the example Actor, create the proxy configuration in `main` with `Actor.create_proxy_configuration`, get a URL with `await proxy_configuration.new_url()`, and pass it to `build_chrome_driver`. To select specific proxy groups or a country, pass the relevant arguments to `Actor.create_proxy_configuration`. For details, see [Proxy management](https://docs.apify.com/sdk/python/sdk/python/docs/concepts/proxy-management.md).

## Conclusion[](#conclusion)

In this guide you learned how to use Selenium for web scraping in Apify Actors. You can now create your own Actors that use Selenium to scrape dynamic websites and interact with web pages just like a human would. See the [Actor templates](https://apify.com/templates/categories/python) to get started with your own scraping tasks. If you have questions or need assistance, feel free to reach out on our [GitHub](https://github.com/apify/apify-sdk-python) or join our [Discord community](https://discord.com/invite/jyEM2PRvMU). Happy scraping!

## Additional resources[](#additional-resources)

* [Apify templates: Selenium + Chrome](https://apify.com/templates/python-selenium)
* [Selenium: Official documentation](https://www.selenium.dev/documentation/)
