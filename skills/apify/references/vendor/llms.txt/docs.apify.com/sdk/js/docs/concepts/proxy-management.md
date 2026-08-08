# Proxy management

Copy for LLM

[IP address blocking](https://en.wikipedia.org/wiki/IP_address_blocking) is one of the oldest and most effective ways of preventing access to a website. It is therefore paramount for a good web scraping library to provide easy to use but powerful tools which can work around IP blocking. The most powerful weapon in your anti IP blocking arsenal is a [proxy server](https://en.wikipedia.org/wiki/Proxy_server).

With Apify SDK you can use your own proxy servers, proxy servers acquired from third-party providers, or you can rely on [Apify Proxy](https://apify.com/proxy) for your scraping needs.

## Quick start[](#quick-start)

If you already subscribed to Apify Proxy or have proxy URLs of your own, you can start using them immediately in only a few lines of code.

Authentication required

If you want to use Apify Proxy, make sure that your [scraper is logged in](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/actor-lifecycle.md).

```
const proxyConfiguration = await Actor.createProxyConfiguration();

const proxyUrl = proxyConfiguration.newUrl();
```

```
const proxyConfiguration = await Actor.createProxyConfiguration({

    proxyUrls: ['http://proxy-1.com', 'http://proxy-2.com'],

});

const proxyUrl = proxyConfiguration.newUrl();
```

## Proxy Configuration[](#proxy-configuration)

All your proxy needs are managed by the [`ProxyConfiguration`](https://docs.apify.com/sdk/js/sdk/js/reference/class/ProxyConfiguration.md) class. You create an instance using the [`Actor.createProxyConfiguration()`](https://docs.apify.com/sdk/js/sdk/js/reference/class/Actor.md#createProxyConfiguration) function. See the [`ProxyConfigurationOptions`](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ProxyConfigurationOptions.md) for all the possible constructor options.

### Crawler integration[](#crawler-integration)

`ProxyConfiguration` integrates seamlessly into [`CheerioCrawler`](https://crawlee.dev/api/cheerio-crawler/class/CheerioCrawler) and [`PuppeteerCrawler`](https://crawlee.dev/api/puppeteer-crawler/class/PuppeteerCrawler).

```
const proxyConfiguration = await Actor.createProxyConfiguration({

    /* your proxy opts */

});

const crawler = new CheerioCrawler({

    proxyConfiguration,

    // ...

});
```

```
const proxyConfiguration = await Actor.createProxyConfiguration({

    /* your proxy opts */

});

const crawler = new PuppeteerCrawler({

    proxyConfiguration,

    // ...

});
```

Your crawlers will now use the selected proxies for all connections.

### IP Rotation and session management[](#ip-rotation-and-session-management)

[`proxyConfiguration.newUrl()`](https://docs.apify.com/sdk/js/sdk/js/reference/class/ProxyConfiguration.md#newUrl)

allows you to pass a `sessionId` parameter. It will then be used to create a `sessionId`-`proxyUrl` pair, and subsequent `newUrl()` calls with the same `sessionId` will always return the same `proxyUrl`. This is extremely useful in scraping, because you want to create the impression of a real user. See the [session management guide](https://docs.apify.com/sdk/js/sdk/js/docs/concepts/session-management.md) and [`SessionPool`](https://crawlee.dev/api/core/class/SessionPool) class for more information on how keeping a real session helps you avoid blocking.

When no `sessionId` is provided, your proxy URLs are rotated round-robin, whereas Apify Proxy manages their rotation using black magic to get the best performance.

```
const proxyConfiguration = await Actor.createProxyConfiguration({

    /* opts */

});

const sessionPool = await SessionPool.open({

    /* opts */

});

const session = await sessionPool.getSession();

const proxyUrl = proxyConfiguration.newUrl(session.id);
```

```
const proxyConfiguration = await Actor.createProxyConfiguration({

    /* opts */

});

const crawler = new PuppeteerCrawler({

    useSessionPool: true,

    persistCookiesPerSession: true,

    proxyConfiguration,

    // ...

});
```

## Apify Proxy vs. Your own proxies[](#apify-proxy-vs-your-own-proxies)

The `ProxyConfiguration` class covers both Apify Proxy and custom proxy URLs so that you can easily switch between proxy providers, however, some features of the class are available only to Apify Proxy users, mainly because Apify Proxy is what one would call a super-proxy. It's not a single proxy server, but an API endpoint that allows connection through millions of different IP addresses. So the class essentially has two modes: Apify Proxy or Your proxy.

The difference is easy to remember. [`ProxyConfigurationOptions.proxyUrls`](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ProxyConfigurationOptions.md#proxyUrls) and [`ProxyConfigurationOptions.newUrlFunction`](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ProxyConfigurationOptions.md#newUrlFunction) enable use of your custom proxy URLs, whereas all the other options are there to configure Apify Proxy. Visit the [Apify Proxy docs](https://docs.apify.com/proxy) for more info on how these parameters work.

## Apify Proxy Configuration[](#apify-proxy-configuration)

With Apify Proxy, you can select specific proxy groups to use, or countries to connect from. This allows you to get better proxy performance after some initial research.

```
const proxyConfiguration = await Actor.createProxyConfiguration({

    groups: ['RESIDENTIAL'],

    countryCode: 'US',

});

const proxyUrl = proxyConfiguration.newUrl();
```

Now your crawlers will use only Residential proxies from the US. Note that you must first get access to a proxy group before you are able to use it. You can find your available proxy groups in the [proxy dashboard](https://console.apify.com/proxy).

## Inspecting current proxy in Crawlers[](#inspecting-current-proxy-in-crawlers)

`CheerioCrawler` and `PuppeteerCrawler` grant access to information about the currently used proxy in their `handlePageFunction` using a [`proxyInfo`](https://docs.apify.com/sdk/js/sdk/js/reference/interface/ProxyInfo.md) object. With the object, you can easily access the proxy URL. If you're using Apify Proxy, the other configuration parameters will also be available in the `proxyInfo` object.
