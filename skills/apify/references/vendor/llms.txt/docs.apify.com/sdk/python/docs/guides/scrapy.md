# Building crawlers with Scrapy

Copy for LLM

In this guide, you'll learn how to build web crawlers with the [Scrapy](https://scrapy.org/) framework in your Apify Actors.

## Introduction[](#introduction)

[Scrapy](https://scrapy.org/) is an open-source web scraping framework for Python. It provides tools for defining scrapers, extracting data from web pages, following links, and handling pagination. With the Apify SDK, Scrapy projects can be converted into Apify [Actors](https://docs.apify.com/platform/actors), integrated with Apify [storages](https://docs.apify.com/platform/storage), and executed on the Apify [platform](https://docs.apify.com/platform).

## Integrating Scrapy with the Apify platform[](#integrating-scrapy-with-the-apify-platform)

The Apify SDK provides an Apify-Scrapy integration. The main challenge is to combine two asynchronous frameworks that use different event loop implementations. Scrapy uses [Twisted](https://twisted.org/) for asynchronous execution, while the Apify SDK is based on [asyncio](https://docs.python.org/3/library/asyncio.html). The key thing is to install Twisted's `asyncioreactor` to run Twisted's asyncio compatible event loop. The `apify.scrapy.run_scrapy_actor` function handles this reactor installation automatically. This allows both Twisted and asyncio to run on a single event loop, enabling a Scrapy spider to run as an Apify Actor with minimal modifications.

\_\_main\_\_.py: The Actor entry point

```
from __future__ import annotations



import os



from apify.scrapy import initialize_logging, run_scrapy_actor



# Import your main Actor coroutine here.

from .main import main



# Point Scrapy at the settings module.

os.environ['SCRAPY_SETTINGS_MODULE'] = 'src.settings'





if __name__ == '__main__':

    initialize_logging()

    run_scrapy_actor(main())
```

In this setup, `apify.scrapy.initialize_logging` configures an Apify log formatter and reconfigures loggers to ensure consistent logging across Scrapy, the Apify SDK, and other libraries. The `apify.scrapy.run_scrapy_actor` installs Twisted's asyncio-compatible reactor and bridges asyncio coroutines with Twisted's reactor, enabling the Actor's main coroutine, which contains the Scrapy spider, to be executed.

Make sure the `SCRAPY_SETTINGS_MODULE` environment variable is set to the path of the Scrapy settings module. This variable is also used by the `Actor` class to detect that the project is a Scrapy project, triggering additional actions.

main.py: The Actor main coroutine

```
from __future__ import annotations



import asyncio



from scrapy.crawler import AsyncCrawlerRunner



from apify import Actor

from apify.scrapy import apply_apify_settings



# Import your Scrapy spider here.

from .spiders import TitleSpider as Spider





async def main() -> None:

    """Apify Actor main coroutine for executing the Scrapy spider."""

    async with Actor:

        # Read the Actor input.

        actor_input = await Actor.get_input() or {}

        start_urls = [url['url'] for url in actor_input.get('startUrls', [])]

        allowed_domains = actor_input.get('allowedDomains')

        proxy_config = actor_input.get('proxyConfiguration')



        # Apply Apify settings (override the Scrapy project settings).

        settings = apply_apify_settings(proxy_config=proxy_config)



        # Run the Scrapy spider.

        crawler_runner = AsyncCrawlerRunner(settings)

        await crawler_runner.crawl(

            Spider,

            start_urls=start_urls,

            allowed_domains=allowed_domains,

        )





if __name__ == '__main__':

    asyncio.run(main())
```

Within the Actor's main coroutine, the Actor's input is processed as usual. The function `apify.scrapy.apply_apify_settings` is then used to configure Scrapy settings with Apify-specific components before the spider is executed. The key components and other helper functions are described in the next section.

## Key integration components[](#key-integration-components)

The Apify SDK provides several custom components to support integration with the Apify platform:

* [`apify.scrapy.ApifyScheduler`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ApifyScheduler.md) - Replaces Scrapy's default [scheduler](https://docs.scrapy.org/en/latest/topics/scheduler.html) with one that uses Apify's [request queue](https://docs.apify.com/platform/storage/request-queue) for storing requests. It manages enqueuing, dequeuing, and maintaining the state and priority of requests.
* [`apify.scrapy.pipelines.ActorDatasetPushPipeline`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ActorDatasetPushPipeline.md) - A Scrapy [item pipeline](https://docs.scrapy.org/en/latest/topics/item-pipeline.html) that pushes scraped items to Apify's [dataset](https://docs.apify.com/platform/storage/dataset). When enabled, every item produced by the spider is sent to the dataset.
* [`apify.scrapy.middlewares.ApifyHttpProxyMiddleware`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ApifyHttpProxyMiddleware.md) - A Scrapy [middleware](https://docs.scrapy.org/en/latest/topics/downloader-middleware.html) that manages proxy configurations. This middleware replaces Scrapy's default `HttpProxyMiddleware` to facilitate the use of Apify's proxy service.
* [`apify.scrapy.extensions.ApifyCacheStorage`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ApifyCacheStorage.md) - A storage backend for Scrapy's built-in [HTTP cache middleware](https://docs.scrapy.org/en/latest/topics/downloader-middleware.html#module-scrapy.downloadermiddlewares.httpcache). This backend uses Apify's [key-value store](https://docs.apify.com/platform/storage/key-value-store). To enable caching, set `HTTPCACHE_ENABLED` and `HTTPCACHE_EXPIRATION_SECS` in your settings. By default, when the spider closes, up to 100 expired and unreadable entries per run are cleaned up. To change this number, update `APIFY_HTTPCACHE_EXPIRATION_MAX_ITEMS`.

Additional helper functions in the [`apify.scrapy`](https://github.com/apify/apify-sdk-python/tree/master/src/apify/scrapy) subpackage include:

* `apply_apify_settings` - Applies Apify-specific components to Scrapy settings.
* `to_apify_request` and `to_scrapy_request` - Convert between Apify and Scrapy request objects.
* `initialize_logging` - Configures logging for the Actor environment.
* `run_scrapy_actor` - Installs Twisted's asyncio reactor and bridges asyncio and Twisted event loops.

## Create a new Apify-Scrapy project[](#create-a-new-apify-scrapy-project)

The simplest way to start using Scrapy in Apify Actors is to use the [Scrapy Actor template](https://apify.com/templates/python-scrapy). The template provides a pre-configured project structure and setup that includes all necessary components to run Scrapy spiders as Actors and store their output in Apify datasets. If you prefer manual setup, refer to the example Actor section below for configuration details.

## Wrapping an existing Scrapy project[](#wrapping-an-existing-scrapy-project)

The Apify CLI supports converting an existing Scrapy project into an Apify Actor with a single command. The CLI expects the project to follow the standard Scrapy layout (including a `scrapy.cfg` file in the project root). During the wrapping process, the CLI:

* Creates the necessary files and directories for an Apify Actor.
* Installs the Apify SDK and required dependencies.
* Updates Scrapy settings to include Apify-specific components.

For further details, see the [Scrapy migration guide](https://docs.apify.com/cli/docs/integrating-scrapy).

## Example Actor[](#example-actor)

The following example shows a Scrapy Actor that scrapes page titles and enqueues links found on each page. This example aligns with the structure provided in the Apify Actor templates.

<!-- -->

* \_\_main\_\_.py
* main.py
* settings.py
* items.py
* spiders/title.py

```
from __future__ import annotations



import os



from apify.scrapy import initialize_logging, run_scrapy_actor



# Import your main Actor coroutine here.

from .main import main



# Point Scrapy at the settings module.

os.environ['SCRAPY_SETTINGS_MODULE'] = 'src.settings'





if __name__ == '__main__':

    initialize_logging()

    run_scrapy_actor(main())
```

```
from __future__ import annotations



import asyncio



from scrapy.crawler import AsyncCrawlerRunner



from apify import Actor

from apify.scrapy import apply_apify_settings



# Import your Scrapy spider here.

from .spiders import TitleSpider as Spider





async def main() -> None:

    """Apify Actor main coroutine for executing the Scrapy spider."""

    async with Actor:

        # Read the Actor input.

        actor_input = await Actor.get_input() or {}

        start_urls = [url['url'] for url in actor_input.get('startUrls', [])]

        allowed_domains = actor_input.get('allowedDomains')

        proxy_config = actor_input.get('proxyConfiguration')



        # Apply Apify settings (override the Scrapy project settings).

        settings = apply_apify_settings(proxy_config=proxy_config)



        # Run the Scrapy spider.

        crawler_runner = AsyncCrawlerRunner(settings)

        await crawler_runner.crawl(

            Spider,

            start_urls=start_urls,

            allowed_domains=allowed_domains,

        )





if __name__ == '__main__':

    asyncio.run(main())
```

```
BOT_NAME = 'titlebot'

DEPTH_LIMIT = 1

LOG_LEVEL = 'INFO'

NEWSPIDER_MODULE = 'src.spiders'

ROBOTSTXT_OBEY = True

SPIDER_MODULES = ['src.spiders']

TELNETCONSOLE_ENABLED = False

# Don't change the Twisted reactor unless you know what you're doing.

TWISTED_REACTOR = 'twisted.internet.asyncioreactor.AsyncioSelectorReactor'

HTTPCACHE_ENABLED = True

HTTPCACHE_EXPIRATION_SECS = 7200
```

```
from __future__ import annotations



from scrapy import Field, Item





class TitleItem(Item):

    """Represents a title item scraped from a web page."""



    url = Field()

    title = Field()
```

```
from __future__ import annotations



from typing import TYPE_CHECKING, Any

from urllib.parse import urljoin



from scrapy import Request, Spider



from ..items import TitleItem



if TYPE_CHECKING:

    from collections.abc import Generator



    from scrapy.http.response import Response





class TitleSpider(Spider):

    """A spider that extracts page titles and queues links for further crawling."""



    name = 'title_spider'



    # Limit the number of pages to scrape.

    custom_settings = {'CLOSESPIDER_PAGECOUNT': 10}



    def __init__(

        self,

        start_urls: list[str],

        allowed_domains: list[str],

        *args: Any,

        **kwargs: Any,

    ) -> None:

        """Store the start URLs and allowed domains."""

        super().__init__(*args, **kwargs)

        self.start_urls = start_urls

        self.allowed_domains = allowed_domains



    def parse(self, response: Response) -> Generator[TitleItem | Request, None, None]:

        """Yield a `TitleItem` and a `Request` for each link on the page."""

        self.logger.info('TitleSpider is parsing %s...', response)



        # Yield the title item.

        url = response.url

        title = response.css('title::text').extract_first()

        yield TitleItem(url=url, title=title)



        # Yield a request for each link.

        for link_href in response.css('a::attr("href")'):

            link_url = urljoin(response.url, link_href.get())

            if link_url.startswith(('http://', 'https://')):

                yield Request(link_url)
```

## Dealing with imminent migration to another host[](#dealing-with-imminent-migration-to-another-host)

Under some circumstances, the platform may decide to [migrate your Actor](https://docs.apify.com/academy/expert-scraping-with-apify/migrations-maintaining-state) from one piece of infrastructure to another while it's in progress. While [Crawlee](https://crawlee.dev/python)-based projects can pause and resume their work after a restart, achieving the same with a Scrapy-based project can be challenging.

As a workaround for this issue (tracked as [apify/actor-templates#303](https://github.com/apify/actor-templates/issues/303)), turn on caching with `HTTPCACHE_ENABLED` and set `HTTPCACHE_EXPIRATION_SECS` to at least a few minutes—the exact value depends on your use case. If your Actor gets migrated and restarted, the subsequent run will hit the cache, making it fast and avoiding unnecessary resource consumption.

## Conclusion[](#conclusion)

In this guide you learned how to use Scrapy in Apify Actors. You can now start building your own web scraping projects using Scrapy, the Apify SDK and host them on the Apify platform. See the [Actor templates](https://apify.com/templates/categories/python) to get started with your own scraping tasks. If you have questions or need assistance, feel free to reach out on our [GitHub](https://github.com/apify/apify-sdk-python) or join our [Discord community](https://discord.com/invite/jyEM2PRvMU). Happy scraping!

## Additional resources[](#additional-resources)

* [Apify CLI: Integrating Scrapy projects](https://docs.apify.com/cli/docs/integrating-scrapy)
* [Apify: Run Scrapy spiders on Apify](https://apify.com/run-scrapy-in-cloud)
* [Apify templates: Python Actor Scrapy template](https://apify.com/templates/python-scrapy)
* [Apify store: Scrapy Books Example Actor](https://apify.com/vdusek/scrapy-books-example)
* [Scrapy: Official documentation](https://docs.scrapy.org/)
