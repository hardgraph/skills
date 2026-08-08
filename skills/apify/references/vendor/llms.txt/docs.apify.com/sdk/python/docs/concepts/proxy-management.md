# Proxy management

Copy for LLM

The Apify SDK provides built-in proxy management through the [`ProxyConfiguration`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ProxyConfiguration.md) class, supporting both [Apify Proxy](https://apify.com/proxy) and custom proxy servers. Proxies are essential for web scraping to avoid [IP address blocking](https://en.wikipedia.org/wiki/IP_address_blocking) and distribute requests across multiple addresses.

## Quick start[](#quick-start)

If you want to use Apify Proxy locally, make sure that you run your Actors via the Apify CLI and that you are [logged in](https://docs.apify.com/cli/docs/installation#login-with-your-apify-account) with your Apify account in the CLI.

### Using Apify Proxy[](#using-apify-proxy)

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbigpXFxuXFxuICAgICAgICBpZiBub3QgcHJveHlfY2ZnOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignTm8gcHJveHkgY29uZmlndXJhdGlvbiBhdmFpbGFibGUuJylcXG5cXG4gICAgICAgIHByb3h5X3VybCA9IGF3YWl0IHByb3h5X2NmZy5uZXdfdXJsKClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnVXNpbmcgcHJveHkgVVJMOiB7cHJveHlfdXJsfScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.-vrwZ-v-FAYHc0OdltCo21oURVB8jsmDITGNz-XXqnM\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration()



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()

        Actor.log.info(f'Using proxy URL: {proxy_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

### Using your own proxies[](#using-your-own-proxies)

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbihcXG4gICAgICAgICAgICBwcm94eV91cmxzPVtcXG4gICAgICAgICAgICAgICAgJ2h0dHA6Ly9wcm94eS0xLmNvbScsXFxuICAgICAgICAgICAgICAgICdodHRwOi8vcHJveHktMi5jb20nLFxcbiAgICAgICAgICAgIF0sXFxuICAgICAgICApXFxuXFxuICAgICAgICBpZiBub3QgcHJveHlfY2ZnOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignTm8gcHJveHkgY29uZmlndXJhdGlvbiBhdmFpbGFibGUuJylcXG5cXG4gICAgICAgIHByb3h5X3VybCA9IGF3YWl0IHByb3h5X2NmZy5uZXdfdXJsKClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnVXNpbmcgcHJveHkgVVJMOiB7cHJveHlfdXJsfScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.Bs6h_TDfILAjgBGcHSBpm5cGYhFQxjmRrq0GTQ8NcAc\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration(

            proxy_urls=[

                'http://proxy-1.com',

                'http://proxy-2.com',

            ],

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()

        Actor.log.info(f'Using proxy URL: {proxy_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Proxy configuration[](#proxy-configuration)

All your proxy needs are managed by the [`ProxyConfiguration`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ProxyConfiguration.md) class. You create an instance using the [`Actor.create_proxy_configuration()`](https://docs.apify.com/sdk/python/sdk/python/reference/class/Actor.md#create_proxy_configuration) method. Then you generate proxy URLs using the [`ProxyConfiguration.new_url()`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ProxyConfiguration.md#new_url) method.

### Apify Proxy vs. your own proxies[](#apify-proxy-vs-your-own-proxies)

The `ProxyConfiguration` class covers both Apify Proxy and custom proxy URLs, so that you can easily switch between proxy providers. However, some features of the class are available only to Apify Proxy users, mainly because Apify Proxy is what one would call a super-proxy. It's not a single proxy server, but an API endpoint that allows connection through millions of different IP addresses. So the class essentially has two modes: Apify Proxy or Your proxy.

The difference is easy to remember. Using the `proxy_urls` or `new_url_function` arguments enables use of your custom proxy URLs, whereas all the other options are there to configure Apify Proxy. Visit the [Apify Proxy docs](https://docs.apify.com/proxy) for more info on how these parameters work.

### IP rotation and session management[](#ip-rotation-and-session-management)

`ProxyConfiguration.new_url` allows you to pass a `session_id` parameter. It will then be used to create a `session_id`-`proxy_url` pair, and subsequent `new_url()` calls with the same `session_id` will always return the same `proxy_url`. This is extremely useful in scraping, because you want to create the impression of a real user.

When no `session_id` is provided, your custom proxy URLs are rotated round-robin, whereas Apify Proxy manages their rotation using black magic to get the best performance.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbihcXG4gICAgICAgICAgICBwcm94eV91cmxzPVtcXG4gICAgICAgICAgICAgICAgJ2h0dHA6Ly9wcm94eS0xLmNvbScsXFxuICAgICAgICAgICAgICAgICdodHRwOi8vcHJveHktMi5jb20nLFxcbiAgICAgICAgICAgIF0sXFxuICAgICAgICApXFxuXFxuICAgICAgICBpZiBub3QgcHJveHlfY2ZnOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignTm8gcHJveHkgY29uZmlndXJhdGlvbiBhdmFpbGFibGUuJylcXG5cXG4gICAgICAgIHByb3h5X3VybCA9IGF3YWl0IHByb3h5X2NmZy5uZXdfdXJsKCkgICMgaHR0cDovL3Byb3h5LTEuY29tXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybCgpICAjIGh0dHA6Ly9wcm94eS0yLmNvbVxcbiAgICAgICAgcHJveHlfdXJsID0gYXdhaXQgcHJveHlfY2ZnLm5ld191cmwoKSAgIyBodHRwOi8vcHJveHktMS5jb21cXG4gICAgICAgIHByb3h5X3VybCA9IGF3YWl0IHByb3h5X2NmZy5uZXdfdXJsKCkgICMgaHR0cDovL3Byb3h5LTIuY29tXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybChzZXNzaW9uX2lkPSdhJykgICMgaHR0cDovL3Byb3h5LTEuY29tXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybChzZXNzaW9uX2lkPSdiJykgICMgaHR0cDovL3Byb3h5LTIuY29tXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybChzZXNzaW9uX2lkPSdiJykgICMgaHR0cDovL3Byb3h5LTIuY29tXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybChzZXNzaW9uX2lkPSdhJykgICMgaHR0cDovL3Byb3h5LTEuY29tXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.unLTB5-qLlaD52dDzm7o4cMz6eMWRD9Ryn4j9WBJzis\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration(

            proxy_urls=[

                'http://proxy-1.com',

                'http://proxy-2.com',

            ],

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()  # http://proxy-1.com

        proxy_url = await proxy_cfg.new_url()  # http://proxy-2.com

        proxy_url = await proxy_cfg.new_url()  # http://proxy-1.com

        proxy_url = await proxy_cfg.new_url()  # http://proxy-2.com

        proxy_url = await proxy_cfg.new_url(session_id='a')  # http://proxy-1.com

        proxy_url = await proxy_cfg.new_url(session_id='b')  # http://proxy-2.com

        proxy_url = await proxy_cfg.new_url(session_id='b')  # http://proxy-2.com

        proxy_url = await proxy_cfg.new_url(session_id='a')  # http://proxy-1.com





if __name__ == '__main__':

    asyncio.run(main())
```

### Apify Proxy configuration[](#apify-proxy-configuration)

With Apify Proxy, you can select specific proxy groups to use, or countries to connect from. For even finer control, you can also target a specific subdivision (e.g. a US state) using the `subdivision_code` parameter alongside `country_code`. This allows you to get better proxy performance after some initial research.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbihcXG4gICAgICAgICAgICBncm91cHM9WydSRVNJREVOVElBTCddLFxcbiAgICAgICAgICAgIGNvdW50cnlfY29kZT0nVVMnLFxcbiAgICAgICAgICAgIHN1YmRpdmlzaW9uX2NvZGU9J0NBJyxcXG4gICAgICAgIClcXG5cXG4gICAgICAgIGlmIG5vdCBwcm94eV9jZmc6XFxuICAgICAgICAgICAgcmFpc2UgUnVudGltZUVycm9yKCdObyBwcm94eSBjb25maWd1cmF0aW9uIGF2YWlsYWJsZS4nKVxcblxcbiAgICAgICAgcHJveHlfdXJsID0gYXdhaXQgcHJveHlfY2ZnLm5ld191cmwoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidQcm94eSBVUkw6IHtwcm94eV91cmx9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.LwLlH1sGkiqcMxcsBQqEgYrLfvOelaJPf5DcXrVY3kA\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration(

            groups=['RESIDENTIAL'],

            country_code='US',

            subdivision_code='CA',

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()

        Actor.log.info(f'Proxy URL: {proxy_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

Now your connections using proxy\_url will use only Residential proxies from California, US. The `subdivision_code` accepts a 1–3 character ISO 3166-2 code (e.g. `CA` for California) and currently only works for the United States (`country_code='US'`). Note that you must first get access to a proxy group before you are able to use it. You can find your available proxy groups in the [proxy dashboard](https://console.apify.com/proxy).

If you don't specify any proxy groups, automatic proxy selection will be used.

### Your own proxy configuration[](#your-own-proxy-configuration)

There are two options how to make `ProxyConfiguration` work with your own proxies.

Either you can pass it a list of your own proxy servers:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbihcXG4gICAgICAgICAgICBwcm94eV91cmxzPVtcXG4gICAgICAgICAgICAgICAgJ2h0dHA6Ly9wcm94eS0xLmNvbScsXFxuICAgICAgICAgICAgICAgICdodHRwOi8vcHJveHktMi5jb20nLFxcbiAgICAgICAgICAgIF0sXFxuICAgICAgICApXFxuXFxuICAgICAgICBpZiBub3QgcHJveHlfY2ZnOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignTm8gcHJveHkgY29uZmlndXJhdGlvbiBhdmFpbGFibGUuJylcXG5cXG4gICAgICAgIHByb3h5X3VybCA9IGF3YWl0IHByb3h5X2NmZy5uZXdfdXJsKClcXG4gICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnVXNpbmcgcHJveHkgVVJMOiB7cHJveHlfdXJsfScpXFxuXFxuXFxuaWYgX19uYW1lX18gPT0gJ19fbWFpbl9fJzpcXG4gICAgYXN5bmNpby5ydW4obWFpbigpKVxcblwifSIsIm9wdGlvbnMiOnsiYnVpbGQiOiJsYXRlc3QiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uL2pzb247IGNoYXJzZXQ9dXRmLTgiLCJtZW1vcnkiOjEwMjQsInRpbWVvdXQiOjE4MH19.Bs6h_TDfILAjgBGcHSBpm5cGYhFQxjmRrq0GTQ8NcAc\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration(

            proxy_urls=[

                'http://proxy-1.com',

                'http://proxy-2.com',

            ],

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()

        Actor.log.info(f'Using proxy URL: {proxy_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

Or you can pass it a method (accepting one optional argument, the session ID), to generate proxy URLs automatically:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImZyb20gX19mdXR1cmVfXyBpbXBvcnQgYW5ub3RhdGlvbnNcXG5cXG5pbXBvcnQgYXN5bmNpb1xcblxcbmZyb20gYXBpZnkgaW1wb3J0IEFjdG9yLCBSZXF1ZXN0XFxuXFxuXFxuYXN5bmMgZGVmIGN1c3RvbV9uZXdfdXJsX2Z1bmN0aW9uKFxcbiAgICBzZXNzaW9uX2lkOiBzdHIgfCBOb25lID0gTm9uZSxcXG4gICAgcmVxdWVzdDogUmVxdWVzdCB8IE5vbmUgPSBOb25lLFxcbikgLT4gc3RyIHwgTm9uZTpcXG4gICAgIyBQaWNrIGEgcHJveHkgVVJMIGJhc2VkIG9uIHRoZSBzZXNzaW9uIGFuZC9vciB0aGUgcmVxdWVzdCBiZWluZyBwcm94aWVkLlxcbiAgICBpZiByZXF1ZXN0IGlzIG5vdCBOb25lOlxcbiAgICAgICAgQWN0b3IubG9nLmRlYnVnKGYnU2VsZWN0aW5nIGEgcHJveHkgVVJMIGZvciB7cmVxdWVzdC51cmx9LicpXFxuICAgIGlmIHNlc3Npb25faWQgaXMgbm90IE5vbmU6XFxuICAgICAgICByZXR1cm4gZidodHRwOi8vbXktY3VzdG9tLXByb3h5LXN1cHBvcnRpbmctc2Vzc2lvbnMuY29tP3Nlc3Npb24taWQ9e3Nlc3Npb25faWR9J1xcbiAgICByZXR1cm4gJ2h0dHA6Ly9teS1jdXN0b20tcHJveHktbm90LXN1cHBvcnRpbmctc2Vzc2lvbnMuY29tJ1xcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgYXN5bmMgd2l0aCBBY3RvcjpcXG4gICAgICAgIHByb3h5X2NmZyA9IGF3YWl0IEFjdG9yLmNyZWF0ZV9wcm94eV9jb25maWd1cmF0aW9uKFxcbiAgICAgICAgICAgIG5ld191cmxfZnVuY3Rpb249Y3VzdG9tX25ld191cmxfZnVuY3Rpb24sXFxuICAgICAgICApXFxuXFxuICAgICAgICBpZiBub3QgcHJveHlfY2ZnOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignTm8gcHJveHkgY29uZmlndXJhdGlvbiBhdmFpbGFibGUuJylcXG5cXG4gICAgICAgIHByb3h5X3VybF93aXRoX3Nlc3Npb24gPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybCgnYScpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1VzaW5nIHByb3h5IFVSTDoge3Byb3h5X3VybF93aXRoX3Nlc3Npb259JylcXG5cXG4gICAgICAgIHByb3h5X3VybF93aXRob3V0X3Nlc3Npb24gPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybCgpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1VzaW5nIHByb3h5IFVSTDoge3Byb3h5X3VybF93aXRob3V0X3Nlc3Npb259JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.AjQyoahlJuhJzL7Cu5e6_iXiK_PbEEIgXO9CVFLvhbE\&asrc=run_on_apify)

```
from __future__ import annotations



import asyncio



from apify import Actor, Request





async def custom_new_url_function(

    session_id: str | None = None,

    request: Request | None = None,

) -> str | None:

    # Pick a proxy URL based on the session and/or the request being proxied.

    if request is not None:

        Actor.log.debug(f'Selecting a proxy URL for {request.url}.')

    if session_id is not None:

        return f'http://my-custom-proxy-supporting-sessions.com?session-id={session_id}'

    return 'http://my-custom-proxy-not-supporting-sessions.com'





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration(

            new_url_function=custom_new_url_function,

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url_with_session = await proxy_cfg.new_url('a')

        Actor.log.info(f'Using proxy URL: {proxy_url_with_session}')



        proxy_url_without_session = await proxy_cfg.new_url()

        Actor.log.info(f'Using proxy URL: {proxy_url_without_session}')





if __name__ == '__main__':

    asyncio.run(main())
```

### Tiered proxy rotation[](#tiered-proxy-rotation)

[`ProxyConfiguration`](https://docs.apify.com/sdk/python/sdk/python/reference/class/ProxyConfiguration.md) supports tiered proxy URLs via the `tiered_proxy_urls` parameter. This accepts a list of lists of proxy URLs, where each inner list represents a tier. The proxy rotator starts with the first (cheapest) tier and automatically escalates to higher tiers when lower-tier proxies get blocked. This is useful for optimizing proxy costs — you use cheap datacenter proxies for most requests and only switch to expensive residential proxies when necessary.

info

The `tiered_proxy_urls` parameter is only available when constructing `ProxyConfiguration` directly. It is not supported by `Actor.create_proxy_configuration()`.

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3IsIFByb3h5Q29uZmlndXJhdGlvblxcblxcblxcbmFzeW5jIGRlZiBtYWluKCkgLT4gTm9uZTpcXG4gICAgYXN5bmMgd2l0aCBBY3RvcjpcXG4gICAgICAgICMgQ3JlYXRlIGEgcHJveHkgY29uZmlndXJhdGlvbiB3aXRoIHRpZXJlZCBwcm94eSBVUkxzLlxcbiAgICAgICAgIyBUaGUgcHJveHkgcm90YXRvciBzdGFydHMgd2l0aCB0aGUgY2hlYXBlc3QgdGllciBhbmQgZXNjYWxhdGVzIGFzIG5lZWRlZC5cXG4gICAgICAgIHByb3h5X2NvbmZpZ3VyYXRpb24gPSBQcm94eUNvbmZpZ3VyYXRpb24oXFxuICAgICAgICAgICAgdGllcmVkX3Byb3h5X3VybHM9W1xcbiAgICAgICAgICAgICAgICAjIFRpZXIgMDogY2hlYXAgZGF0YWNlbnRlciBwcm94aWVzLCB0cmllZCBmaXJzdFxcbiAgICAgICAgICAgICAgICBbJ2h0dHA6Ly9kYXRhY2VudGVyLXByb3h5LTE6ODA4MCcsICdodHRwOi8vZGF0YWNlbnRlci1wcm94eS0yOjgwODAnXSxcXG4gICAgICAgICAgICAgICAgIyBUaWVyIDE6IHJlc2lkZW50aWFsIHByb3hpZXMsIHVzZWQgd2hlbiB0aWVyIDAgZ2V0cyBibG9ja2VkXFxuICAgICAgICAgICAgICAgIFsnaHR0cDovL3Jlc2lkZW50aWFsLXByb3h5LTE6ODA4MCcsICdodHRwOi8vcmVzaWRlbnRpYWwtcHJveHktMjo4MDgwJ10sXFxuICAgICAgICAgICAgXSxcXG4gICAgICAgIClcXG5cXG4gICAgICAgIGF3YWl0IHByb3h5X2NvbmZpZ3VyYXRpb24uaW5pdGlhbGl6ZSgpXFxuXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jb25maWd1cmF0aW9uLm5ld191cmwoKVxcbiAgICAgICAgQWN0b3IubG9nLmluZm8oZidVc2luZyBwcm94eSBVUkw6IHtwcm94eV91cmx9JylcXG5cXG5cXG5pZiBfX25hbWVfXyA9PSAnX19tYWluX18nOlxcbiAgICBhc3luY2lvLnJ1bihtYWluKCkpXFxuXCJ9Iiwib3B0aW9ucyI6eyJidWlsZCI6ImxhdGVzdCIsImNvbnRlbnRUeXBlIjoiYXBwbGljYXRpb24vanNvbjsgY2hhcnNldD11dGYtOCIsIm1lbW9yeSI6MTAyNCwidGltZW91dCI6MTgwfX0.6EJt7f9xtsPrWSWQKBC263Md0eWL3cC0DYHvOfdtNFk\&asrc=run_on_apify)

```
import asyncio



from apify import Actor, ProxyConfiguration





async def main() -> None:

    async with Actor:

        # Create a proxy configuration with tiered proxy URLs.

        # The proxy rotator starts with the cheapest tier and escalates as needed.

        proxy_configuration = ProxyConfiguration(

            tiered_proxy_urls=[

                # Tier 0: cheap datacenter proxies, tried first

                ['http://datacenter-proxy-1:8080', 'http://datacenter-proxy-2:8080'],

                # Tier 1: residential proxies, used when tier 0 gets blocked

                ['http://residential-proxy-1:8080', 'http://residential-proxy-2:8080'],

            ],

        )



        await proxy_configuration.initialize()



        proxy_url = await proxy_configuration.new_url()

        Actor.log.info(f'Using proxy URL: {proxy_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

### Configuring proxy based on Actor input[](#configuring-proxy-based-on-actor-input)

To make selecting the proxies that the Actor uses easier, you can use an input field with the editor [`proxy` in your input schema](https://docs.apify.com/platform/actors/development/input-schema#object). This input will then be filled with a dictionary containing the proxy settings you or the users of your Actor selected for the Actor run.

You can then use that input to create the proxy configuration:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBhY3Rvcl9pbnB1dCA9IGF3YWl0IEFjdG9yLmdldF9pbnB1dCgpIG9yIHt9XFxuICAgICAgICBwcm94eV9zZXR0aW5ncyA9IGFjdG9yX2lucHV0LmdldCgncHJveHlTZXR0aW5ncycpXFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbihcXG4gICAgICAgICAgICBhY3Rvcl9wcm94eV9pbnB1dD1wcm94eV9zZXR0aW5nc1xcbiAgICAgICAgKVxcblxcbiAgICAgICAgaWYgbm90IHByb3h5X2NmZzpcXG4gICAgICAgICAgICByYWlzZSBSdW50aW1lRXJyb3IoJ05vIHByb3h5IGNvbmZpZ3VyYXRpb24gYXZhaWxhYmxlLicpXFxuXFxuICAgICAgICBwcm94eV91cmwgPSBhd2FpdCBwcm94eV9jZmcubmV3X3VybCgpXFxuICAgICAgICBBY3Rvci5sb2cuaW5mbyhmJ1VzaW5nIHByb3h5IFVSTDoge3Byb3h5X3VybH0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.vmqGfC71vRwsi56tPanlENNRygASiuvOmwJgMZCWT9k\&asrc=run_on_apify)

```
import asyncio



from apify import Actor





async def main() -> None:

    async with Actor:

        actor_input = await Actor.get_input() or {}

        proxy_settings = actor_input.get('proxySettings')

        proxy_cfg = await Actor.create_proxy_configuration(

            actor_proxy_input=proxy_settings

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()

        Actor.log.info(f'Using proxy URL: {proxy_url}')





if __name__ == '__main__':

    asyncio.run(main())
```

## Using the generated proxy URLs[](#using-the-generated-proxy-urls)

`ProxyConfiguration` only generates proxy URLs. It doesn't make requests. To route requests through the proxy, pass a generated URL to the HTTP client your Actor uses.

### HTTPX[](#httpx)

To use the generated proxy URLs with the `httpx` library, use the [`proxy`](https://www.python-httpx.org/advanced/proxies/) argument:

[Run on](https://console.apify.com/actors/HH9rhkFXiZbheuq1V?runConfig=eyJ1IjoiRWdQdHczb2VqNlRhRHQ1cW4iLCJ2IjoxfQ.eyJpbnB1dCI6IntcImNvZGVcIjpcImltcG9ydCBhc3luY2lvXFxuXFxuaW1wb3J0IGh0dHB4XFxuXFxuZnJvbSBhcGlmeSBpbXBvcnQgQWN0b3JcXG5cXG5cXG5hc3luYyBkZWYgbWFpbigpIC0-IE5vbmU6XFxuICAgIGFzeW5jIHdpdGggQWN0b3I6XFxuICAgICAgICBwcm94eV9jZmcgPSBhd2FpdCBBY3Rvci5jcmVhdGVfcHJveHlfY29uZmlndXJhdGlvbihcXG4gICAgICAgICAgICBwcm94eV91cmxzPVtcXG4gICAgICAgICAgICAgICAgJ2h0dHA6Ly9wcm94eS0xLmNvbScsXFxuICAgICAgICAgICAgICAgICdodHRwOi8vcHJveHktMi5jb20nLFxcbiAgICAgICAgICAgIF0sXFxuICAgICAgICApXFxuXFxuICAgICAgICBpZiBub3QgcHJveHlfY2ZnOlxcbiAgICAgICAgICAgIHJhaXNlIFJ1bnRpbWVFcnJvcignTm8gcHJveHkgY29uZmlndXJhdGlvbiBhdmFpbGFibGUuJylcXG5cXG4gICAgICAgIHByb3h5X3VybCA9IGF3YWl0IHByb3h5X2NmZy5uZXdfdXJsKClcXG5cXG4gICAgICAgIGFzeW5jIHdpdGggaHR0cHguQXN5bmNDbGllbnQocHJveHk9cHJveHlfdXJsKSBhcyBodHRweF9jbGllbnQ6XFxuICAgICAgICAgICAgcmVzcG9uc2UgPSBhd2FpdCBodHRweF9jbGllbnQuZ2V0KCdodHRwOi8vZXhhbXBsZS5jb20nKVxcbiAgICAgICAgICAgIEFjdG9yLmxvZy5pbmZvKGYnUmVzcG9uc2U6IHtyZXNwb25zZX0nKVxcblxcblxcbmlmIF9fbmFtZV9fID09ICdfX21haW5fXyc6XFxuICAgIGFzeW5jaW8ucnVuKG1haW4oKSlcXG5cIn0iLCJvcHRpb25zIjp7ImJ1aWxkIjoibGF0ZXN0IiwiY29udGVudFR5cGUiOiJhcHBsaWNhdGlvbi9qc29uOyBjaGFyc2V0PXV0Zi04IiwibWVtb3J5IjoxMDI0LCJ0aW1lb3V0IjoxODB9fQ.hSmdDQBnFOxt6ZQDwySpeK-W0lcjJKntz-45znPSgWM\&asrc=run_on_apify)

```
import asyncio



import httpx



from apify import Actor





async def main() -> None:

    async with Actor:

        proxy_cfg = await Actor.create_proxy_configuration(

            proxy_urls=[

                'http://proxy-1.com',

                'http://proxy-2.com',

            ],

        )



        if not proxy_cfg:

            raise RuntimeError('No proxy configuration available.')



        proxy_url = await proxy_cfg.new_url()



        async with httpx.AsyncClient(proxy=proxy_url) as httpx_client:

            response = await httpx_client.get('http://example.com')

            Actor.log.info(f'Response: {response}')





if __name__ == '__main__':

    asyncio.run(main())
```

Make sure you have the `httpx` library installed:

```
pip install httpx
```
